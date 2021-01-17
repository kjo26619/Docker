# Docekr 란?

Docker는 Container 기반 가상화 플랫폼이다.

Container는 운영 체제 수준의 가상화 인스턴스로 어떠한 소프트웨어를 실행하기 위한 환경을 독립적인 인스턴스로 만든 것이다.

이러한 Container를 Docker가 생성 및 관리하며 더욱 효율적으로 소프트웨어를 구동할 수 있는 환경을 제공한다.

# Container와 VM

VM(Virtual Machine)은 운영 체제 자체를 가상화하는 것이다.

![Image1](https://github.com/kjo26619/Docker/blob/main/Chapter1/Image/virtualization-vs-containers_transparent.png)

< 그림 1 Container vs VM > (출처 : Red Hat https://www.redhat.com/en/topics/containers/containers-vs-vms)

그림 1과 같이 VM은 Hypervisor를 이용하여 Guest OS를 가상화하고 그 곳에서 소프트웨어가 동작한다.

쉽게 얘기하여 컴퓨터 내에 가상으로 새로운 하드웨어를 만들어내고 새로운 운영 체제를 사용하는 것이다.

Container는 운영 체제 수준의 가상화로 소프트웨어 실행에 필요한 라이브러리와 의존성을 포함하는 코드 기반 파일인 'Image'로 구성된다.

이러한 Container는 VM보다 빠르고 적은 메모리로도 구동할 수 있다는 장점이 있다.

# Docker Edition

Docker는 현재 Linux, Window, MAC, Cloud 등에서 지원하고 있다.

Docker에는 CE(Comunity Edition)와 EE(Enterprise Edition)이 있다. 

CE는 무료 버전이고 오픈 소스이다. EE는 유료 버전으로 대규모 조직 등에서 사용한다. EE 버전의 자세한 내용은 https://www.docker.com/pricing 에서 확인할 수 있다.

그리고 CE 버전에서 release와 관련하여 Stable 과 Edge Build가 있다.

Stable은 분기마다 업데이트를 제공. Edge는 매월 업데이트를 제공한다.

그 외에도 추가되어 Nightly와 Test Build가 있다.

# Docker 설치 ( Linux )

1. Docker Engine

Docker Engine은 Image를 생성하고 Container를 구성하는 daemon과 daemon과의 CLI 명령을 지원하는 API이다.

https://docs.docker.com/get-docker/ 에서 자신의 운영 체제에 맞는 Docker를 선택한다.

참고로 Windows 10 Home 같은 경우 Hyper-V를 지원하지 않으므로 Docker에서 따로 설치를 지원한다. 자세한 내용은 위 링크에서 Windows에 들어가면 나와있다.

Linux 같은 경우 플랫폼에 따라 설치 방법이 달라진다.

하나씩 확인하고 설치를 한다면 docs를 따라가는 것이 맞지만 Docker에서는 이 설치를 자동화해놓은 스크립트가 있다.

이는 https://get.docker.com 에서 확인할 수 있으며 위 주석에서 스크립트를 받는 방법도 나와있다.

2. Docker Machine

Docker Machine은 Mac 또는 Windows에서 Docker를 설치 및 실행할 수 있게 해주며 

로컬의 가상 호스트나 다른 원격 호스트에게 Docker를 설치하고 관리할 수 있게 하는 도구이다.

Docker Machine은 https://docs.docker.com/machine/install-machine/ 에서 설치 방법을 지원한다.

3. Docker Compose

여러 Container를 생성하고 실행시킬 수 있는 도구로 YAML 파일을 이용하여 여러 Container 서비스를 구성할 수 있다.

만약 여러 개의 Microservice로 이루어진 소프트웨어가 있다면 Docker에서 하나씩 running 하기 보다는 Compose로 묶어서 관리하는 편이 효율적이다.

Docker Compose는 https://docs.docker.com/compose/install/ 에서 설치 방법을 지원한다.

4. Docker Hub

자신이 만든 Docker Container Image를 저장하고 관리하고 싶다면 https://hub.docker.com/ 에 가입하여 사용할 수 있다.

hub를 통해 git과도 연결할 수 있고 git과 매우 유사한 UI/UX를 가지고 있다.

자세한 내용은 https://docs.docker.com/docker-hub/ 에서 확인할 수 있다.
