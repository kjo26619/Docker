# Image vs. Container

Image는 어플리케이션을 구성하는 Binaries, Libarary 그리고 Source code들의 구성이다. 

그리고 Container는 이러한 Image를 실행하고 있는 인스턴스이다.

즉, 동일한 Image를 가진 여러 Container가 존재할 수 있다.

# Nginx web server

Nginx 는 무료 오픈 소스 HTTP 서버이며 HTTP 프록시, 메일 프록시 등의 기능을 제공한다.

가벼우면서 성능을 높이는 데 초점을 둔 웹 서버이다.

이 강의에서는 Nginx 웹 서버를 많이 사용한다.

Nginx 서버의 Image는 https://hub.docker.com 에서 찾아볼 수 있다.

Nginx Container를 만들기 위해서 run 명령어를 사용한다.

```
# docker container run --publish 80:80 nginx
```

