docker help


## nginx 이미지 생성
docker create nginx


## pull
wsl@BOOK-4H3SDLR4UV:/mnt/d/docker$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
f1f26f570256: Pull complete
7f7f30930c6b: Pull complete
2836b727df80: Pull complete
e1eeb0f1c06b: Pull complete
86b2457cc2b0: Pull complete
9862f2ee2e8c: Pull complete
Digest: sha256:2ab30d6ac53580a6db8b657abf0f68d75360ff5cc1670a85acb5bd85ba1b19c0
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest


docker run nginx:1.21

## port가 않열려있어서 localhost로 접속이 않된다.

## docker container 강제삭제
docker rm -f [이름]

## 중지된 container 삭제

docker container prune

## 엔트리 포인트와 커맨드
## 컨테이너가 실행될때 고정적으로 실행되는 스크립트 혹은 명령어 -> Entrypoint
## 컨테이너가 실행될때 명령어나 entrypoin에 지정된 명령어에 대한 인자값-> command(cmd)
ex) docker run --entrypoint sh ubuntu

결과)
latest: Pulling from library/ubuntu
2ab09b027e7f: Pull complete
Digest: sha256:67211c14fa74f070d27cc59d69a7fa9aeff8e28ea118ef3babc295a0428a6d21
Status: Downloaded newer image for ubuntu:latest

REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    080ed0ed8312   13 days ago     142MB
ubuntu       latest    08d22c0ceb15   4 weeks ago     77.8MB
nginx        1.21      0e901e68141f   10 months ago   142MB

-->ubuntu가 다운로드됨
docker run --entrypoint echo ubuntu hello ubuntu
>hello ubuntu

## docker에서 환경변수도 사용가능

docker run -it -e MY_HOST=1.1.1.1 ubuntu:latest bash

>root@795b4bfbe945:/#
## bash를 쓰면 container내부로 들어갈수 있음
 echo $MY_HOST
>1.1.1.1


## 파일로 사용시
docker run -it --env-file ./sampe
docker run -it --env-file sample.env ubuntu env


>PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
 HOSTNAME=d618b66d5a0c
 TERM=xterm
 MY_HOST=1,1,1,1
 HOME=/root


## container 내부로 들어가기

docker exec -it my-nginx bash
>root@7b515c48ce13:/#

## 환경변수 확인하기
docker exec my-nginx env
>PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=7b515c48ce13
NGINX_VERSION=1.23.4
NJS_VERSION=0.7.11
PKG_RELEASE=1~bullseye
HOME=/root

## 컨테이너 포트 노출
docker run -p [Host IP:port] :[Container Port] [container]
dicker run -p 9999:80 dev-nginx

docker run -d -p 8090:80 nginx
>localhost:8090으로 인터넷에 접속
>Welcome to nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.


##포트는 않쓰는 포트를 지정해서 넣어주도록 하자 8000->80이라던가
