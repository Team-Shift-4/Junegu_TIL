# Cisco IOS

본 프로젝트는 Cisco Catalyst WS-C3650-24TD 기준으로 작성 되었다.

## 목차

* 사용자 계정 생성(권한, Secret)
* SSH 접속
* 초기화 -> 초기설정
* 패스워드 리커버리(복구)
* 로그에 연도 표시
* 콘솔 / VTY 설정



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

<table><thead><tr><th width="186">Privilege Level</th><th>설명</th></tr></thead><tbody><tr><td>0</td><td>최소한의 권한 수준, 사용자 모드에서만 동작하며 장비에 대한 조회만 가능</td></tr><tr><td>1</td><td>기본 사용자 권한. 일반적인 조회 및 디바이스 상태 점검 가능</td></tr><tr><td>2 ~ 14</td><td>관리자가 필요에 따라 커스터마이징 가능한 중간 권한 수준. 특정 명령어만 허용하도록 설정 가능</td></tr><tr><td>15</td><td>최고 관리자 권한. 모든 명령어에 접근 가능하며, 글로벌 설정 및 구성 변경 가능</td></tr></tbody></table>

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



## 패스워드 리커버리



## 로그에 연도표시



## 콘솔 / VTY 설정











