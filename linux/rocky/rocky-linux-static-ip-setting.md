# Rocky Linux Static IP Setting&#x20;

### 본 프로젝트는 Rocky 9 를 기준으로 작성한 문서이다.

* 시작하기 전에 자신의 IP 주소, 넷마스크 주소, 그리고 브로드캐스트 주소를 확인해야 합니다.
* 아이피 주소를 확인하는 명령어는 운영 체제에 따라 다릅니다.&#x20;
* **Windows 운영 체제:**

```
ipconfig
```

* **macOS 운영 체제:**

```
ifconfig
```

* **Linux/Unix 운영 체제:**

```html
ifconfig
```

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

1. 오른쪽 위에 전원 버튼을 클릭 합니다.
2. 그 창에서 "이더넷 (enp0s8) 연결됨"을 클릭합니다.
3. "유선 네트워크 설정" 클릭 합니다.

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

4. 이더넷(enp0s8) 창에서 오른쪽 톱니바퀴를 클릭합니다.

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

5. IPv4 창을 클릭 합니다.
6. 주소를 192.168.56.xxx로 설정하려면 IPv4 설정을 수동으로 변경해야 합니다.

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

7. 이더넷을 끄고 다시 켜준다.

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

8. 연결이 잘 된것을 확인해준다.

* 연결 확인 명령어

```
ping ip주소
```
