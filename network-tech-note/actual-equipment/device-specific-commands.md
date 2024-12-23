# Device-specific commands

## 1. HP 스위치

| 명령어                           | 설명               |
| ----------------------------- | ---------------- |
| display current-configuration | 현재 설정 확인         |
| display  version              | 장비 및 소프트웨어 버전 확인 |
| display device manuinfo       | 장비 제조 정보 확인      |
| display clock                 | 시간 확인            |
| display cpu-usage             | CPU 사용률 확인       |
| display cpu-usage summary     | CPU 요약 정보 확인     |
| display memory                | 메모리 사용률 확인       |
| display memory summary        | 메모리 요약 정보 확인     |
| display interface brief       | 인터페이스 상태 요약 확인   |
| display vrrp verbose          | VRRP 정보 확인       |
| display arp                   | ARP 테이블 확인       |
| display mac-address dynamic   | 동적 MAC 주소 테이블 확인 |
| display system health         | 시스템 상태 확인        |
| display fan                   | 팬 상태 확인          |
| display power-supply          | 전원 공급 상태 확인      |
| display power                 | 전원 상태 확인         |
| display environment           | 환경 정보 확인         |
| display logbuffer reverse     | 로그 버퍼 확인         |



## 2. Cisco 스위치

### 1. IOS

| 명령어                                                   | 설명         |
| ----------------------------------------------------- | ---------- |
| show running-config                                   | 현재 설정 확인   |
| show version                                          | 버전 정보 확인   |
| show processes cpu                                    | CPU 사용률 확인 |
| <p>show preocesses memory<br>show memory platform</p> | 메모리 사용률 확인 |
| show interfaces status                                | 인터페이스 상태   |
| show environment                                      | 환경 상태 확인   |
| show logging                                          | 로그 확인      |
| show lldp neighbors                                   | 이웃하는 장비 확인 |



### 2. NX-OS

| 명령어                    | 설명         |
| ---------------------- | ---------- |
| show running-config    | 현재 설정 확인   |
| show version           | 버전 정보 확인   |
| show system resources  | CPU 사용률 확인 |
| show system resources  | 메모리 사용률 확인 |
| show interfaces status | 인터페이스 상태   |
| show environment       | 환경 상태 확인   |
| show logging logfile   | 로그 확인      |



### 3. IOS XR

| 명령어                   | 설명         |
| --------------------- | ---------- |
| show running-config   | 현재 설정 확인   |
| show version          | 버전 정보 확인   |
| show processes cpu    | CPU 사용률 확인 |
| show memory           | 메모리 사용률 확인 |
| show interfaces brief | 인터페이스 상태   |
| show environment      | 환경 상태 확인   |
| show logging          | 로그 확인      |

## 3. Dell 스위치

| 명령어                    | 설명               |
| ---------------------- | ---------------- |
| show running-config    | 현재 설정 확인         |
| show version           | 장비 및 소프트웨어 버전 확인 |
| show system            | 시스템 정보 확인        |
| show process cpu       | CPU 사용률 확인       |
| show memory cpu        | 메모리 사용률 확인       |
| show interfaces status | 인터페이스 상태 요약 확인   |
| show ip route          | 라우팅 테이블 확인       |
| show logging           | 시스템 로그 확인        |



## 4. Piolink L4 스위치

| 명령어                 | 설명               |
| ------------------- | ---------------- |
| show running-config | 현재 설정 확인         |
| show version        | 장비 및 소프트웨어 버전 확인 |
| show system         | 시스템 정보 확인        |
| show interface      | 인터페이스 상태 확인      |



## 5. SDN

| 명령어                                   | 설명                  |
| ------------------------------------- | ------------------- |
| show running-config                   | 현재 설정 확인            |
| show version                          | 장비 및 소프트웨어 버전 확인    |
| show clock                            | 시간 확인               |
| show processes cpu history            | CPU 사용 이력 확인        |
| show cpu utiliztion                   | CPU 사용률 확인 (리프 스위치) |
| show processes memory sorted physical | 메모리 사용률 확인          |
| show memory summary total             | 메모리 요약 확인 (리프 스위치)  |
| show environment                      | 환경 정보 확인            |
| show interface status                 | 인터페이스 상태 요약 확인      |
| show logging buffer                   | 로그 버퍼 확인            |



## 6. 한드림넷 스위치

| 명령어                      | 설명               |
| ------------------------ | ---------------- |
| show version             | 장비 및 소프트웨어 버전 확인 |
| show clock               | 시간 확인            |
| show system system-info  | 시스템 정보 확인        |
| show system uptime       | 시스템 업타임 확인       |
| show system cpu-load     | CPU 사용률 확인       |
| show system memory       | 메모리 상태 확인        |
| show system fan          | 팬 상태 확인          |
| show system psu          | 전원 공급 상태 확인      |
| show system temperature  | 온도 확인            |
| show system alarm        | 시스템 경고 확인        |
| show port status         | 포트 상태 확인         |
| show running-config      | 현재 설정 확인         |
| show syslog              | 시스템 로그 확인        |
| show interface bandwidth | 인터페이스 대역폭 확인     |





## 7. 안랩 방화벽

| 명령어       | 설명                  |
| --------- | ------------------- |
| nosinfo   | 버전 정보 확인            |
| uptime    | 시스템 업타임 확인          |
| ifconfig  | 인터페이스 정보 확인         |
| 대시보드      | CPU, 메모리, 디스크 상태 확인 |
| 시스템 정보 메뉴 | NTP 상태 확인           |



## 8. HP 1910 / 1920 스위치

| 명령어                | 설명               |
| ------------------ | ---------------- |
| \_cmdline-mode on  | 슈퍼모드 진입          |
| 512900             | 슈퍼모드 초기 암호       |
| show configuration | 현재 설정 확인         |
| show cpu           | CPU 사용률 확인       |
| show memory        | 메모리 상태 확인        |
| show interface all | 모든 인터페이스 상태 확인   |
| show uptime        | 시스템 업타임 확인       |
| show version       | 장비 및 소프트웨어 버전 확인 |



## 9. 지니안

| 명령어                | 설명               |
| ------------------ | ---------------- |
| show configuration | 현재 설정 확인         |
| show cpu           | CPU 사용률 확인       |
| show memory        | 메모리 상태 확인        |
| show interface all | 모든 인터페이스 상태 확인   |
| show uptime        | 시스템 업타임 확인       |
| show version       | 장비 및 소프트웨어 버전 확인 |
| @shell             | 쉘 모드 진입          |
| gnlogin            | 쉘 모드 종료          |
| top                | CPU 사용량 보기       |















