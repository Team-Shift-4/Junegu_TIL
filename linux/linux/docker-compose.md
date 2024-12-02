# Docker compose

본 프로젝트는 ubuntu:latest 버전을 사용한다.

## 도커 컴포즈를 이용한 리눅스 설치

1. 도커설치

{% embed url="https://www.docker.com/products/docker-desktop/" %}

2. Dockerfile과 docker-compose.uml 작성

```docker
dockerfile
# Use Ubuntu as the base image
FROM ubuntu:latest

# Install lecture related packages
RUN apt-get update && apt-get install -y openssh-server sudo systemd systemd-sysv net-tools tcpdump ethtool plocate man-db vim traceroute fdisk dnsutils iputils-ping cron jq
ARG DEBIAN_FRONTEND=noninteractive
RUN apt install ntp -y
RUN yes | unminimize

# Add a user 'myuser' with a password 'password' (You should change this)
RUN useradd -rm -d /home/유저이름-s /bin/bash -g root -G sudo -u 1001 myuser
RUN echo '유저이름:비밀번호' | chpasswd

# Setup SSH
RUN mkdir /var/run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise, the user is kicked off after login
RUN sed -i 's/UsePAM yes/UsePAM no/' /etc/ssh/sshd_config

# Expose the SSH port
EXPOSE 포트번호

# Install dumb-init
RUN apt-get install -y dumb-init

# Start the SSH service
COPY bootstrap.sh /root/
RUN chmod +x /root/bootstrap.sh

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["/usr/sbin/sshd", "-D"]
```



```docker
dockercompose
version: "3.9"

services:
  ubuntu:
    build: .
    ports:
      - "포트번호"
    # command: sh -c "service ssh start && tail -f /dev/null"
    command: sh -c "/usr/bin/dumb-init /bin/bash -c '/root/bootstrap.sh; sleep infinity'"
`
```



3. docker compose up 을 실행하여 컨테이너 실행

```docker
# 컨테이너 실행
docker-compose up -d // 도커 백그라운드 실행
docker-compose up --force-recreate // 도커 컨테이너 새로 만들기
docker-compose up --build // 도커 이미지 빌드 후 compose up

# 컨테이너 내리기
docker-compose down // 컨테이너 stop & 삭제
docker-compose stop
```

4. docker ps 명령어를 사용하여 확인

```docker
docker ps
```

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

5. ubuntu 접속

* root 계정 접속

```docker
docker exec -it [CONTAINER ID] /bin/bash
```

* 도커 파일에서 만든 계정으로 접속

```docker
ssh -p [포트] [유저이름]@localhost
```
