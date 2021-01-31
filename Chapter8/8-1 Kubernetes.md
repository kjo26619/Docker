# Kubernetes

Kubernetes는 컨테이너를 관리할 수 있는 오픈 소스 플랫폼이다.

즉, Docker Swarm과 유사한 Orchestration 이다. Docker Swarm 은 Docker에 내장되어 있는 Orchestration 이고 Kubernetes는 내장은 아니지만 많이 사용되는 Orchestration 이다.

2014년에 구글에서 처음 발표되었으며 2015년에 최초로 출시 되었다. 출시 직후 구글과 리눅스 재단은 파트너십을 맺고 Cloud Native Computing Foundation(CNCF)를 설립하여서 Kubernetes를 관리하고 있다.

Docker Swarm과 Kubernetes는 유사한 기능들을 가지고 있지만 각자 장점이 있다.

Docker Swarm은 Docker 내장이다 보니 Container 관리가 쉽고 빠르며 Docker의 여러 도구들(Docker Compose, Docker CLI 등)과 통합해서 사용할 수 있다.

그에 비해 Kubernetes는 kubectl 이라는 CLI 새로운 명령어가 존재하며 이를 공부하고 사용법을 숙지해야 한다. 그리고 Docker의 여러 도구들과는 같이 사용하기 힘들다.

하지만 이러한 단점을 상쇄시킬 수 있을 장점들이 있는데 Docker Swawrm보다 더 많은 기능을 가지고 있고 모든 OS에서 작동하는 오픈 소스 및 모듈식이며 CNCF의 지원과 활발하게 이루어지는 커뮤니티 등이 있다.

Kubernetes의 목표는 Docker Swarm과 마찬 가지로 노드 들간의 클러스터를 구성하고 서로 연결하여 Container를 쉽게 관리하고 배포할 수 있도록 한다.

# Kubernetes Components

![image1](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

< Kubernetes 의 구성 > ( 출처 : https://kubernetes.io/docs/concepts/overview/components/ )

Kubernetes의 클러스터에는 노드와 Control Plane이 존재한다. Docker Swarm에서 보면 Worker 노드와 Manager 노드로 나뉘었던 것이 그냥 노드와 Control Plane으로 나뉜 것이다.

Control Plane에는 API Server, Cloud Controller Manager, ETCD, Controller Manager, Scheduler가 존재한다.

원래는 Master 노드와 Worker 노드로 나눈 것 같은데, Control Plane 이라는 이름으로 바뀌었다.

1. kube-apiserver

    kuber-apiserver는 Control Plane에서 Kubernetes의 전체 구조를 관리하는 서버이다. 
    
    pod, service, replication 등을 포함하는 API 개체에 대한 데이터를 검증하고 클러스터의 API를 지원한다.
    
    즉, 클러스터 내에 모든 구성들은 kube-apiserver를 통해서 요청을 보내고 응답을 받는다.
    
    Control Plane과 노드 간의 연결을 유지한다.
    
2. etcd

    Key-value 저장소로 노드, pod, 설정, 보안, 주소 등의 정보를 저장해 놓는다.
    
    즉, 클러스터의 모든 데이터에 대해 저장하는 곳이다. 
    
3. kube-scheduler

    Kubernetes에서 실행하는 Container들을 배치한다.
    
    여러가지 요소(하드웨어/소프트웨어/정책/데이터 지역성/노드의 용량, 메모리) 등을 고려하여 가장 알맞은 노드에게 배치할 수 있도록 한다.
    
4. kube-controller-manager

    시스템 내 구성 요소들을 모두 모니터링하고 전체 시스템이 원하는 방향으로 나아갈 수 있게끔 관리한다.
    
    노드, pod, service, replication 등을 관리하는 Controller가 존재한다.
    
5. cloud-controller-manager

    클라우드 관련 제어 로직을 포함하고 있다.
    
    클러스터를 클라우드 공급자의 API에 연결하며, 해당 클라우드 플랫폼과 상호 작용하는 요소와 클러스터와 상호작용하는 요소를 구분한다.
    
    클라우드가 있을 경우에만 사용한다.
    
Control Plane의 관리를 받으며 직접 Container를 실행하고 일을 하는 노드는 Kubelet, Proxy, Container Runtime으로 구성된다.

1. kubelet

    노드를 총 관리할 수 있다. 
    
    노드를 클러스터에 등록하며 pod의 스펙을 가지고 와서 Container를 구성한다.
    
2. kube-proxy

    각 노드에서 실행되는 네트워크 프록시이다. 
    
    간단한 TCP, UDP 및 SCTP 스트림 전달을 수행한다. 즉, 클러스터 내부 또는 외부의 네트워크와 통신을 지원한다.
    
3. Container runtime

    Container를 실행하는 소프트웨어.
    
    여러가지 Container를 지원한다. ( Docker, Containerd, CRI-O , Kubernetes CRI )
