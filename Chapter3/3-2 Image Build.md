# Image Build

Dockerfile로 Image를 만들었다면 docker image build 명령어로 Build할 수 있다.

```
# docker image build (PATH or URL)
```

build 할 때에는 필요한 파일들과 Dockerfile이 존재해야 한다.

nginx 를 자신의 Image로 바꾸면서 가지고 있는 index.html을 바꿀 수 있도록 Image를 바꾸어 본다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter3/Image/build1.PNG)

먼저, Dockerfile을 설정한다. FROM을 통해서 Nginx 기본 Image를 가지고 온다.

그리고 작업해야 하는 Container 내부의 directory를 WORKDIR로 설정한다. 이는 Nginx가 가지는 html폴더이다.

그리고 COPY를 통해서 Host에 있는 index.html을 WORKDIR의 index.html로 복사한다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter3/Image/build2.PNG)

Dockerfile과 필요한 파일인 index.html을 같은 폴더 내에 위치시킨다.

그리고 build 명령을 통해서 새로운 Image를 만들어낸다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter3/Image/build3.PNG)

-t는 Tag를 설정하는 옵션이다.

Image의 리스트를 보고 싶으면 docker image ls 명령을 사용하면 된다.

```
# docker image ls
```

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter3/Image/build4.PNG)

다음과 같이 자신이 만든 새로운 Image 파일이 리스트에 추가된다.

그리고 run을 통해서 자신의 Image를 Container로 생성할 수 있다.

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter3/Image/build5.PNG)

생성이 끝나고 80번 Port로 들어가보면 자신이 만든 HTML 파일로 Nginx가 시작되는 것을 확인할 수 있다.

이와 같이 자신이 원하는 다양한 Custom Image를 만들어낼 수 있다.

그리고 이를 Hub에 올리거나 Image에 REPOSITORY 이름이나 TAG를 변경하고 싶을 때에는 docker image tag 명령을 통해 바꿀 수 있다.

```
# docker image tag (SOURCE IMAGE NAME[:TAG]) (TARGET IMAGE NAME[:TAG])
```

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter3/Image/build6.PNG)

