# Cisco NX-OS

본 프로젝트는  GNS3 CiscoNX-OSv90009300v10.3.1 기준으로 작성 되었다.



## 목차

* 사용자 계정 생성(권한, Secret)
* SSH 접속
* 초기화 -> 초기설정
* 패스워드 리커버리(복구)
* IOS와 다른 명령어
* 콘솔 / VTY



## 사용자 계정 생성(권한, Secret)

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

## SSH 접속







## 초기화 (설정 초기화)







## 패스워드 리커버리





















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









