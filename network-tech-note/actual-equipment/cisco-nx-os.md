# Cisco NX-OS

본 프로젝트는  GNS3 CiscoNX-OSv90009300v10.3.1 기준으로 작성 되었다.

## 목차

* IOS와 NX-OS의 차이점
* 사용자 계정 생성(권한, Secret)
* SSH 접속
* 초기화 -> 초기설정
* 패스워드 리커버리(복구)
* 콘솔 / VTY



## IOS와 NX-OS의 차이점

### 운영체제 차이점

<table data-full-width="true"><thead><tr><th>특징</th><th>Cisco IOS</th><th>Cisco NX-OS</th></tr></thead><tbody><tr><td>주요 사용 환경</td><td>엔터프라이즈 네트워크 장비 (라우터, 스위치 등)</td><td>데이터센터 장비 (스위치, Nexus 시리즈 등)</td></tr><tr><td>운영 체제 기반</td><td>IOS 전용</td><td>Linux 기반</td></tr><tr><td>디자인 목표</td><td>단순하고 엔터프라이즈 네트워크 환경에 적합</td><td>고가용성, 확장성, 데이터센터 환경에 적합</td></tr><tr><td>모드 구조</td><td>사용자 모드, 특권 모드, 구성 모드 등</td><td>비슷하지만 VDC(Virtual Device Context) 기능 추가</td></tr><tr><td>다중 역할 관리 (RBAC)</td><td>제한적 (Privilege Levels로만 가능)</td><td>RBAC(Role-Based Access Control) 지원</td></tr><tr><td>기본 설정</td><td>기본적으로 모든 기능 활성화</td><td>필요에 따라 기능 활성화 <br>(feature 명령어 필요)</td></tr><tr><td>기능 활성화 방식</td><td>기본적으로 활성화된 상태</td><td>기능을 수동으로 활성화 <br>(ex : <code>feature</code>)</td></tr><tr><td>CLI 명령어 구조</td><td>간단한 명령어</td><td>더 세분화된 명령어 제공</td></tr></tbody></table>

### 명령어 차이점

<table data-full-width="true"><thead><tr><th>기능 / 설정</th><th>Cisco IOS 명령어</th><th>Cisco NX-OS 명령어</th></tr></thead><tbody><tr><td>인터페이스 진입</td><td>interface GigabitEthernet1/0</td><td>interface ethernet 1/1</td></tr><tr><td>VLAN 생성</td><td>vlan 10</td><td>vlan 10</td></tr><tr><td>VLAN 활성화</td><td>기본적으로 활성화</td><td>feature vlan</td></tr><tr><td>라우팅 활성화</td><td>기본적으로 활성화</td><td>feature ospf</td></tr><tr><td>OSPF 설정</td><td>router ospf 1</td><td>router ospf 1</td></tr><tr><td>Port-Channel 생성</td><td>interface port-channel1</td><td>interface port-channel1</td></tr><tr><td>Port-Channel 활성화</td><td>기본적으로 활성화</td><td>feature lacp</td></tr><tr><td>HSRP 설정</td><td>standby 1 ip 192.168.1.1</td><td>hsrp version2</td></tr><tr><td>구성 저장</td><td>write memory 또는 copy run start</td><td>copy running-config startup-config</td></tr><tr><td>장치 재부팅</td><td>reload</td><td>reload</td></tr><tr><td>장치 상태 확인</td><td>show version</td><td>show version</td></tr><tr><td>장치 로그 확인</td><td>show logging</td><td>show logging logfile</td></tr><tr><td>MAC 주소 테이블 확인</td><td>show mac address-table</td><td>show mac address-table</td></tr><tr><td>ARP 테이블 확인</td><td>show ip arp</td><td>show ip arp</td></tr><tr><td>디버깅 활성화</td><td>debug ip ospf events</td><td>debug ospf events</td></tr><tr><td>역할 기반 접근 제어 설정</td><td>Privilege Levels로 설정</td><td>role name &#x3C;role> 로 설정</td></tr><tr><td>CPU / MEM 확인</td><td>show process cpu / <br>show memory platform</td><td>show system resources</td></tr><tr><td>인터페이스 상태 확인</td><td>show interfaces status</td><td>show interface status</td></tr><tr><td>장비의 하드웨어 환경 상태 확인</td><td>show environment all</td><td>show environment</td></tr></tbody></table>



## 사용자 계정 생성

### 사용자 계정 생성 명령어

```
conf t
username 사용자 계정 password 비밀번호 role 롤
```

<figure><img src="../../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>

### 유저 아이디 생성 확인

```
username admin password 5 $5$OLBDNI$/9qn0mZDtAidBxrvIH3j9o2J/HAqjZ6MG5MrEjdiMd0
 role network-admin
 
username ire password 5 $5$ALCLFB$kY41B6ZxcgW1WOwtitH18z2gaW4tpuFT18p5fNuOA84  r
ole priv-15
```

* show running-config 명령어를  통해 알아보니 무조건 IOS의 secret처럼 암호화가 되는 것을 확인할 수 있다.

### 패스워드 설정 양식

```
Password should contain characters from at least three of the following classes: 
lower case letters, upper case letters, digits and special characters.

비밀번호에는 소문자, 대문자, 숫자 및 특수 문자 등 세 가지 클래스 중 하나 이상의 문자가 
포함되어야 한다.

 Length should be at least 8 characters
 
 길이는 8자 이상이어야 한다.
```



### privilege와 role의 차이

1. Cisco IOS의 Privilege Levels
   1. Cisco IOS에서는 privilege levels(권한 수준)을 통해 명령어에 대한 접근 권한을 제어한다.
   2. 기본적으로 16개의 권한 수준(0 \~ 15)이 있으며, 이 중 주로 3개가 기본적으로 사용된다.
      1. Level 0 : 최소 권한 (ex : logout, enable 등 제한된 명령어)
      2. Level 1 : 사용자 수준 (ex : 읽기 전용 명령어 - show, ping)
      3. Level 15 : 관리자 수준 (모든 명령어에 접근 가능)
   3. 관리자가 특정 명령어를 특정 수준으로 할당하여 사용자 맞춤형 권한을 설정할 수 있다.



2. Cisco NX-OS의 Roles
   1. Cisco NX-OS는 **Role-Based Access Control (RBAC)** 방식을 사용하며, 사용자의 역할(Role)에 따라 명령어 및 자원에 대한 접근 권한을 세부적으로 제어한다.
   2. 각 Role은 특정 명령어와 자원(ex : 인터페이스, VLAN 등)에 대한 권한을 정의한다.
   3. 기본 제공 Role
      1. network-admin : 모든 권한을 가진 최고 관리자
         1. network-operator : 읽기 전용 권한을 가진 사용자
         2. vdc-admin 및 cdv-operator : VDC(가상 장치 컨텍스트) 관련 권한



## SSH 접속

















## 초기화 (설정 초기화)

* 초기화 작업은 장비를 공장 초기 상태로 복원하거나 특정 설정만 재구성하기 위해 사용된다

### 공장 초기화 (Factory Reset)

1. running-config와 startup-config 삭제

```
switch# write erase
```

* 이 명령어는 startup-config를 삭제항 장비를 기본 설정으로 되돌린다.

2. 장비 재부팅

```
switch# reload
```

* 재부팅 시 초기 설정 상태로 시작한다.

3. 설정을 저장하지 않고 재부팅 확인

* 만약 설정 저장 여부를 묻는 메시지가 표시되면 \<no> 를 선택한다.

### 특정 데이터만 삭제

* VLAN 정보 삭제
  * NX-OS에서는 VLAN 데이터베이스가 별도로 저장되므로 VLAN 정보를 초기화하려면 아래 명령어 사용한다.

```
switch# clear vlan database
```

### 재부팅 후 상태 확인

1. 재부팅 후 기본 설정인지 확인

```
switch# show running config
```

* 기본 상태로 복원된 경우 거의 비어 있는 설정이 표시된다.

2. VLAN 정보 확인

```
switch# show vlan brief
```

* 모든 VLAN이 초기화되었는지 확인한다.

### 백업

* 초기화를 진행하면 모든 설정과 데이터가 삭제되므로, 필요하다면 초기화 전에 설정을 백업해야한다.
* 설정 백업 명령어

```
switch# copy running-config tftp://<TFTP-서버-IP>/backup-config
```

* 초기화 후 NX-OS는 관리 IP와 로그인 인증 설정이 필요할 수 있다.

```
switch(config)# interface mgmt0
switch(config-if)# ip address <IP주소> <서브넷마스크>
switch(config-if)# no shutdown
```



## 패스워드 리커버리

1. 장비 재부팅
   1. 장비를 재부팅한다.

```
switch# reload
```

* 재부팅 시 현재 설정이 저장되지 않도록 \<no> 를 선택

2. ROMMON 모드로 진입
   1. 재부팅 과정에서 Ctrl + c 또는 Vreak 키를 눌려 ROMMON 모드(boot loader)로 진입한다.
      1. ROMMON 모드에 진입하지 못하면 장비의 전원을 껏다가 다시 켜고 타이밍에 맞게 Break 키를 입력해야한다.
3. NX-OS 부팅 모드 변경
   1. 부트 변수 설정을 수정하여 초기화된 비밀번호 모드로 부팅한다.

```
loader> config terminal
loader> boot nxos bootflash:nxos.XXXX.bin module-password-reset
```

* nxos.XXXX.bin은 장비에 설치된 NX-OS 이미지 파일 이름이고, 정확한 이름은 bootflash: 디렉토리를 확인해 찾을 수 있다.
* 명령 실행 후, 장비는 패스워드 초기화 상태로 재부팅된다.

4. 관리 암호 초기화
   1. 재부팅 후 NX-OS는 비밀번호 없이 로그인 가능하다.
   2. 새로운 관리 암호를 설정한다

```
switch(config)# username admin password <새로운 암호>
```

* 설정 저장

```
switch# copy running-config startup-config
```

5. 부트 모드 복구
   1. 정상 부트 모드로 변경한다.

```
loader> boot nxos bootflash:nxos.XXXX.bin
```



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









