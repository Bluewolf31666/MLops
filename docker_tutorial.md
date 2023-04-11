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


