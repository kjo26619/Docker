# Docker Container Image

Image의 정확한 정의는 

' An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. '

이다. 즉, Container가 실행 중에 사용하기 위한 시스템 정의사항, 의존성, Binaries, 등으로 이루어진 것이 바로 Image이다.

이러한 Docker의 Image는 정확하게 여러 개의 Layer로 이루어 져있다.

# Image Layer

Image의 정확한 사항에 대해서는 Dockerfile에 구성이 되어 있다.

Dockerfile에 환경, 명령어, 실행 등을 정의하고 이를 Build하면 Image로 만들 수 있다.

이러한 Dockerfile에 하나씩 쌓아가는 것이 Layer이다.

예를 들어, Container에서 사용할 기본 Image를 설정한다. 이 때 하나의 Layer가 쌓이는 것이다.

그리고 파일을 추가하는 Layer를 쌓고 그리고 이 파일을 어떻게 작업할 것인지 Layer를 쌓는다.

이렇게 여러 개의 Layer를 통해 하나의 Dockerfile이 되고 이것이 성공적으로 Build되면 하나의 Image가 되어서 여러 Container로 만들 수 있는 것이다.

# Dockerfile

Dockerfile에서는 다양한 명령을 자유롭게 사용할 수 있다.

참조 : https://docs.docker.com/engine/reference/builder/#volume

1. FROM : 새로운 빌드 단계를 시작하고 기본 Image를 설정한다. Dockerfile은 FROM 명령어로 시작되어야 한다.

2. RUN : 현재 Image 위에 있는 Layer 명령을 실행하고 결과를 얻는다.

3. CMD : Container 내에서 특정 명령을 실행

4. LABEL : Image에 Label을 추가

5. MAINTAINER : Image 저자의 이름, 이메일 등을 설정 (사용하지 않음)

6. EXPOSE : Port를 지정하고 연결한다. Container를 run할 때 -p 가 있을 경우 이 명령을 무시하고 재설정한다.

7. ENV : 환경 변수 <key>=<value>를 지정한다.

8. COPY : 파일이나 디렉토리를 복사하여 Image 파일 시스템에 추가한다.

9. ADD : 파일이나 디렉토리를 복사하여 Image 파일 시스템에 추가한다. ADD는 COPY와는 다르게 압축 파일을 해체하여 복사하거나 URL도 사용할 수 있다. 단순한 복사는 COPY를 이용하는 것이 가장 좋다.

10. ENTRYPOINT : Image로 Container를 생성할 때 실행하는 명령. run이나 start를 할 때 실행하는 명령어이다.

11. VOLUME : Container에서 Host의 디렉토리로 접속이 가능하게 할 때 사용한다.

12. USER : Container를 실행하는 사용자를 지정한다.

13. WORKDIR : RUN, CMD, ENTRYPOINT, COPY, ADD 등이 실행될 작업 디렉토리를 설정한다. 

14. ARG : Build 중에 사용되는 변수 설정한다.

15. ONBUILD : Image가 다른 Build의 기반으로 사용될 때 실행할 명령을 추가한다.

16. STOPSIGNAL : 종료하기 위해 Container로 보낼 시스템 호출 신호를 설정한다.

17. HEALTHCHECK : Docker에게 Container가 작동하는 지 확인하기 위해 Container를 테스트 하는 방법을 설정한다. 서버가 실행중이지만 무한 루프 등에 갇혀서 새로운 연결을 처리할 수 없는 등의 문제점을 파악하기 위해 사용.

18. SHELL : 사용할 기본 Shell을 지정한다.

이러한 명령들을 통해 Dockerfile을 구성하고 Image로 Build할 수 있다.

Docker Docs의 Dockerfile 예제

```
# Firefox over VNC
#
# VERSION               0.3

FROM ubuntu

# Install vnc, xvfb in order to create a 'fake' display and firefox
RUN apt-get update && apt-get install -y x11vnc xvfb firefox
RUN mkdir ~/.vnc
# Setup a password
RUN x11vnc -storepasswd 1234 ~/.vnc/passwd
# Autostart firefox (might not be the best way, but it does the trick)
RUN bash -c 'echo "firefox" >> /.bashrc'

EXPOSE 5900
CMD    ["x11vnc", "-forever", "-usepw", "-create"]
```
