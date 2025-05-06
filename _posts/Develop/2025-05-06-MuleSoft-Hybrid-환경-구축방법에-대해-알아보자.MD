---
layout: post
title: "[MuleSoft] MuleSoft Hybrid 환경 구축방법에 대해 알아보자"
description: MuleSoft Hybrid 환경 구축방법에 대해 알아보자
date: 2025-05-06T11:05:00 +09:00
categories: develop
keyword:
  - MuleSoft
  - MessageFlow
tags:
  - Back-End
  - MuleSoft
img-tag: mulesoft
---
# MuleSoft Hybrid 환경 구축 Manual

---

### 목차

#### 1. 필요사항

#### 2. 서버 구성 및 구동

#### 3. MuleSoft Standalone 다운로드

#### 4. MobaXterm 다운로드

#### 5. MobaXterm을 사용하여 SSH 접속

#### 6. MuleSoft Standalone 설치

#### 7. JDK 17 설치

#### 8. Enterprise 라이센스 설치

#### 9. Enterprise 라이센스 확인 및 뮬서버 구동

#### 10. (필수아님) ARM 에이전트 다운로드 및 설치(다른 버전의 에이전트가 필요한 경우)

#### 11. ARM Agent를 사용하여 Runtime Manager에 Runtime Engine 등록

#### 12. 애플리케이션 배포

#### 13. AM(anypoint monitoring) Agent 설치

#### 14. MuleSoft Proxy Server 설치하여 Reverse Proxy 적용

#### 15. (필수아님) Nginx 설치하여 Reverse Proxy 적용

#### 16. (필수아님) AWS & Azure 서버 클러스터 생성

#### 17. (필수아님) Mule Gateway with Nginx 구성

#### 18. (필수아님) Mule Gateway with ELK 구성

#### 19. Troubleshooting

---

## 1. 필요사항

<b>만약 Azure에 구축시</b>

- Azure VM
- Red Hat Enterprise Linux 9.4 (LVM) - x64 Gen2 (OS)

<b>만약 AWS에 구축시</b>

- AWS EC2
- amazon/RHEL-9.5.0_HVM-20241211-x86_64-0-Hourly2-GP3 (OS)

<b>공통 필요사항</b>

- MuleSoft Standalone 4.8.1 (설치URL: https://www.mulesoft.com/lp/dl/anypoint-mule-studio)
- MobaXterm
- Nginx (optional)

## 2. 서버 구성 및 구동

설치할 서버에 따라 서버 구성하고 구동하면 됩니다.

Azure에 설치할 경우, 2.1. Azure에 설치를 보시고

AWS에 설치할 경우, 2.2. AWS에 설치를 보시면 됩니다.

### 2.1. Azure에 설치

중요 속성은 다음과 같음

- 초기 입력창
  ![alt text](/assets/img/1_AzureSetting.png)
- 확인창
  ![alt text](/assets/img/2_AzureConfirm.png)

#### 2.1.1. Azure 중요속성

- 지역: Korea Central
- 사용자이름: mulesoftuser
- 이미지: Red Hat Enterprise Linux 9.4 (LVM) - x64 Gen2
- 서버크기: 2 vcpu, 8 GiB 메모리

1. 중요속성을 준수하고 AzureVM 생성
2. Pem Key도 중요하여 다운받는다.

\*AzureVM에서 AzureRocky 현재 안됨 (아래 스크린샷 참고)
![alt text](/assets/img/3_AzureRockyError.png)

---

### 2.2. AWS에 설치

중요 속성은 다음과 같음

- 초기 입력창
  ![alt text](/assets/img/47_aws_instance.png)

#### 2.2.1. AWS 중요 속성

- 지역: 아시아 태평양(서울)
- 이미지: amazon/RHEL-9.5.0_HVM-20241211-x86_64-0-Hourly2-GP3
  (이미지는 위 텍스트로 검색 후, 커뮤니티 AMI 탭에서 선택한다)
- 서버크기: t2.small (2 vcpu, 8 GiB 메모리)

1. 중요속성을 준수하고 AWS EC2 생성
2. Pem Key도 중요하여 새 키 페어 생성하여 다운받는다.

## 3. MuleSoft Standalone 다운로드

1. https://www.mulesoft.com/lp/dl/anypoint-mule-studio 방문하여 아래와 같이 작성 후, 다운로드 클릭
   ![alt text](/assets/img/4_Download.png)

2. 아래와 같은 이메일이 도착하면 Go to download page를 클릭하여 다운 받는다.
   ![alt text](/assets/img/5_email.png)

## 4. MobaXterm 다운로드

1. https://mobaxterm.mobatek.net/download.html 에 접속하여 Home Edition MobaXterm을 설치한다.

## 5. MobaXterm을 사용하여 SSH 접속

1. AzureVM 개요 페이지에서 연결할 공용 IP 주소를 확인한다.

2. MobaXterm을 실행하여 아래와 같이 세션 설정한다.
   ![alt text](/assets/img/6_SessionSetting.png)

- Remote host: Azure의 공용 IP 주소 입력
- username: 설정한 username 입력
- Advenced SSH setting 탭에 Use private key 클릭 후 pem key 위치 설정

위의 사항을 모두 준수하였으면 OK를 눌러 세션 설정하여 Azure VM 접속한다.

## 6. MuleSoft Standalone 설치

1. MobaXterm을 통해 AzureVM에 접속하고 rootDirectory에 아까 다운받은 MuleSoftStandone(mule-ee-distribution-standalone-4.8.1.zip)을 Drag & Drop으로 옮긴다.

2. 설치파일 압축제거 및 압축파일 제거

```bash
// rootdirectory에 압축풀기
$ unzip mule-ee-distribution-standalone-4.8.1.zip
// 압축파일 제거
$ rm -rf mule-ee-distribution-standalone-4.8.1.zip
```

3. bash_profile에서 MULE_HOME 환경변수 설정

```bash
// rootdirectory에서 vi로 bashrc 편집
$ vi .bashrc
// bashrc에 아래와 같이 추가 후 저장
export MULE_HOME=~/mule-enterprise-standalone-4.8.1
```

4. source 명령어로 변경사항 반영

```bash
$ source ~/.bashrc
```

## 7. JDK 17 설치

1. VM의 root Directory에서 아래 명령어를 입력하여 JDK 17을 다운로드 받는다.

```bash
$ sudo yum install java-17-openjdk-17.0.13.0.11-4.el9.x86_64
```

2. 환경변수 JAVA_HOME 추가

- cd /usr/lib/jvm을 사용하여 이동하고 ls 명령어를 사용하여 아래 java-17로 시작하는 디렉토리를 찾는다.

- 해당 명령어를 .bashrc에 추가 후 저장한다.

```bash
export JAVA_HOME="/usr/lib/jvm/java-17-openjdk-17.0.13.0.11-4.el9.x86_64"
export PATH="$JAVA_HOME/bin:$PATH"
```

- source 명령어로 변경사항 반영

```bash
$ source ~/.bashrc
```

3. 아래 명령어를 사용하여 환경변수 Test를 진행한다.

```bash
// 해당 명령어는 foreground mode로서 터미널에 콘솔 로그를 출력 합니다.
// Mule이 실행되는 동안에는 터미널에서 추가 명령을 실행할 수 없습니다.
$ $MULE_HOME/bin/mule
```

3. 성공적으로 테스트 완료되면 아래와 같은 메세지가 터미널에 표시된다.
   ![alt text](/assets/img/7_successdeploy.png)

## 8. Enterprise 라이센스 설치

1. MuleSoft Help Center(https://help.mulesoft.com)를 방문 후 로그인한다.
2. 상단에 Support를 클릭하여 서포트 페이지 방문한다.
3. 하단에 License keys를 클릭하여 License 페이지로 이동한다.
4. 프로젝트의 라이센트 키 클릭 후 다운로드한다.
   (형식: license.lic)
5. $MULE_HOME/bin 디렉토리로 이동 후 다운받은 license.lic를 해당 디렉토리에 복사 붙여넣기 한다.
6. 해당 명령어를 입력하여 license를 설치한다.

```bash
// 상황에 따라, 뮬 standalone 버전넘버는 다를 수 있다.
$ ./mule -installLicense ~/mule-enterprise-standalone-4.8.1/bin/license.lic
```

7. 성공적으로 설치가 완료되었다면, %MULE_HOME%\conf 디렉토리에 하위에 muleLicenseKey.lic가 보임

## 9. Enterprise 라이센스 확인 및 뮬서버 구동

1. %MULE_HOME%\bin 디렉토리에서 아래와 같은 명령어를 입력하여 MuleSoft 라이센스 확인

```bash
$ ./mule -verifyLicense
```

2. 정상적으로 확인되었으면 아래와 같은 콜솔 메세지 확인할 수 있다.
   ![alt text](/assets/img/8_validLicense.png)

3. 아래 명령어를 사용하여 뮬서버 구동

```bash
$ ./mule start
```

## 10. (필수아님) ARM 에이전트 다운로드 및 설치(다른 버전의 에이전트가 필요한 경우)

1. 뮬소프트 지원페이지 방문 (https://help.mulesoft.com/s/support)
2. 중앙에 Download 클릭
3. 설치할 에이전트를 찾아 작업 열에서 다운로드를 클릭합니다.
   ![alt text](/assets/img/9_muleagent.png)
4. Mule Runtime을 중지합니다.

```bash
$ ./mule stop
```

5. 다운로드한 agent-setup-VERSION.zip 파일을 OS 유형에 따라 $MULE_HOME/bin 또는 %MULE_HOME%\bin에 추출합니다. (메시지가 표시되면 충돌하는 모든 파일을 덮어씁니다.)
6. $MULE_HOME/bin/amc_setup 또는 %MULE_HOME%\bin\amc_setup.bat을 실행하고 설치에 대한 매개변수를 지정합니다.
   지정할 매개변수에 대한 정보는
   (https://docs.mulesoft.com/runtime-manager/installing-and-configuring-runtime-manager-agent#amc_setup-parameters)를 참조하세요.
7. Mule Runtime을 다시 시작합니다.
8. Runtime Manager Agent(ARM Agent)가 $MULE_HOME/plugins 또는 %MULE_HOME%\plugins 디렉터리에 설치됩니다.

## 11. ARM Agent를 사용하여 Runtime Manager에 Runtime Engine 등록

- ARM Agent는 Mule Runtime Engine을 Runtime Manager에 등록합니다.
- Runtime이 등록된 후에는 Mule이 등록된 특정 환경 및 Anypoint Platform 비즈니스 그룹 내에서 Runtime Manager를 사용하여 관리할 수 있습니다.
- 여러 개의 Runtime Manager 비즈니스 그룹이나 환경에 Mule Runtime을 등록할 수 없습니다.

### 11.1. Runtime Manager에 등록

#### 필요사항

- registration token: Anypoint Platform에서 특정 비즈니스 그룹의 특정 환경을 식별하는 유효한 등록 토큰입니다.
- instance name: 런타임 관리자 콘솔에서 Mule을 식별하는 인스턴스 이름입니다.

Any Point Studio로 접속하여 아래 순서대로 진행한다.

1. Runtime Manager에서 왼쪽 메뉴에서 Servers를 클릭합니다.
2. Add Server 버튼을 클릭합니다.
3. 서버의 고유한 이름을 입력합니다.
   ![alt text](/assets/img/10_add_server.png)
4. Copy command를 클릭하여 AzureVM $MULE_HOME/bin에서 명령어를 붙여넣고 실행합니다.

```bash
$ ./amc_setup -H f608a6a8-515c-4aab-813b-9137a49b7367---1928550 MuleSoftVM1-azure
```

5. 아래 로그와 같이 성공적으로 설정이 완료되었다는 문구 표기되면 정상적으로 설정된 것이다.

```bash
Mule Agent configured successfully
Mule Agent configuration saved successfully
```

6. 다시 뮬서버 재기동

```bash
$ ./mule restart
```

7. 문제없이 서버 설정했으면 서버가 아래와 같이 목록에 추가됩니다.
   ![alt text](/assets/img/11_runtimemanager.png)

## 12. 애플리케이션 배포

1. Anypoint Platform 로그인 및 접근

- Runtime Manager에 접속해 애플리케이션 배포

2. 애플리케이션 이름 지정

- 환경 내 고유 이름 설정 (3~42자, 알파벳 및 숫자 조합).

3. 배포 대상 선택

- 서버, 서버 그룹 또는 클러스터 중 선택.

4. 애플리케이션 파일 업로드 방식

- Exchange에서 가져오기: 기존 애플리케이션 선택.
  ![alt text](/assets/img/12_pub_to_ex.png)
  ![alt text](/assets/img/13_pub_pop_up.png)
  ![alt text](/assets/img/14_deploy_app.png)
  ![alt text](/assets/img/15_get_from_exchange.png)
- ZIP 파일 업로드: 로컬파일 업로드
- 다른 환경에서 복사: 샌드박스 등 비프로덕션 환경에서 가져오기
- JAR 형식 요구시: ZIP 파일의 확장자만 JAR로 변경 가능

## 13. AM(anypoint monitoring) Agent 설치

<b>Azure일때</b>

1. AzureVM 아웃바운드 포트 443 생성하여 열어 놓기
   ![alt text](/assets/img/16_azureport.png)

<b>AWS일때</b>

<b>EC2 콘솔 접속 및 보안그룹으로 이동</b>

1. AWS EC2 Console로 로그인한다.
2. 대시보드 상단 리소스 하위 인스턴스(실행중)을 클릭한다.
3. 아래 그림과 같이 첫째 인스턴스 ID를 클릭한다.
4. 둘째, 하단에 보안 탭을 클릭한다.
5. 셋째, 보안 그룹을 클릭한다.
   ![alt text](/assets/img/26_select_security_group_aws.png)

<b>아웃바운드 규칙 추가</b>

1. 인바운드 규칙 편집을 클릭
   ![alt text](/assets/img/29_edit_outbound.png)

2. 첫째, 규칙 추가를 클릭
3. 둘째, 아래와 같은 Spec으로 inbound 규칙 추가
   - 유형: 사용자 지정 TCP
   - 포트 범위: 443
   - CIDR 블록: 0.0.0.0/0
4. 셋째, 규칙 저장 클릭
   ![alt text](/assets/img/48_outbound443.png)

<b>이후 세팅</b>

1. Anypoint Platform에서 Monitoring을 클릭하고 Settings를 누른다.
   ![alt text](/assets/img/17_monitoring.png)
2. MobaXterm을 사용하여 MuleSoft Root Directory에 해당페이지에서 다운받은 am.zip을 drag & drop으로 옮긴다.
3. 설치파일 압축제거 및 zip파일 제거

```bash
// rootdirectory에 압축풀기
$ unzip am-2.5.11.2.zip
// 압축파일 제거
$ rm -rf am-2.5.11.2.zip
```

5. am 디렉토리로 이동 후 설치 명령어 실행

```bash
// am 디렉토리로 이동
$ cd am
// 설치 명령어 실행
$ ./bin/install
```

6. 뮬 애플리케이션을 종료하고 뮬 서버 및 뮬 애플리케이션을 다시시작한다.
7. MobaXterm을 사용하여 am 폴더로 이동 후 아래 명령어를 입력해 설정을 한다.

```bash
$ ./bin/setup
```

참고로 뮬 서버 시작과 AM시작 명령어는 다음과 같다.

```bash
// Server 시작 : [mulesoftuser@MuleSoftVM1 bin]
$ ./$MULE_HOME/bin/mule start
// AM 시작 : [mulesoftuser@MuleSoftVM1 am
$ ./$MULE_HOME/am/bin/am-service start
```

8. 10분이 지나면, Anypoint studio - monitoring에서 아래와 같이 확인이 가능하다.
   ![alt text](/assets/img/18_am.png)

## 14. MuleSoft Proxy Server 설치하여 Reverse Proxy 적용

상황에 따라서 MuleSoft Proxy Server를 설치하거나 Nginx를 설치하도록 하자

1. (Azure 기준) 네트워크 보안 그룹에서 인바운드 80포트와 8081포트(뮬어플리케이션 listener 포트)를 열도록 인바운드 보안 규칙을 추가한다. (https 사용시 443 port도 열어야함)
   ![alt text](/assets/img/19_80port.png)

2. Proxy Server가 방화벽에 서비스를 제공해야 하는 포트를 엽니다. 예를 들어 firewalld에서 HTTP(포트 80) 및 HTTPS(포트 443)의 기본 포트를 열려면 다음을 입력합니다.

```bash
$ firewall-cmd --permanent --add-port={80/tcp,8081/tcp,443/tcp}
$ firewall-cmd --reload
```

3. MobaXterm에서 서버 원격접속하여 bin으로 이동후 관리자 권한으로 뮬서버 재가동

```bash
// MULE_HOME의 bin 디렉토리로 이동
$ cd mule-enterprise-standalone-4.8.3/bin
// 관리자 권한으로 JAVA_HOME 명시하고 Mule Server 다시 시작
$ sudo JAVA_HOME=$JAVA_HOME ./mule restart
```

4. API Manager에서 Add new API를 클릭한다.

5. Mule Runtime 설정
   아래와 같이 설정한다.

   runtime: Mule Gateway
   Proxy type: Deploy a proxy application
   Target type: Hybrid
   Select server: 프록시 서버 적용할 뮬 서버 선택
   Next 버튼 클릭
   ![alt text](/assets/img/20_add_api.png)

6. Proxy API 설정
   아래와 같이 설정한다.

   Create new API 선택
   Name에 이름 작성
   Asset type HTTP API 선택

7. Downstream 설정한다.
   Protocol: HTTP
   Port: 80
   Base path: /api (/ 로 설정하면 포트 충돌 발생함)

8. Upstream 설정한다.
   Upstream URL: 서버주소와 뮬어플리케이션에서 사용할 포트 조합으로 입력한다 (ex: http://20.123.45.6:8081)

9. Save& Deploy를 클릭하여 배포 진행한다.

10. 정상적으로 배포되었다면, 아래와 같이 나올 것입니다.
    그 이후, 뮬 프록시 서버를 /api/개발한api주소로 호출해보자.
    ![alt text](/assets/img/21_api_spec.png)

## 15. (필수아님) Nginx 설치하여 Reverse Proxy 적용

1. (Azure 기준) 네트워크 보안 그룹에서 인바운드 80포트와 8081포트를 열도록 인바운드 보안 규칙을 추가한다. (https 사용시 443 port도 열어야함)
   ![alt text](/assets/img/19_80port.png)

2. nginx 패키지를 설치합니다

```bash
$ yum install nginx
```

3. NGINX가 방화벽에 서비스를 제공해야 하는 포트를 엽니다. 예를 들어 firewalld에서 HTTP(포트 80) 및 HTTPS(포트 443)의 기본 포트를 열려면 다음을 입력합니다.

```bash
$ firewall-cmd --permanent --add-port={80/tcp,443/tcp,8081/tcp}
$ firewall-cmd --reload
```

4. 시스템이 부팅될 때 nginx 서비스가 자동으로 시작되도록 활성화합니다.

```bash
$ sudo systemctl enable nginx
```

5. vi 편집기로 ngix.conf 파일 수정

```bash
$ vi /etc/nginx/nginx.conf
```

6. server > location에 아래와 같이 추가 후 저장
   proxy_pass의 포트는 뮬 application 마다 상이하므로 원하는 포트로 수정

```conf
   location / {
      proxy_pass http://127.0.0.1:8081; # 포트 8081로 요청 전달
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
   }

```

7. 뮬 서버 및 애플리케이션 다시 시작 후 nginx 서비스 시작

```bash
$ systemctl start nginx
```

8. 정상적으로 완료 되었으면, postman에서 해당 서버 호출 시, 포트 없이 호출 가능함

## 16. (필수아님) AWS & Azure 서버 클러스터 생성

본 과정은 AWS와 Azure에 모두 기본 뮬서버 세팅이 완료된 것으로 가정한다.

### 16.1. AWS 사전 작업

#### 16.1.1. EC2 콘솔 접속 및 보안그룹으로 이동

1. AWS EC2 Console로 로그인한다.
2. 대시보드 상단 리소스 하위 인스턴스(실행중)을 클릭한다.
3. 아래 그림과 같이 첫째 인스턴스 ID를 클릭한다.
4. 둘째, 하단에 보안 탭을 클릭한다.
5. 셋째, 보안 그룹을 클릭한다.
   ![alt text](/assets/img/26_select_security_group_aws.png)

#### 16.1.2. 인바운드 규칙 추가

1. 인바운드 규칙 편집을 클릭
   ![alt text](/assets/img/27_edit_inbound.png)
2. 첫째, 규칙 추가를 클릭
3. 둘째, 아래와 같은 Spec으로 inbound 규칙 추가
   - 유형: 사용자 지정 TCP
   - 포트 범위: 5701 - 5800
   - CIDR 블록: 0.0.0.0/0
4. 셋째, 규칙 저장 클릭
   ![alt text](/assets/img/28_add_inbound_rule.png)

#### 16.1.3. 아웃바운드 규칙 추가

1. 아웃바운드 규칙 탭을 클릭하고 아웃바운드 규칙을 클릭
   ![alt text](/assets/img/29_edit_outbound.png)
2. 첫째, 규칙 추가를 클릭
3. 둘째, 아래와 같은 Spec으로 outbound 규칙 추가
   - 유형: 사용자 지정 TCP
   - 포트 범위: 5701 - 5800
   - CIDR 블록: 0.0.0.0/0
4. 셋째, 규칙 저장 클릭
   ![alt text](/assets/img/30_add_outbound.png)

#### 16.1.4. AWS 원격서버 내 방화벽 열기

1. MobaXterm으로 원격서버 접속
2. 아래 명령어를 입력하여 5701 ~ 5800번 포트까지 열기

```bash
$ sudo firewall-cmd --zone=public --add-port=5701-5800/tcp --permanent
```

3. 리로드하여 설정 적용

```bash
$ sudo firewall-cmd --reload
```

4. 아래 명령어 입력하여 확인

```bash
sudo firewall-cmd --zone=public --list-ports
```

- 아래와 같이 5701 - 5800이 추가되었는지 확인
  ![alt text](/assets/img/31_ports.png)

### 16.2. Azure 사전 작업

#### 16.2.1. AzureVM 콘솔 접속 및 네트워크 설정으로 이동

1. Azure으로 로그인한다.
2. AzureVM을 클릭하여 이동.
3. 아래 그림과 같이 첫째 네트워크 설정 클릭.
4. 둘째, 포트규칙 만들기 클릭한다.
5. 셋째, 인바운드 포트 규칙을 클릭한다.
   ![alt text](/assets/img/32_add_inbound_rule_azure.png)

#### 16.2.2. 인바운드 규칙 추가

1. 아래 스펙대로 인바운드 규칙을 설정하고 추가를 클릭

- 소스: Any
- 원본 포트 범위: \*
- 대상 주소: Any
- 서비스: Custom
- 대상 포트 범위: 5701-5800
- 프로토콜: Any
- 작업: 허용
- 우선순위: {우선순위는 상관없음}
- 이름: {이름은 상관없음}
  ![/assets/img/33_Add_Inbound_Azure.png](/assets/img/33_Add_Inbound_Azure.png)

#### 16.2.3. 아웃바운드 규칙 추가

1. 16.2.1. 그림을 참고하여 네트워크 설정에서 포트 규칙 만들기 클릭 후 아웃바운드 규칙을 클릭
2. 아래 스펙대로 아웃바운드 규칙을 설정하고 추가를 클릭

- 소스: Any
- 원본 포트 범위: \*
- 대상 주소: Any
- 서비스: Custom
- 대상 포트 범위: 5701-5800
- 프로토콜: Any
- 작업: 허용
- 우선순위: {우선순위는 상관없음}
- 이름: {이름은 상관없음}
  ![alt text](/assets/img/34_Add_Outbound_Azure.png)

#### 16.2.4. Azure 원격서버 내 방화벽 열기

1. MobaXterm으로 원격서버 접속
2. 아래 명령어를 입력하여 5701 ~ 5800번 포트까지 열기

```bash
$ sudo firewall-cmd --zone=public --add-port=5701-5800/tcp --permanent
```

3. 리로드하여 설정 적용

```bash
$ sudo firewall-cmd --reload
```

4. 아래 명령어 입력하여 확인

```bash
sudo firewall-cmd --zone=public --list-ports
```

- 아래와 같이 5701 - 5800이 추가되었는지 확인
  ![alt text](/assets/img/31_ports.png)

### 16.3. Runtime Manager에 클러스터 생성

#### 16.3.1. 클러스터 생성화면으로 이동

1. Anypoint Platform의 Runtime Manager로 이동한다.
2. 좌측에 Servers 탭을 클릭하고 Create Cluster를 눌러 클러스터 생성화면으로 이동
   ![alt text](/assets/img/35_Add_Cluster.png)

#### 16.3.2. 클러스터 생성

1. 아래 스팩대로 입력 후 Create Cluster를 클릭하여 클러스터 생성

- Cluster Name: {클러스터 이름은 상관없음}
- Cluster Type: Unicast
- Select Servers: AWS서버와 Azure 서버를 체크
- IP: Azure와 AWS의 공용 IP 주소
- Port: 5701
  ![alt text](/assets/img/22_createacluster.png)

2. 기존 AWS와 Azure 서버가 restart되는데까지 기다려야함 (5분정도)

3. Runtime Manager Server Tab에서 Cluster status가 초록색 Running임을 확인
   ![alt text](/assets/img/36_ConfirmCluster.png)

---

## 17. Mule Gateway with Nginx 구성

### 17.1. System Architecture Diagram

![alt text](/assets/img/37_outline_mulegateway.png)
현재 챕터에서 구현할 시스템 아키텍처 다이어그램은 위 그림과 같다.

### 17.1. AWS 사전 작업

#### 17.1.1. EC2 콘솔 접속 및 보안그룹으로 이동

1. AWS EC2 Console로 로그인한다.
2. 대시보드 상단 리소스 하위 인스턴스(실행중)을 클릭한다.
3. 아래 그림과 같이 첫째 설치할 인스턴스 ID를 클릭한다.
4. 둘째, 하단에 보안 탭을 클릭한다.
5. 셋째, 보안 그룹을 클릭한다.
   ![alt text](/assets/img/26_select_security_group_aws.png)

#### 17.1.2. 인바운드 규칙 추가

1. 인바운드 규칙 편집을 클릭
   ![alt text](/assets/img/27_edit_inbound.png)
2. 첫째, 규칙 추가를 클릭
3. 둘째, 아래와 같은 Spec으로 inbound 규칙 추가
   80

   - 유형: HTTP
   - 포트 범위: 80
   - CIDR 블록: 0.0.0.0/0

   8080

   - 유형: 사용자 지정 TCP
   - 포트 범위: 8080
   - CIDR 블록: 0.0.0.0/0

   8081

   - 유형: 사용자 지정 TCP
   - 포트 범위: 8081
   - CIDR 블록: 0.0.0.0/0

4. 셋째, 규칙 저장 클릭
   ![alt text](/assets/img/38_add_inbound_rule.png)

### 17.2. Nginx 설치

1. nginx 패키지를 설치합니다

```bash
$ sudo yum install nginx
```

2. NGINX가 방화벽에 서비스를 제공해야 하는 포트를 엽니다. 예를 들어 firewalld에서 HTTP(포트 80) 및 HTTPS(포트 443)의 기본 포트를 열려면 다음을 입력합니다.

```bash
$ firewall-cmd --permanent --add-port={80/tcp,8080/tcp,8081/tcp}
$ firewall-cmd --reload
```

3. SELinux Permissive 모드로 변경

```bash
$ sudo setenforce 0
```

4. SELinux Permissive 모드로 변경

```bash
$ sudo vi /etc/selinux/config
```

5. 파일 내 SELINUX=enforcing 을 SELINUX=permissive로 수정

6. 시스템이 부팅될 때 nginx 서비스가 자동으로 시작되도록 활성화합니다.

```bash
$ sudo systemctl enable nginx
```

7. vi 편집기로 ngix.conf 파일 수정

```bash
$ vi /etc/nginx/nginx.conf
```

8. log_format 및 server에 아래와 같이 수정 후 저장

```bash

# upstream mule-servers 설정
http {
    upstream mule-servers {
        server 20.196.66.2:8081;
        server 13.209.83.213:8081;
    }

# 로그 포멧 수정
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                  'upstream: $upstream_addr, '
                  'host: "$host", status: $status, '
                  'response_time: $upstream_response_time';

 # 서버 설정 수정:
 server {
        listen       8080;
        listen       [::]:8080;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
        # downstream 설정
        location /api/ {
            proxy_pass http://mule-servers/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
		}
```

7. 뮬 서버 및 애플리케이션 시작 후 nginx 서비스 시작

```bash
$ systemctl start nginx
```

### 17.3. Proxy Application 설치

1. MuleSoft Anypoint Platform에 접속한다.

- https://anypoint.mulesoft.com/login/

2. API Manager에서 Add new API를 클릭한다.
   <br>

3. Mule Runtime 설정
   아래와 같이 설정한다.

   runtime: Mule Gateway
   Proxy type: Deploy a proxy application
   Target type: Hybrid
   Select server: 프록시 서버 적용할 뮬 서버 선택
   Next 버튼 클릭
   ![alt text](/assets/img/20_add_api.png)
   <br>

4. Proxy API 설정
   아래와 같이 설정한다.

   Create new API 선택
   Name에 이름 작성
   Asset type HTTP API 선택
   <br>

5. Downstream 설정한다.
   Protocol: HTTP
   Port: 80
   Base path: /gateway
   <br>
6. Upstream 설정한다.
   Upstream URL: 서버주소와 뮬어플리케이션에서 사용할 포트 조합(8080)으로 입력한다 (ex: http://20.123.45.6:8080/api)
   <br>
7. Save & Deploy를 클릭하여 배포 진행한다.
   <br>
8. 정상적으로 배포되었다면, 아래와 같이 나올 것입니다.
   그 이후, 뮬 프록시 서버를 /gateway/개발한api주소로 호출해보자.
   ![alt text](/assets/img/39_proxy.png)

## 18. (필수아님) Mule Gateway with ELK 구성

### 18.1. Mule API Gateway Wrapper 설정 변경

1. wrapper.conf vi로 편집

```bash
$ sudo vi $MULE_HOME/conf/wrapper.conf
```

2. wrapper.conf 설정 변경

```bash
# <n>은 각 conf파일 별로 상이하기 때문에 자신의 설정파일에 알맞게 설정해야 함
# 이미 기존 설정값이 있으면 해당 숫자(<n>)를 사용하고
# 만약 없다면 가장 큰 값 + 1을 한다
wrapper.java.additional.<n>=-Danypoint.platform.analytics_base_uri=
wrapper.java.additional.<n>=-Danypoint.platform.analytics_enabled=true
```

3. Mule 서버 재시작

```bash
$ sudo JAVA_HOME=$JAVA_HOME ./mule restart
```

### 18.2. Inbound 설정 및 원격서버 포트 개방

해당 파트는 뮬소프트 로그를 전송할 ELK 서버과 어느 포트로 통신할 것인지에 따라 달라진다.
정확히는 대상 Elasticsearch와 통신할 포트에 대한 설정이다.
Elasticsearch 기본으로 9200포트로 통신함

아래 예제는 AWS에 선치된 Mule Gateway와 Elasticsearch가 443(https)포트로 통신하는 것을 기준으로 진행함
상황에 따라 Inbound와 Outbound를 모두 열어줘야될 수도 있음

#### 18.2.1. EC2 콘솔 접속 및 보안그룹으로 이동

1. AWS EC2 Console로 로그인한다.
2. 대시보드 상단 리소스 하위 인스턴스(실행중)을 클릭한다.
3. 아래 그림과 같이 첫째 Mule API Gateway 인스턴스 ID를 클릭한다.
4. 둘째, 하단에 보안 탭을 클릭한다.
5. 셋째, 보안 그룹을 클릭한다.
   ![alt text](/assets/img/26_select_security_group_aws.png)

#### 18.2.2. 인바운드 규칙 추가

1. 인바운드 규칙 편집을 클릭
   ![alt text](/assets/img/27_edit_inbound.png)
2. 첫째, 규칙 추가를 클릭
3. 둘째, 아래와 같은 Spec으로 inbound 규칙 추가
   - 유형: HTTPS
   - 포트 범위: 443
   - CIDR 블록: 0.0.0.0/0
4. 셋째, 규칙 저장 클릭

#### 18.2.3. Elasticsearch와 통신할 포트 개방

1. Mule Gateway 원격 접속한다.

2. 통신포트 개방 및 리로드

{Elasticsearch 통신 포트}를 상황에 맞게 조정

```bash
$ firewall-cmd --permanent --add-port={{Elasticsearch 통신 포트}/tcp}
$ firewall-cmd --reload
```

### 18.3. Logstash 저장소 추가 및 설치

1. Mule Gateway EC2 SSH 접속 후 아래 명령어를 입력하여 Logstash 저장소 추가

```bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
sudo tee /etc/yum.repos.d/logstash.repo <<EOF
[logstash-8.x]
name=Elastic repository for 8.x packages
baseurl=https://artifacts.elastic.co/packages/8.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```

2. 아래 명령어를 입력하여 Logstash 설치

```bash
# version 8.17.2으로 고정
$ sudo yum install logstash-1:8.17.2-1.x86_64 -y
```

### 18.3. Anypoint Studio ELK 연동 DateFormat 설정

![alt text](/assets/img/41_runtime_manager.png)

1. Anypoint Platform에서 Runtime Manager를 클릭
2. 아래 사진과 같이 Servers 클릭 후 APIGateway 클릭

![alt text](/assets/img/42_settings.png)

3. Setting 탭 클릭
4. plugins 탭 클릭
5. 톱니바퀴 클릭

![alt text](/assets/img/43_specific_setting.png)

6. 상단 토글을 활성화해서 상세 설정을 연다
7. Date Format을 아래와 같이 변경한다.

```
yyyy-MM-dd'T'HH:mm:ss.SSSZ
```

8. Apply를 눌러 적용

![alt text](/assets/img/44_back_to_main.png)

9. Runtime Manager 페이지에서 다시 돌아간다.

10. Setting 탭 클릭

11. plugins 탭 클릭

12. 톱니바퀴 클릭

![alt text](/assets/img/45_gwlog.png)

13. 상단 토글을 활성화해서 상세 설정을 연다

14. Date Format을 아래와 같이 변경한다.

```
yyyy-MM-dd'T'HH:mm:ss.SSSZ
```

### 18.4. log4j2 DateFormat 설정

1. 서버 내 log4j2.xml 변경

```bash
$ sudo vi $MULE_HOME/conf/log4j2.xml
```

3. 아래 두가지 설정을 변경하여 DateFormat을 맞춤

- Console 날짜 형식을 지정
  `<PatternLayout pattern="%-5p %d{yyyy-MM-dd'T'HH:mm:ss.SSSZ} [%t] [processor: %X{processorPath}; event: %X{correlationId}] %c: %m%n"/>`

- ConsoleLogUtil 날짜 형식을 지정
  `<PatternLayout pattern="%d{yyyy-MM-dd'T'HH:mm:ss.SSSZ} %m%n"/>`

```bash
# 기존
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%-5p %d [%t] [processor: %X{processorPath}; event: %X{correlationId}] %c: %m%n"/>
            # 여기 변경
            <PatternLayout pattern="%-5p %d{yyyy-MM-dd'T'HH:mm:ss.SSSZ} [%t] [processor: %X{processorPath}; event: %X{correlationId}] %c: %m%n"/>

        </Console>
        <Console name="ConsoleLogUtil" target="SYSTEM_OUT">

            <PatternLayout pattern="%m%n"/>
            # 여기 변경
            <PatternLayout pattern="%d{yyyy-MM-dd'T'HH:mm:ss.SSSZ} %m%n"/>
        </Console>
    </Appenders>

    <Loggers>

        <AsyncLogger name="org.mule.runtime.core.internal.processor.LoggerMessageProcessor" level="INFO"/>
        <AsyncLogger name="com.mulesoft.agent" level="INFO"/>

        <AsyncLogger name="org.mule.runtime.core.internal.logging" additivity="false" level="INFO">
            <AppenderRef ref="ConsoleLogUtil"/>
        </AsyncLogger>

        <AsyncRoot level="WARN">
            <AppenderRef ref="Console"/>
        </AsyncRoot>

        <AsyncLogger name="com.mulesoft.mule.runtime.gw" level="INFO"/>
        <AsyncLogger name="com.mulesoft.extension.policies" level="INFO"/>
        <AsyncLogger name="com.mulesoft.extension.gateway" level="INFO"/>
        <AsyncLogger name="com.mulesoft.analytics" level="INFO"/>

    </Loggers>

</Configuration>
```

### 18.5. Logstash conf 파일 설정

1. Mule Gateway EC2 SSH 접속 후 mule_gateway_log.conf 생성

```bash
$ sudo vi /etc/logstash/conf.d/mule_gateway_log.conf
```

2. mule_gateway_log.conf 작성
   아래 내용을 복사해서 붙여넣기한다.

```bash
input {
  file {
    path => [
      "/home/ec2-user/mule-enterprise-standalone-4.8.3/logs/events.log",
      "/home/ec2-user/mule-enterprise-standalone-4.8.3/logs/gw-http-events.log"
    ]
    start_position => "beginning"
    sincedb_path => "/dev/null"
    ignore_older => 86400
    stat_interval => 2
    discover_interval => 10
    codec => json {
      target => "parsed_json"
    }
  }
}

filter {
  # 원본 "message" 필드 제거 (ECS와의 충돌 방지)
  mutate {
    remove_field => ["message"]
  }

  # componentLocation 필드 처리: 문자열이면 Hash로 변환 후 그대로 저장
  if [parsed_json][componentLocation] and [parsed_json][componentLocation] != "" {
    ruby {
      code => '
        cl = event.get("[parsed_json][componentLocation]")
        if cl.is_a?(String)
          begin
            cl = JSON.parse(cl)
          rescue JSON::ParserError
            cl = nil
          end
        end
        if cl.is_a?(Hash)
          event.set("[parsed_json][componentLocation]", cl)
        end
      '
    }
  }

  # parts 배열이 존재하면, 문자열이면 Array로 변환한 후 그대로 유지
  if [parsed_json][componentLocation][parts] and [parsed_json][componentLocation][parts] != "" {
    ruby {
      code => '
        parts = event.get("[parsed_json][componentLocation][parts]")
        if parts.is_a?(String)
          begin
            parts = JSON.parse(parts)
          rescue JSON::ParserError
            parts = []
          end
        end
        if parts.is_a?(Array)
          event.set("[parsed_json][componentLocation][parts]", parts)
        end
      '
    }
  }

  # componentLocation 내부의 중첩 객체 처리: typedComponentIdentifier 및 partIdentifier를 객체로 저장
  if [parsed_json][componentLocation] and [parsed_json][componentLocation] != "" {
    ruby {
      code => '
        fields = ["typedComponentIdentifier", "partIdentifier"]
        comp = event.get("[parsed_json][componentLocation]")
        if comp.is_a?(String)
          begin
            comp = JSON.parse(comp)
          rescue JSON::ParserError
            comp = {}
          end
        end
        if comp.is_a?(Hash) and comp["parts"].is_a?(Array)
          comp["parts"].each do |part|
            fields.each do |field|
              if part[field].is_a?(String)
                begin
                  parsed_field = JSON.parse(part[field])
                  if parsed_field.is_a?(Hash)
                    part[field] = parsed_field
                  end
                rescue JSON::ParserError
                  # 파싱 실패하면 원본 문자열 유지
                end
              end
            end
          end
          event.set("[parsed_json][componentLocation][parts]", comp["parts"])
        end
      '
    }
  }

  # timestamp 변환 (밀리초 포함/미포함 모두 지원)
  date {
    match => ["[parsed_json][timestamp]", "yyyy-MM-dd'T'HH:mm:ss.SSSZ", "yyyy-MM-dd'T'HH:mm:ssZ"]
    target => "[parsed_json][timestamp]"
    timezone => "UTC"
  }
}

output {
  elasticsearch {
    hosts => ["https://es.fukso.dev:443"]
    index => "mule-gateway-logs-%{+YYYY.MM.dd}"
    user => "elastic"
    password => "changeme"
    ssl => true
  }
  stdout { codec => rubydebug }
}
```

3. 파일 권한 설정

아래 5개 명령어를 입력하여 순차적으로 파일 권한이 설정되도록 한다.

```bash
# 1️⃣ /home/ec2-user/ 디렉토리를 다른 사용자(logstash)가 접근할 수 있도록 설정
sudo chmod 755 /home/ec2-user/

# 2️⃣ mule-enterprise-standalone-4.8.3 디렉토리를 logstash가 접근할 수 있도록 설정
sudo chmod 755 /home/ec2-user/mule-enterprise-standalone-4.8.3/

# 3️⃣ logs 디렉토리를 logstash가 탐색할 수 있도록 설정
sudo chmod 750 /home/ec2-user/mule-enterprise-standalone-4.8.3/logs/

# 4️⃣ logs 디렉토리 및 내부 파일의 소유자를 logstash로 변경
sudo chown -R logstash:logstash /home/ec2-user/mule-enterprise-standalone-4.8.3/logs/

# 5️⃣ logstash가 로그 파일을 읽을 수 있도록 설정
sudo chmod 640 /home/ec2-user/mule-enterprise-standalone-4.8.3/logs/events.log
```

4. Logstash 실행

```bash
$ sudo systemctl start logstash
```

### 18.6. Logstash 적용 및 테스트

1. 개발되어 있는 Mule Application을 호출하여 문제 없이 정상 작동하는지 확인한다.
2. elasticsearch에서 인덱스가 생성되는지 확인

```bash
# 브라우저에서 elasticsearch의 모든 인덱스를 조회
# 아래 사진과 같이 mule-gateway-log가 생성되면 정상 작동한 것
http://{Elasticsearch 주소}:{포트}/_cat/indices?v
```

![alt text](/assets/img/40_indices.png)

---

## 19. Troubleshooting

### 19.1. Runtime Manager에서 Create cluster failed: [object Object] 발생 시 해결방법

![alt text](/assets/img/25_failed_cluster.png)

1. 웹브라우저에서 오른쪽 클릭하여 검사클릭
2. 네트워크 탭에서 이름 clusters 클릭 후 응답 확인

```
"message": "There are applications already deployed on the following servers: [MuleSoftVM2-aws, MuleSoftVM1-azure]Please undeploy all apps from those servers before adding them to a server group."
```

3. 각서버에 있는 모든 앱을 제거
4. 클러스터 재등록

### 19.2. am-service status: command not found

"13.7.MobaXterm을 사용하여 am 폴더로 이동 후 아래 명령어를 입력해 설정을 한다."를 진행하다가

am폴더에서 `./bin/setup` → `$MULE_HOME/am/bin/am-service start` 이후 아래와 같은 에러 발생

```
/home/ec2-user/mule-enterprise-standalone-4.9.2/am/bin/tools/tool-rpm: line 14: /etc/rc.d/init.d/functions: No such file or directory
/home/ec2-user/mule-enterprise-standalone-4.9.2/am/bin/tools/tool-rpm: line 18: status: command not found

```

해결 방법)

첫 번째 에러는 시스템에 필요한 initscripts 패키지가 설치되지 않았기 때문이다. 아래 명령어로 패키지 설치할 것

1. 패키지 설치: `sudo yum install initscripts`
2. 뮬 재시작: `$MULE_HOME/bin/mule restart`
3. am폴더에서 `./bin/setup` → `./bin/am-service start`

![alt text](/assets/img/46_error-case1.png)
