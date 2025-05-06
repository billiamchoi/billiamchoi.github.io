---
layout: post
title: "[MuleSoft] MuleSoft FlexGateway 구축방법에 대해 알아보자"
description: MuleSoft FlexGateway 구축방법에 대해 알아보자
date: 2025-05-07T11:05:00 +09:00
categories: develop
keyword:
  - MuleSoft
  - MessageFlow
tags:
  - Back-End
  - MuleSoft
img-tag: mulesoft
---

# MuleSoft Flex Gateway 구축 Manual

---

### 목차

#### 1. 필요사항

#### 2. 서버 구성 및 구동

#### 3. MobaXterm 다운로드

#### 4. MobaXterm을 사용하여 SSH 접속

#### 5. 💡 LVM을 이용한 /home 파일 시스템 용량 확장 (선택)

#### 6. Outbound 443 Port 열기

#### 7. Flex Gateway 설치

#### 8. API Instance 설치

#### n. Troubleshooting

---

## 1. 필요사항

- Azure VM
- Red Hat Enterprise Linux 9.4 (LVM) - x64 Gen2 (OS)
- MuleSoft FlexGateway 1.9.0
- MobaXterm

## 2. 서버 구성 및 구동

### 2.1. Azure에 설치

중요 속성은 다음과 같음

- 초기 입력창
  ![alt text](/assets/img/1_Azure_setting.png)

#### 2.1.1. Azure 중요속성

- 지역: Korea Central
- 사용자이름: mulesoftuser
- 이미지: Red Hat Enterprise Linux 9.4 (LVM) - x64 Gen2
- 서버크기: 2 vcpu, 4 GiB 메모리

1. 중요속성을 준수하고 AzureVM 생성
2. Pem Key도 중요하여 다운받는다.

## 3. MobaXterm 다운로드

1. https://mobaxterm.mobatek.net/download.html 에 접속하여 Home Edition MobaXterm을 설치한다.

## 4. MobaXterm을 사용하여 SSH 접속

1. AzureVM 개요 페이지에서 연결할 공용 IP 주소를 확인한다.

2. MobaXterm을 실행하여 아래와 같이 세션 설정한다.
   ![alt text](/assets/img/2_SessionSetting.png)

- Remote host: Azure의 공용 IP 주소 입력
- username: 설정한 username 입력
- Advenced SSH setting 탭에 Use private key 클릭 후 pem key 위치 설정

위의 사항을 모두 준수하였으면 OK를 눌러 세션 설정하여 Azure VM 접속한다.

## 5. 💡 LVM을 이용한 /home 파일 시스템 용량 확장 (선택)

다음 조건을 모두 만족하는 경우에만 본 절차를 수행하십시오:

- [x] 설치 대상 시스템의 운영체제가 Red Hat Enterprise Linux이며, LVM(Logical Volume Manager)을 사용 중이다.
- [x] 원격 접속 후 df -h 명령어를 실행했을 때, /home 디렉토리의 사용 가능 용량이 설정한 저장 공간에 비해 현저히 작게 나타난다.

### 5.1. 시스템의 운영체제 및 **LVM(Logical Volume Manager)** 확인 방법

1. MobaXterm으로 서버에 원격 접속한다.

2. OS 확인 명령어를 입력한다.

```bash
$ cat /etc/os-release
```

3. LVM 확인 명령어를 입력한다.

```bash
$ lsblk
```

4. 아래와 같이 PRETTY_NAME에 Red Hat Enterprise Linux가 포함되어 있고 TYPE이 lvm인지를 확인한다.
   ![alt text](/assets/img/3_osnLvm.png)

좋아요! 이어지는 **용량 확장 절차**를 매뉴얼 형식에 맞춰 자연스럽게 이어서 작성해드릴게요. 각 단계에 **설명**을 추가하여, 사용자가 왜 이 명령어를 실행하는지 쉽게 이해할 수 있도록 했습니다.

---

### 5.2. /home 파일 시스템 용량 확장 절차

> 💬 **사전 확인**: 이 작업은 `/home` 논리 볼륨에 LVM의 남은 공간을 모두 할당하고, 해당 파일 시스템(XFS)을 확장하는 절차입니다.  
> 시스템에 따라 중요한 데이터가 있을 경우, 반드시 **백업 후 진행**하시기 바랍니다.

#### 5.2.1. LVM의 남은 공간을 `/home` 논리 볼륨에 할당

```bash
$ sudo lvextend -l +100%FREE /dev/rootvg/homelv
```

- `-l +100%FREE` 옵션은 현재 Volume Group(`rootvg`) 내의 **모든 남은 공간**을 `/home` 논리 볼륨(`homelv`)에 할당합니다.
- `/dev/rootvg/homelv` 경로는 시스템에 따라 다를 수 있으며, `lvs` 명령어로 확인할 수 있습니다.

#### 5.2.2. 파일 시스템 확장

```bash
$ sudo xfs_growfs /home
```

- LVM에 새롭게 할당된 공간이 실제 `/home` 디렉토리에서 사용 가능하도록 **XFS 파일 시스템의 크기를 확장**하는 명령어입니다.
- 이 명령어는 마운트된 상태에서도 실행할 수 있으며, 시스템 재시작 없이 즉시 적용됩니다.

#### 5.2.3. 적용된 용량 확인

```bash
$ df -h /home
```

- `/home`의 디스크 용량이 정상적으로 확장되었는지 확인합니다.
- `Available` 항목에서 확장된 용량이 반영되어야 합니다.

  **참고**: `/dev/rootvg/homelv`의 경로가 다른 경우, 아래 명령어로 실제 LV 이름과 마운트 위치를 먼저 확인해 주세요:

```bash
$ df -hT
```

## 6. Outbound 443 Port 열기

1. AzureVM 아웃바운드 포트 443 생성하여 열어 놓기
   ![alt text](/assets/img/4_azureport.png)

2. AzureVM 내 방화벽 열기
- 아래 명령어를 입력하여 443 포트 열기
```bash
$ sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
```
- 리로드하여 설정 적용
```bash
$ sudo firewall-cmd --reload
```
- 아래 명령어 입력하여 확인
```bash
sudo firewall-cmd --zone=public --list-ports
```

## 7. Flex Gateway 설치

### 7.1. gnupg2 설치

1. gnupg2 설치합니다

- 다운로드 페이지
  https://gnupg.org/download/index.html

2. MobaXterm으로 원격 접속합니다

3. 사용자 홈 디렉토리로 이동하여 다운받은 압축파일을 drag&drop한다.

```bash
# 사용자의 홈디렉토리로 이동
$  cd ~
```

4.  사용자 홈 디렉토리로 이동하여 다운받은 gnpg2를 압축해제한다.

```bash
# 사용자의 홈디렉토리로 이동
$ cd ~
# 압축해제
$ tar -xvjf gnupg-2.4.7.tar.bz2 -C ~/
# 압축파일 삭제
$ rm -rf ./gnupg-2.4.7.tar.bz2
```

### 7.2.1. Registration token으로 Flex Gateway 설치 (RHEL)

1. MobaXterm으로 원격 접속합니다

2. Flex Gateway 설치

- 버전별로 설치 스크립트가 다를 수 있으니 항상 anypoint platform 들어가서 확인한다.
- 접속 페이지
  https://anypoint.mulesoft.com/cloudhub/#/console/home/gateways/create

아래 명령어를 터미널에 복사 붙여넣기해서 실행한다.

- 변수 $releasever는 에러가 발생하여 9로 고정하여 실행

```bash
sudo tee /etc/yum.repos.d/flex-gateway.repo <<\EOF
[flex-gateway]
name = Anypoint Flex Gateway
baseurl = https://flex-packages.anypoint.mulesoft.com/rhel/9/main
gpgcheck = 1
gpgkey = https://flex-packages.anypoint.mulesoft.com/rhel/pubkey.gpg
repo_gpgcheck = 1
enabled = 1
EOF
sudo yum install -y flex-gateway
```

3. Flex Gateway 등록

- 아래는 예시일뿐 꼭 아래 링크를 방문하고, 설치할 bussinessGroup을 확인 후< gateway-name>를 flex gateway 이를으로 변경한다. 해당 명령어를 복사하자
  - https://anypoint.mulesoft.com/cloudhub/#/console/home/gateways/create

```bash
sudo /usr/local/bin/flexctl registration create <gateway-name> \
  --token=d573c0dd-6c36-4b55-bca4-768e2f7d85ed \
  --organization=2437c48f-31ee-4850-9e15-f6942f7590eb \
  --connected=true \
  --output-directory=/usr/local/share/mulesoft/flex-gateway/conf.d
```

- 참고
  ![alt text](/assets/img/5_regiFlex.png)

4. 터미널에 명령어를 붙여넣기해서 실행한다.

```bash
$ sudo systemctl start flex-gateway
```

5. Flex Gateway 실행

```bash
$ sudo systemctl start flex-gateway
```

6. Flex Gateway 구동 확인

```bash
$ systemctl list-units flex-gateway.service
```

아래 화면 같이 나오면 정상 작동 한것이다.

```bash
UNIT                 LOAD   ACTIVE SUB     DESCRIPTION
flex-gateway.service loaded active running Application
```

### 7.2.2. Registration token으로 Flex Gateway 설치 (Ubuntu)
1. MobaXterm으로 원격 접속합니다

2. Flex Gateway 설치

- 버전별로 설치 스크립트가 다를 수 있으니 항상 anypoint platform 들어가서 확인한다.
- 접속 페이지
  https://anypoint.mulesoft.com/cloudhub/#/console/home/gateways/create

아래 명령어를 터미널에 복사 붙여넣기해서 실행한다.

- 변수 $(lsb_release -cs)는 현재 사용 중인 Ubuntu 버전의 코드명이 자동으로 치환되는데, 에러 발생 시 직접 넣어줄 것
- 아래 코드 블럭은 docs를 참고했으나, `apt-key`의 사용은 deprecated 되었음(2025-03-27 확인)

```bash
curl -XGET https://flex-packages.anypoint.mulesoft.com/ubuntu/pubkey.gpg | sudo apt-key add -
echo "deb [arch=amd64] https://flex-packages.anypoint.mulesoft.com/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/mulesoft.list
sudo apt update
sudo apt install -y flex-gateway
```
- 따라서 아래 코드 블럭을 참고할 것
```bash
curl -fsSL https://flex-packages.anypoint.mulesoft.com/ubuntu/pubkey.gpg | sudo gpg --dearmor -o /usr/share/keyrings/mulesoft-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/mulesoft-archive-keyring.gpg] https://flex-packages.anypoint.mulesoft.com/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/mulesoft.list
sudo apt update
sudo apt install -y flex-gateway
```
3. ~ 이후 등록 과정은 RHEL과 동일함

## 8. API Instance 설치

### 8.1. 💡 API Instance Downstream을 위한 Port 개방

- 해당 장은 프로젝트 별로 매우 상이 할 수 있습니다.
- Client와 API Instance가 통신할 포트를 해당장에서 개방하는 작업을 합니다.
  1. AzureVM 인바운드 규칙 추가
  2. linux 서버 내 Firewall

#### 8.1.1. AzureVM 인바운드 포트 개방

1. 설치한 AzureVM으로 접속한다.
2. 왼쪽 탭에 네트워킹 > 네트워크 설정을 선택한다.
3. Inbound 규칙 추가한다.

- 대상 포트 범위 :8081
- 프로토콜: Any
  ![alt text](/assets/img/9_add_inbound.png)

#### 8.1.2. Linux 서버에서 Inbound Port 개방

1. MobaXterm으로 서버에 원격 접속

2. 아래 명령어를 입력하여 8081번 포트까지 열기

```bash
$ sudo firewall-cmd --zone=public --add-port=8081/tcp --permanent
```

3. 리로드하여 설정 적용

```bash
$ sudo firewall-cmd --reload
```

4. 아래 명령어 입력하여 확인

```bash
sudo firewall-cmd --zone=public --list-ports
```

### 8.2. Anypoint Platform에서 API Instance를 설치

1. API Instance를 설치할 Flex Gateway를 선택한다.
2. 왼쪽 APIs 탭 클릭 후, Add API 버튼 클릭한다.
   ![alt text](/assets/img/7_API_Instance.png)

3. Add API에서 알맞게 선택한다.

- Select runtime: Flex Gateway
- Select Gateway: API Instance를 설치할 Gateway 선택

![alt text](/assets/img/8_Add_API.png)

4. API이름과 Asset types를 올바르게 기입하고 Next click

- Name: 사용할 API Instance 이름
- Asset types: HTTP API

![alt text](/assets/img/10_create_new_API.png)

5. Downstream 설정

- Protocol: HTTP
- Port: 8081

![alt text](/assets/img/11_downstream.png)

6. Downstream 설정

- Upstream URL을 통신할 서버 수 만큼 넣어주고 Weight를 기입한다.

![alt text](/assets/img/12_upstream.png)

7. 정상적으로 API Instance가 배포가 되면 아래와 같이 나온다.

![alt text](/assets/img/13_finish.png)

8. postman으로 해당 Instance를 호출 하여 정상 응답이 오는지 Postman으로 테스트한다.

