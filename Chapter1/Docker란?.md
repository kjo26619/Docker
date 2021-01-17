# Docekr 란?

Docker는 Container 기반 가상화 플랫폼이다.

Container는 운영 체제 수준의 가상화 인스턴스로 어떠한 소프트웨어를 실행하기 위한 환경을 독립적인 인스턴스로 만든 것이다.

이러한 Container를 Docker가 생성 및 관리하며 더욱 효율적으로 소프트웨어를 구동할 수 있는 환경을 제공한다.

# Container와 VM

VM(Virtual Machine)은 운영 체제 자체를 가상화하는 것이다.

![Image1](https://github.com/kjo26619/Docker/blob/main/Chapter1/Image/virtualization-vs-containers_transparent.png)

< Container vs VM > (출처 : Red Hat https://www.redhat.com/en/topics/containers/containers-vs-vms)

그림 1과 같이 VM은 Hypervisor를 이용하여 Guest OS를 가상화하고 그 곳에서 소프트웨어가 동작한다.

쉽게 얘기하여 컴퓨터 내에 가상으로 새로운 하드웨어를 만들어내고 새로운 운영 체제를 사용하는 것이다.

Container는 운영 체제 수준의 가상화로 소프트웨어 실행에 필요한 라이브러리와 의존성을 포함하는 코드 기반 파일인 'Image'로 구성된다.

