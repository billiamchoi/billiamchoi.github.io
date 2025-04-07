---
layout: post
title: Azure Terraform으로 Redhat 인프라 구축 자동화하기
description: Azure Terraform으로 Redhat 인프라 구축 자동화하기
date: 2025-04-07T09:27:00 +09:00
categories: develop
keyword:
  - Azure
  - Terraform
tags:
  - Back-End
img-tag: study
---
# Azure Terraform

---

### 목차

#### 1. 필요사항

#### 2. Azure CLI 및 Terraform 설치

#### 3. Terraform 코드 구현

#### 4. Terraform 초기화

#### 5. Terraform 실행 계획 만들기

#### 6. Terraform 실행 계획 적용

#### 7. private-key 기반 pem key 생성

#### 8. MobaXterm을 통해 원격 서버 접속

#### n. References

---

## 1. 필요사항

- Azure CLI
- Terraform

## 2. Azure CLI 및 Terraform 설치

### 2.1. Azure CLI 설치

[Azure CLI 다운로드 링크](https://learn.microsoft.com/ko-kr/cli/azure/install-azure-cli-windows?pivots=msi)

### 2.2. Azure CLI 로그인

```bash
$ az login # Azure CLI 로그인
$ az account show  # 로그인 확인
```

### 2.3. Chocolatey 설치 (Windows 용 패키지 관리자)

1. 파워쉘을 실행하여 아래 명령어를 실행한다.

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### 2.4. Terraform 설치

```bash
choco install terraform
```

## 3. Terraform 코드 구현

- Terraform 코드는 /terraform 위치에 정리되어 있습니다
- Terraform은 .tf 확장자를 가진 모든 파일을 자동으로 읽어서 하나로 합칩니다.

1. Terraform 코드를 테스트할 디렉토리를 만들고, 이를 현재 디렉토리로 만듭니다.

```bash
$ mkdir terraform
$ cd terraform
```

### 3.1. provider 코드 생성

1. providers.tf라는 파일을 만들고 다음 코드를 삽입합니다.

- Provider란?
  - upstream API의 logical abstraction입니다. 이들은 API 상호작용을 이해하고 resource를 노출하는 역할을 합니다.
    - 클라우드 플랫폼(Azure 등)과 통신하는 방법을 알고 있습니다.
    - Terraform 코드를 실제 API 호출로 변환합니다.
    - Terraform이 실제 클라우드 리소스를 생성, 읽기, 업데이트 및 삭제하도록 돕습니다.

```bash
terraform {
  # Terraform의 최소 요구 버전 설정 (0.12 이상 필요)
  required_version = ">=0.12"

  required_providers {
    azapi = {
      # azapi provider: Azure 리소스를 더 유연하게 제어하기 위해 사용
      source  = "azure/azapi"
      version = "~>1.5"  # 1.5버전대 사용
    }
    azurerm = {
      # azurerm provider: Azure 리소스를 관리하는 데 사용
      source  = "hashicorp/azurerm"
      version = "~>3.0"  # 3.0버전대 사용
    }
    random = {
      # random provider: 랜덤 문자열, 숫자 등을 생성할 때 사용
      source  = "hashicorp/random"
      version = "~>3.0"  # 3.0버전대 사용
    }
  }
}

# Azure 리소스를 관리하기 위한 azurerm provider 설정
provider "azurerm" {
  features {}  # 기본 기능 활성화 (필수로 선언해야 함)
}
```

### 3.2. ssh 코드 생성

1. ssh.tf라는 파일을 만들고 다음 코드를 삽입합니다.

```bash
# 랜덤한 문자열을 생성하여 SSH 키 이름에 사용할 접두어로 사용
resource "random_pet" "ssh_key_name" {
  prefix    = "ssh"    # 생성된 이름 앞에 붙을 접두어
  separator = ""       # 단어들 사이에 구분자 없음 (예: sshFuzzyLion)
}

# Azure의 SSH Public Key 리소스를 정의 (먼저 리소스를 생성)
resource "azapi_resource" "ssh_public_key" {
  type      = "Microsoft.Compute/sshPublicKeys@2022-11-01"  # 리소스 타입 및 API 버전
  name      = random_pet.ssh_key_name.id                    # 생성된 랜덤 이름을 사용
  location  = azurerm_resource_group.rg.location            # 리소스 그룹의 위치
  parent_id = azurerm_resource_group.rg.id                  # 소속된 리소스 그룹 ID
}

# 위에서 만든 SSH 키 리소스에 대해 KeyPair를 생성하는 액션 수행
resource "azapi_resource_action" "ssh_public_key_gen" {
  type        = "Microsoft.Compute/sshPublicKeys@2022-11-01" # 동일한 리소스 타입
  resource_id = azapi_resource.ssh_public_key.id             # 대상 리소스 ID
  action      = "generateKeyPair"                            # 실행할 액션: 키 쌍 생성
  method      = "POST"                                       # HTTP 메서드 사용

  response_export_values = ["publicKey", "privateKey"]       # 응답 값 중 출력할 항목 설정
}

# 출력 결과: 생성된 공개 키(Public Key)를 보여줌
output "key_data" {
  value = azapi_resource_action.ssh_public_key_gen.output.publicKey
}
```

### 3.3. main 코드 생성

1. main.tf라는 파일을 만들고 다음 코드를 삽입합니다.

```bash
# 랜덤한 이름 생성을 위한 리소스 (리소스 그룹 이름에 사용할 접두어 설정)
resource "random_pet" "rg_name" {
  prefix = var.resource_group_name_prefix
}

# Azure 리소스 그룹 생성
resource "azurerm_resource_group" "rg" {
  location = var.resource_group_location         # 지역 설정 (예: East US, Korea Central 등)
  name     = "MuleSoftTFT"                       # MuleSoftTFT 이름 사용
}

# 가상 네트워크(VNet) 생성
resource "azurerm_virtual_network" "my_terraform_network" {
  name                = "myVnet"
  address_space       = ["10.0.0.0/16"]           # VNet 주소 범위
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
}

# 서브넷 생성 (VNet 내부의 주소 블록 분할)
resource "azurerm_subnet" "my_terraform_subnet" {
  name                 = "mySubnet"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.my_terraform_network.name
  address_prefixes     = ["10.0.1.0/24"]          # 서브넷 주소 범위
}

# 퍼블릭 IP 생성 (외부 인터넷과 연결될 수 있도록)
resource "azurerm_public_ip" "my_terraform_public_ip" {
  name                = "myPublicIP"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  allocation_method   = "Dynamic"                 # 동적 할당 방식
}

# 네트워크 보안 그룹(NSG) 생성 및 SSH 접속 허용 규칙 추가
resource "azurerm_network_security_group" "my_terraform_nsg" {
  name                = "myNetworkSecurityGroup"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  security_rule {
    name                       = "SSH"           # 규칙 이름
    priority                   = 1001            # 우선순위 (낮을수록 우선)
    direction                  = "Inbound"       # 인바운드 트래픽 허용
    access                     = "Allow"         # 허용
    protocol                   = "Tcp"           # TCP 프로토콜
    source_port_range          = "*"             # 어떤 포트에서든
    destination_port_range     = "22"            # SSH 포트
    source_address_prefix      = "*"             # 어디서든
    destination_address_prefix = "*"             # 어디로든
  }
}

# 네트워크 인터페이스(NIC) 생성 (VM과 네트워크 연결 역할)
resource "azurerm_network_interface" "my_terraform_nic" {
  name                = "myNIC"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "my_nic_configuration"
    subnet_id                     = azurerm_subnet.my_terraform_subnet.id
    private_ip_address_allocation = "Dynamic"   # 자동 IP 할당
    public_ip_address_id          = azurerm_public_ip.my_terraform_public_ip.id
  }
}

# NIC에 NSG 연결 (보안 그룹 적용)
resource "azurerm_network_interface_security_group_association" "example" {
  network_interface_id      = azurerm_network_interface.my_terraform_nic.id
  network_security_group_id = azurerm_network_security_group.my_terraform_nsg.id
}

# 스토리지 계정 이름의 고유성 확보를 위한 랜덤 ID 생성
resource "random_id" "random_id" {
  keepers = {
    resource_group = azurerm_resource_group.rg.name  # 리소스 그룹이 바뀌면 새로운 ID 생성
  }
  byte_length = 8                                     # 8바이트 랜덤 값
}

# 부팅 진단을 위한 Azure 스토리지 계정 생성
resource "azurerm_storage_account" "my_storage_account" {
  name                     = "diag${random_id.random_id.hex}"  # 고유한 이름 설정
  location                 = azurerm_resource_group.rg.location
  resource_group_name      = azurerm_resource_group.rg.name
  account_tier             = "Standard"
  account_replication_type = "LRS"                # 로컬 복제 방식
}

# 리눅스 가상 머신 생성
resource "azurerm_linux_virtual_machine" "my_terraform_vm" {
  name                  = "myVM"
  location              = azurerm_resource_group.rg.location
  resource_group_name   = azurerm_resource_group.rg.name
  network_interface_ids = [azurerm_network_interface.my_terraform_nic.id]
  size                  = "Standard_DS1_v2"        # VM 크기 (사양)

  os_disk {
    name                 = "myOsDisk"
    caching              = "ReadWrite"
    storage_account_type = "Premium_LRS"          # 프리미엄 로컬 복제
  }

  # RHEL 9.4 이미지 선택
  source_image_reference {
    publisher = "redhat"
    offer     = "RHEL"
    sku       = "94_gen2"
    version   = "9.4.2025031315"  # 이미지 버전 (Azure Marketplace 기준)
  }

  computer_name  = "MuleSoftTFT"                 # 가상 머신 내부에서 보일 이름
  admin_username = var.username                  # 관리자 사용자 이름

  # SSH 키 기반 로그인 설정
  admin_ssh_key {
    username   = var.username
    public_key = azapi_resource_action.ssh_public_key_gen.output.publicKey
  }

  # VM 부팅 진단 활성화
  boot_diagnostics {
    storage_account_uri = azurerm_storage_account.my_storage_account.primary_blob_endpoint
  }
}
```

2. 사용할 리눅스 가상 머신 size 찾기
   [Azure LinuxVM Pricing](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux/#pricing)

- 위 링크 방문하여 알맞는 Cupeter를 찾아 아래 부분을 수정한다

```bash
# 리눅스 가상 머신 생성
resource "azurerm_linux_virtual_machine" "my_terraform_vm" {
  name                  = "myVM"
  location              = azurerm_resource_group.rg.location
  resource_group_name   = azurerm_resource_group.rg.name
  network_interface_ids = [azurerm_network_interface.my_terraform_nic.id]
  size                  = "Standard_DS1_v2" # 이부분을 원하는 컴퓨터 사이즈에 맞게 수정한다.
```

3. 사용할 source_image reference 찾기

- 아래 부분이 AzureVM에서 어떤 OS를 사용할지를 명시하는 부분이다.
- 올바른 source image reference를 찾기 위해서는 아래 4가지를 잘 찾아야 된다.
  1.  publisher
  2.  offer
  3.  sku
  4.  version

4. publisher 찾기

- 아래 명령어를 사용하여 알맞은 publisher를 찾는다

```bash
# location이 KoreaCentral이고 이름에 'redhat'이 들어간 pulisher 찾기
$ $ az vm image list-publishers --location KoreaCentral --query "[?contains(name, 'redhat')]" --output table
```

5. offer 찾기

```bash
# location이 KoreaCentral이고  publisher가 'redhat'인 offer 찾기
$ az vm image list-offers --location KoreaCentral --publisher redhat --output table
```

6. sku 찾기

```bash
#  location이 KoreaCentral이고  publisher가 'redhat'인 offer가 'RHEL'인 sku 찾기
$ az vm image list-skus --location KoreaCentral --publisher redhat --offer RHEL --output table
```

7. image version 찾기

```bash
# #  location이 KoreaCentral이고  publisher가 'redhat'인 offer가 'RHEL'이고 sku가 '94_gen2'인 image version 찾기
$ az vm image list --location KoreaCentral --publisher redhat --offer RHEL --sku 94_gen2 --all --output table
```

8. main.tf 수정

```bash
  # RHEL 9.4 이미지 선택
  source_image_reference {
    publisher = "redhat"
    offer     = "RHEL"
    sku       = "94_gen2"
    version   = "9.4.2025031315"  # 이미지 버전 (Azure Marketplace 기준)
  }
```

## 4. Terraform 초기화

Terraform init은

Terraform 프로젝트를 초기화하는 명령어입니다.

- 필요한 Provider 다운로드

- .terraform 디렉토리 생성

```bash
terraform init -upgrade
```

## 5. Terraform 실행 계획 만들기

terraform plan -out main.tfplan은 Terraform에서 변경 계획(plan)을 파일로 저장하는 명령어입니다.

- 현재 상태(state)와 .tf 코드의 차이를 비교

- 무엇을 생성/수정/삭제할지 보여주는 계획(plan) 을 출력

```bash
$ terraform plan -out main.tfplan
```

## 6. Terraform 실행 계획 적용

- Terraform이 계획(plan) 을 기반으로 리소스를 실제로 만들거나 수정하거나 삭제하는 명령어
- 즉, 진짜 인프라를 바꾸는 실행 단계입니다.

```bash
$ terraform apply main.tfplan
```

### 6.1 실행 결과

```hcl
key_data = "ssh-rsa AAAAB3Nza...generated-by-azure"
public_ip_address = "4.230.34.8"
resource_group_name = "MuleSoftTFT"
```

## 7. private-key 기반 pem key 생성

- Terraform 상태 파일에서 SSH 개인키를 추출해서 .pem 파일로 저장하고, 권한을 설정
- 터미널에서 아래 명령어를 실행하여 my-key.pem이라는 private key생성

```bash
$ cat terraform.tfstate | grep -oP '(?<=privateKey": ")[^"]+' | sed 's/\\r\\n/\n/g' > my-key.pem && chmod 400 my-key.pem
```

> 위 과정을 통해 Azure VM에 Red Hat OS 기반 서버를 성공적으로 구축할 수 있습니다.

## 8. MobaXterm을 통해 원격 서버 접속

public_ip_address와 my-key.pem을 가지고 MobaXterm을 통해 원격 접속

## n. References

- https://learn.microsoft.com/ko-kr/azure/developer/terraform
