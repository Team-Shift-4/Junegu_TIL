# Cisco IOS

본 프로젝트는 Cisco Catalyst WS-C3650-24TD 기준으로 작성 되었다.

## 목차

* 사용자 계정 생성(권한, Secret)
* SSH 접속
* 초기화 -> 초기설정
* 패스워드 리커버리(복구)
* 로그에 연도 표시
* 콘솔 / VTY



## 사용자 계정 생성(권한, Secret)

### 사용자 계정 생성 명령어

```
Switch(config)#username 사용자 계정 (privilege 권한번호) password/secret 비밀번호
```

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>secret</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>password</p></figcaption></figure>

### password와 secret 차이

#### password

* 설명
  * 일반 텍스트로 암호를 설정하며, 기본적으로 약한 암호화(Type 7, 암호화방식)를 사용한다.
* 암호화 수준
  * 가볍고 쉽게 복호화 가능한 Type 7 암호화 방식 사용, 쉽게 디코딩할 수 있어서 보안이 취약하다.
* 단점
  * 디코딩 도구로 쉽게 복호화 가능하므로 민감한 환경에서는 사용 권장되지 않는다.

#### Secret

* 설명
  * 강력한 암호화를 사용하는 암호로, 기본적으로 MD5 해싱(Type 5)를 적용한다.
* 암호화 수준
  * 매우 강력하며, 원래 비밀번호로 복호화할 수 없도록 설계되었다.
* 장점
  * 보안성이 높아 네트워크 환경에서 권장된다.

#### 설정 확인

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

* Show running-config 명령어를 통해 확인 가능하다.
* secret으로 만든 유저의 비밀번호는 암호화로 되어있고 password를 통해서 만든 유저는 \
  텍스트 형식으로 되어있는 것을 확인할 수 있다.

### privilege

* 장비 계정 관리는 사용자별 접근 권한을 설정하고 암호를 관리하여, 네트워크 장비의 보안을 강화하고\
  운영, 효율성을 유지하는 중요한 절차이다.
* privilege를 명시 안했을 경우 \<default 1>로 설정된다.

<table data-full-width="true"><thead><tr><th width="186">Privilege Level</th><th>설명</th></tr></thead><tbody><tr><td>0</td><td>최소한의 권한 수준, 사용자 모드에서만 동작하며 장비에 대한 조회만 가능</td></tr><tr><td>1</td><td>기본 사용자 권한. 일반적인 조회 및 디바이스 상태 점검 가능</td></tr><tr><td>2 ~ 14</td><td>관리자가 필요에 따라 커스터마이징 가능한 중간 권한 수준. 특정 명령어만 허용하도록 설정 가능</td></tr><tr><td>15</td><td>최고 관리자 권한. 모든 명령어에 접근 가능하며, 글로벌 설정 및 구성 변경 가능</td></tr></tbody></table>

* 장비에 등록된 계정은 Switch#show running-config 명령어를 통해 확인 가능하다.



## SSH 접속



### SSH 란?

* SSH는 네트워크 상에서 안전하게 통신하기 위해 사용되는 암호화 프로토콜이다.
* 주로 원격 로그인이나 명령어 실행을 위해 사용되며, 데이터를 암호화해 전송함으로써 보안을\
  강화한다.

1. 보안성
   1. 통신 데이터(명령어, 파일 전송 등)를 암호화하여 전송한다.
   2. 중간자 공격(Man-in-the-Middle)이나 데이터 도청을 방지한다.
2. 인증
   1. 암호 기반 인증 또는 공개 키 기반 인증을 지원한다.
   2. 사용자를 안전하게 인증하고, 신뢰할 수 있는 연결을 만든다.
3. 사용
   1. 원격 장치 관리, 파일 전송(SCP, SFTP), 포트 포워딩 등 다양한 기능을 제공한다.



### SSH 설정

1. 도메인 이름 설정

SSH는 도메인 이름이 필요하다.

```
conf t
ip domain-name 도메인이름
```

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

2. RSA 키 생성

SSH 연결을 위한 암호화 키를 생선한다.

```
crypto key generate rsa
```

* 키 크기를 묻는 메시지가 나오면 1024 또는 2048을 입력한다. (2048 추천)

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3. VTY 라인에서 SSH 활성화

VTY 라인을 SSH로 설정하고, Telent을 비활성화한다.

```
line vty 0 4
transport input ssh
login local
```

4. SSH 버전 설정

SSH 버전을 2로 설정한다. (더 안전한 프로토콜)

```
ip ssh version 2
```

현재 버전을 확인할 때 명령어

```
show ip ssh
```

5. 인터페이스 IP 주소 설정

```
interface vlan 1
ip address 192.168.1.1 255.255.255.0
no shutdown
```

6. 클라이언트에서 SSH 접속

장비가 SSH를 수락하도록 구성했고 클라이언트에서 SSH를 사용해 접속한다.

* PUTTY 또는 SSH 클라이언트를 사용해 접속
  * PUTTY를 실행하고, Host Name에 장비 IP 입력한다.
  * Connection Type으로 SSH 선택한다.
  * Open 클릭 -> 사용자 이름과 비밀번호 입력한다.

PowerShell 또는 Command Prompt 사용

```
ssh 사용자이름@ip주소
```



## 초기화 (설정 초기화)

* Cisco IOS 장비를 초기화 하여 초기 상태로 복구하는 방법은 모든 설정을 지우고 기본값으로 복구하며, 스위치를 처음 사용하는 상태로 만든다.

### 현재 설정 확인 및 백업

(1) 현재 실행 중인 설정을 확인한다.

```
show running-config
```

(2) 필요하다면 설정을 백업한다.

```
copy running-config tftp
```

(3) 로컬 파일로 저장 (콘솔 / 터미널 소프트웨어 사용)

* 텍스트 파일로 저장한다.



### NVRAM 초기화 (설정 지우기)

#### NVRAM에 저장된 설정 파일(Startup-config)을 삭제한다.

```
write erase
```

* 명령어를 입력하면 스위치의 Startup-config 파일이 삭제된다.



### 장비 재부팅

#### 장비를 재부팅하여 초기 상태로 만든다.

```
reload
```

* 재부팅 도중 메시지가 뜨면 Enter를 눌려 확인한다.

```
Proceed with reload? [confirm]
```

* 재부팅 후, 스위치는 기본적으로 Setup Mode 로 들어간다, 이는 스위치가 초기화 상태임을 의미한다.



### VLAN 데이터베이스 초기화 (필요한 경우)

#### VLAN 데이터베이스는 설정 초기화로 지워지지 않을 수 있다.

(1) Flash 메모리에서 VLAN 데이터베이스 삭제

```
delete flash:vlan.dat
```

* 삭제를 확인하는 메시지가 나오면 Enter를 눌러 확인한다.

(2) VLAN 초기화 후 재부팅

```
reload
```



### 초기 상태 확인

#### 장비 재부팅 후 초기화가 완료되었는지 확인한다.

```
show running-config
```



### 주의사항

* 초기화 과정은 복구 불가능
  * 초기화 이후 설정을 복구하려면 반드시 백업 파일이 필요하다.
* VLAN 초기화는 별도로 진행
  * write erase는 VLAN 데이터베이스를 초기화하지 않으므로, VLAN 초기화가 필요하면\
    delete flash:vlan.dat 명령을 추가로 사용한다.



## 패스워드 리커버리

### 패스워드 리커버리 절차

1. 장비를 재부팅하고 ROMMON(ROM Monitor) 모드로 진입
2. 설정 레지스터 값을 변경하여 startup-config를 무시하도록 설정
3. 장비를 부팅하고, 기존 설정을 복구한 뒤 패스워드를 재설정
4. 설정 레지스터를 원래 상태로 복원하고 장비를 재부팅



### 장비 재부팅

* Cisco 장비의 전원을 끄고 다시 켠다.
* 재부팅 중에 터미널 창에 Cisco 부팅 메시지가 나타날 때, ctrl + Break 키를 눌러 ROMMON모드로 \
  진입한다.
  * ROMMON 모드에 성공적으로 들어가면 콘솔창에 해당 메시지가 표시된다.

```
switch: 
```

### 설정 레지스터 값 변경

* ROMMON 모드에서 설정 레지스터 값을 변경하여 startup-config 파일을 무시하도록 설정한다.

```
confreg 0x2142
```

* 장비를 재부팅한다.

```
reset
```

<figure><img src="../../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

### 장비 부팅 후 CLI 접근

* 장비가 재부팅되면, 초기 설정 상태로 부팅된다.
* 메시지가 나타나면 No를 입력하여 초기 설정 모드에 진입하지 않는다.

```
Would you like to enter the initial configuration dialog? [yes/no]: no
```

* Enable 모드로 진입

```
enable
```

* 비밀번호 없이 Enable 모드에 접근이 가능하다.

### 기존 설정 복구

* Startup-config 파일을 Running-config로 복사하여 기존 설정을 복구한다.

```
copy startup-config running-config
```

* 복구된 설정 파일에서 이전 설정을 다시 활성화한다.



### 패스워드 재설정

* 새로운 패스워드를 설정한다.

```
configure terminal
enable secret <새로운 패스워드>
```



### 설정 레지스터 복원

* 설정 레지스터 값을 초기 상태(0x2102)로 복원한다.

```
config-register 0x2102
```



### 변경 사항 저장 및 재부팅

* 변경 사항을 저장

```
write memory
```

* 장비를 재부팅

```
reload
```



### 패스워드 리커버리 비활성화

* 보안 정책에 따라 패스워드 리커버리를 비활성화할 수 있다.

```
no service password-recovery
```



## 로그에 연도표시

### 현재 설정 확인

#### 현재 로그 타임스탬프 설정을 확인한다

```
show running-config | include service timestamps
```

<figure><img src="../../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

현재는 연도가 포함이 안 된 상태로 로그가 나오는 것이 확인된다.

### 연도 표시 활성화

#### (1) Configure Terminal 모드로 진입

```
configure terminal
```

(2) 로그 타임스탬프에 연도 포함

```
service timestamps log datetime msec localtime show-timezone year
```

* 주요 옵션
  * datetime : 날짜와 시간을 로그에 포함
  * msec : 밀리초 단위로 시간 표시
  * localtime : 로컬 타임존 기반 시간 사용
  * show-timezone : 타임존 정보 표시
  * year : 연도를 포함

### 한국 표준시 설정

#### 연도를 표시하더라도 타임존이 올바르지 않으면 시간 정보가 정확하지 않을 수 있다.

(1) 타임존 설정 확인

```
show clock
```

(2) 타임존 설정

```
clock timezone KST 9
```

<figure><img src="../../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

## 콘솔 / VTY&#x20;

#### 콘솔과 VTY는 Cisco 장비에서 장비를 관리하고 접근하기 위한 두가지 주요 인터페이스이다.

<table data-full-width="true"><thead><tr><th width="158">특징</th><th>콘솔(Console)</th><th>VTY*Virtual Teletype)</th></tr></thead><tbody><tr><td>접속 방식</td><td>물리적 연결</td><td>네트워크 연결</td></tr><tr><td>사용 프로토콜</td><td>없음</td><td>Telnet, SSH</td></tr><tr><td>초기 설정 가능</td><td>가능</td><td>불가능 (초기 네트워크 설정 필요)</td></tr><tr><td>보안성</td><td>물리적 접근으로 보안 높음</td><td>Telnet은 보안 취약, SSH는 안전</td></tr><tr><td>동시 접속 가능</td><td>단일 사용자</td><td>다중 사용자</td></tr><tr><td>접속 제한 조건</td><td>장비에 물리적 접근 필요</td><td>네트워크 연결 필요</td></tr><tr><td>주요 사용 목적</td><td>초기 설정, 문제 복구</td><td>원격 관리, 다중 사용자 접근</td></tr></tbody></table>

* 콘솔 : 초기 설정이나 네트워크 문제가 발생했을 때 사용한다.
* VTY : 원격으로 장비를 관리할 때 사용, SSH를 통해 보안을 강화하는 것이 필수이다.

### 콘솔 장 / 단점

* 장점
  * 네트워크 연결이 없어도 CLI 접근 가능
  * 네트워크 설정 오류로 인해 원격 접근이 불가능할 때 유일한 접근 방식
  * 물리적 접근 권한이 있어야 하므로 상대적으로 안전
* 단점
  * 물리적으로 장비에 가까이 있어야 함
  * 원격 관리 불가능



### VTY 장 / 단점

* 장점
  * 원격으로 장비를 관리할 수 있어 효율적
  * 여러 관리자가 동시에 접속 가능
  * 네트워크를 통해 CLI를 사용할 수 있으므로 물리적 접근 불필요
* 단점
  * Telnet은 보안에 취약
  * 네트워크 연결이 필수적이므로 네트워크 문제가 발생하면 접근 불가
  * 초기 설정(예 : IP 설정)이 완료되지 않은 장비에는 접근 불가
