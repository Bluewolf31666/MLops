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


## Volume
>Docker 컨테이너(container)에 쓰여진 데이터는 기본적으로 컨테이너가 삭제될 때 함께 사라지게 됩니다. Docker에서 돌아가는 많은 애플리케이션이 컨테이너의 생명 주기와 관계없이 데이터를 영속적으로 저장을 해야하는데요. 뿐만 아니라 많은 경우 여러 개의 Docker 컨테이너가 하나의 저장 공간을 공유해서 데이터를 읽거나 써야합니다.

>이렇게 Docker 컨테이너의 생명 주기와 관계없이 데이터를 영속적으로 저장할 수 있도록 Docker는 두가지 옵션을 제공하는데요. 첫번째는 Docker 볼륨(volume), 두번째는 바인드 마운트(bind mount)입니다. 이번 포스팅에서는 Docker 컨테이너에 데이터를 저장하는데 사용되는 이 두가지 방법에 대해서 알아보도록 하겠습니다.

![관련이미지](https://docs.docker.com/storage/images/types-of-mounts.png)


## Host volume
>호스트 디렉토리를 컨테이너의 특정 경로에 마운트함

docker run -d -it -p 8090:80 --name my-nginx nginx:1.21

>마운트할 디렉토리 생성
docker run -d -p 8090:80 -v $(pwd)/html:/user/share/nginx/html --name my-nginx nginx:1.23

>마운드 확인
docker inspect my-nginx

cd /usr/share/nginx/html

cat index.html

>결과
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>


# Volume Container
>docker run -d --name my-volume -it -v $(pwd):/html:/usr/share/nginx/html ubuntu

# my-volume 컨테이너의 볼륨을 공유
>docker run -d -p 8090:80 -name my-nginx --volumes-from my-volume nginx

## Docker volume
## host volume은 최초에만 데이터가 연결되므로 추후 반영이 않됨. 즉 바로바로 연동이 되어야 함. 따라서 docker volume을 사용함, 또한 데이터베이스로 연결할수도 있음

docker volume ls #volume 확인

## volume 생성
>docker volume create --name test_db


## 도커 web-volume 볼륨을 nginx의 웹 루트 디렉토리로 마운트
(하기전에 sql port사용중인지 확인하기)
docker run -d --name my-sql -v test_db:/var/lib/mysql -p 3306:3306 mysql:5.7

## 바로 mysql안으로 들어가는법
docker run -d --name my-sql -v test_db:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=12345 mysql:5.7

docker exec -it my-sql bash
 mysql -u root -p
 12345
 