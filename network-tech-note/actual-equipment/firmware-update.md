# Firmware Update

본 프로젝트는 Cisco Catalyst WS-C3650-24TD 기준으로 작성 되었다.

## 목차

* FTP
* TFTP
* SCP
* USB



## 펌웨어 업데이트

* 펌웨어는 IT 장비의 하드웨어와 소프트웨어 사이에서 데이터 전송, 채널 제어 등\
  시스템 효율을 높이기 위해 제작된 소프트웨어이다.
* server의 BIOS, 스토리지의 운영체제 등이 펌웨어에 포함되며 해당 소프트웨어를\
  업데이트 하는 것을 \[펌웨어 업데이트] 라고 부른다.



## 파일 전송

1. 파일 전송 시, 콘솔(관리용 포트)에서는 네트워크 통신이 불가능하므로 스위치의 포트와 PC(노트북)를 연결한 뒤 파일 전송이 가능해진다
2. \[제어판] - \[네트워크 및 인터넷] - \[네트워크 및 공유 센터] - \[연결된 어댑터] - \[속성] - \[인터넷 프로토콜 버전 4(TCP/IPv4)]를 눌러 연결된 포트의 IP를 연결된 스위치 IP와 대역을 맞춰 지정해준 다음 ping 보내 서로 통신이 되는지 확인 후 모든 파일 전송을 시작한다
3. 파일 전송 시에는 네트워크(Wifi)를 끄도록 한다



## FTP

Adminstration FileZilla Server 1.9.4 Version 이용

\[Rights management] - \[Users] → User is enabled 체크 표시

<figure><img src="../../.gitbook/assets/image (181).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1) (4).png" alt=""><figcaption></figcaption></figure>

유저를 새롭게 생성 후, E:\test에 더미 파일(test.img) 파일을 만들어 놨기에 Natvie path를 지정하고 비밀번호를 지정해주면 됨

```bash
Switch# copy ftp://[유저]:[비밀번호]@[IP 주소]/[파일] flash:
```

```bash
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
```

***

## TFTP

Tftpd64를 이용

<figure><img src="../../.gitbook/assets/다운로드.jfif" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

위의 사진 처럼 Current Directory에는 파일 전송을 할 파일이 있는 디렉토리로 지정해주고, Server interfaces에는 어댑터의 IP를 지정해주면 된다(스위치의 포트와 PC 연결 후 IP를 지정해주면 자동으로 뜸)

```bash
Switch# copy tftp://[IP 주소]/[파일] flash:
```

```bash
Destination filename [test.img]? 
Accessing tftp://192.168.1.127/test.img...
Loading test.img from 192.168.1.127 (via Vlan10): !!!
[OK - 10485760 bytes]

10485760 bytes copied in 4.868 secs (2154018 bytes/sec)
Switch#dir flash:

...
31029  -rw-         10485760   Dec 6 2024 04:25:33 +00:00  test.img
```

***

## USB

파일이 들어있는 USB를 스위치에 꽂고 연결 되면 로그가 뜬다

```bash
Switch# copy usbflash0:[파일] flash:
```

```bash
Destination filename [cisco.img]? 
Copy in progress...
CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
CCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
CCCCCCCCCCCCCC
10485760 bytes copied in 1.476 secs (7104173 bytes/sec)

Switch#dir flash:

...
7765  -rw-         10485760   Dec 6 2024 05:07:08 +00:00  cisco.img
```

***

## SCP

SSH설정이 되어있고 SSH 접속이 가능해야 SCP 전송이 가능하다.

SCP는 다른 전송 방법들과 다르게 스위치가 서버가 되어 파일을 주고 받을 수 있다

\[구형 장비]

```bash
C:\\FTP> scp -O [파일] [유저]@[스위치 IP 주소]:[경로]:[파일명]
```

\[신형 장비]

```bash
C:\\FTP> scp [파일] [유저]@[스위치 IP 주소]:[경로]:[파일명]
```

윈도우의 cmd 창에서 진행해야함

```bash
(user2@1.1.1.1) Password:
test.img                                               100%   10MB 287.3KB/s   00:35
Connection to 1.1.1.1 closed by remote host.

-------------------------------------------------------------------------------------
Switch#dir flash:

...
62006  -rw-         10485760   Dec 9 2024 05:07:08 +00:00  test.img
```
