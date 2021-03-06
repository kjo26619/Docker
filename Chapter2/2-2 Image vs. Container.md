# Image vs. Container

Image는 어플리케이션을 구성하는 Binaries, Libarary 그리고 Source code들의 구성이다. 

그리고 Container는 이러한 Image를 실행하고 있는 인스턴스이다.

즉, 동일한 Image를 가진 여러 Container가 존재할 수 있다.

# Nginx web server

Nginx 는 무료 오픈 소스 HTTP 서버이며 HTTP 프록시, 메일 프록시 등의 기능을 제공한다.

가벼우면서 성능을 높이는 데 초점을 둔 웹 서버이다.

이 강의에서는 Nginx 웹 서버를 많이 사용한다.

Nginx 서버의 Image는 https://hub.docker.com 에서 찾아볼 수 있다.

# Container Commands 

Nginx Container를 만들기 위해서 run 명령어를 사용한다.

```
# docker container run --publish 80:80 nginx
```

다음과 같은 결과를 얻을 수 있다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx1.PNG)

Local image 저장소에는 Nginx가 없기 때문에 인터넷에서 받아오는 것을 알 수 있다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx2.PNG)

여기서, --publish 옵션은 -p와 같은 옵션으로 port를 지정하는 것이다. 

port의 구성은 [Host Port:Container Port] 로 구성된다.

하지만 docker container run 명령어로 들어올 경우 Container 안에 진입하면서 Log만 볼 수 있는 상태가 된다.

다른 작업을 하기 위해서는 Ctrl+C를 통해서 빠져 나와야 하는데 그러면 Container도 같이 종료되는 것을 확인할 수 있다.

이를 위해서 --detach 혹은 -d 옵션을 이용해서 Background로 Container가 run 되게끔 하면 된다.

```
# docker container run --detach (IMAGE NAME)
```

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx3.PNG)

현재 Docker에서 실행 중인 Container를 확인하고 싶으면 docker container ls 명령어를 사용하면 된다.

```
# docker container ls
```

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx4.PNG)

종료하고 싶으면 stop 명령어를 사용하면 종료할 수 있다.

```
# docker container stop (COTAINER ID or NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx5.PNG)

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx6.PNG)

종료된 Cotainer까지 포함하여 모든 Container 리스트를 확인하고 싶으면 ls 명령어에 -a 옵션을 붙이면 된다.

```
# docker container ls -a
```

![image7](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx7.PNG)

여기서 run 명령어에 대해서 알 수 있는데 run 명령어는 반드시 새로운 container를 제작한다.

그래서 detach 하지 않은 Nginx와 detach 했던 Nginx 두 개의 Nginx Container가 있는 것을 확인할 수 있다.

기존의 Container를 실행하기 위한 명령어는 start 명령어이다.

```
# docker container start (CONTAINER ID or NAME)
```

![image8](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx8.PNG)

Docker는 Container를 만들 때 Container의 이름을 지정해줄 수 있는데 ls 명령어에 맨 마지막 NAMES가 지정한 이름이다.

지정은 --name 옵션을 통해서 지정한다.

```
# docker container run --name (CONTAINER NAME) (IMAGE NAME)
```

![image9](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx9.PNG)

그리고 logs 명령어를 통해서 자세한 Cotainer log 를 확인할 수 있고 top 명령어를 통해서 Container의 Process를 확인할 수 있다.

```
# docker container logs (CONTAINER ID or NAME)

# docker container top (CONTAINER ID or NAME)
```

![image10](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx10.PNG)

마지막으로, rm 명령어를 사용해서 Container를 제거할 수 있다.

```
# docker container rm (CONTAINER ID or NAME)
```

하지만, 제거할 때 Running 중인 Container는 지울 수 없다. Container를 종료한 뒤 삭제하거나 rm 명령어에서 -f 옵션을 사용하여 제거한다.

![image11](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/nginx11.PNG)

# Docker container run

docker container run은 다음과 같은 순서로 이루어진다.

1. Local Image Cache에 사용하는 Image가 있는 지 확인하고 없으면 Image Repository에서 다운로드 한다.
2. 기본은 latest 버전을 다운로드 한다.
3. 다운로드한 Image를 가지고 새로운 container를 만든 뒤 시작할 준비를 한다.
4. Docker Engine 내부의 Private Network에서 Virtual IP를 가지고 온다.
5. 지정한 Port들끼리 Forwarding 작업을 한다.
6. Image에 지정되어 있는 Dockerfile을 참고하여 Container를 실행한다.
