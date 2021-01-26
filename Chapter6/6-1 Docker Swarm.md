# Docker Swarm

Docker Swarm이란 Docker Container의 관리를 위해 등장한 모드이다.

개발자는 Container의 Lifecycle, Deploy, Start, Stop, Scale out/in/up/down 등을 관리해야 하지만 수십, 수백 개의 Container를 모두 관리하기란 힘든 상황이었다.

또한, 여러 노드에 위치한 Docker Engine을 관리하는 것 또한 문제였다.

Docker Swarm은 이러한 문제점을 해결하기 위한 Docker Built-in Orchestration이다. 

2016년 Docker에 Swarmkit가 추가되고 2017년에 Stack과 Secret이 추가되었다.

# Docker Swarm Feature

Swarm의 의미는 떼, 집단이라는 의미이다. Docker Swarm은 Docker Engine이 설치된 여러 노드를 클러스터링 하고 이를 관리하는 것이다.

Docker Swarm에는 여러 특징이 있다.

1. Docker Engine이 있는 클러스터 관리 : Docker Engine이 설치된 여러 노드 클러스터를 만들고 관리한다. 다른 외부 Orchestration을 필요로 하지 않는다.

2. 분산형 설계 : Docker Swarm에는 Manager 와 Worker 노드가 존재한다. Docker Engine이 설치된 노드라면 두 노드의 역할 모두 가능하다. 어느 노드든 Manager/Worker가 될 수 있으면서 하나에 집중되지 않은 분산형 설계를 사용한다.

3. 선언형(Declarative) 서비스 모델 : 선언형 접근 방식을 사용하여서 다양한 서비스 설계를 원하는대로 정의할 수 있도록 한다.

4. 스케일링(Scailing) : 각 서비스에 대한 실행 수를 관리자가 원하는대로 설정할 수 있다.

5. 원하는 상태로의 조정 : Swarm Manager 노드가 지속적으로 클러스터를 모니터링하면서 실제 상태를 관리자가 원하는 상태로 조정한다. 

6. 멀티 호스트 네트워킹 : 서비스에 대한 Overlay 네트워크를 지정할 수 있다.

7. 서비스 검색 : Swarm Manager 노드는 Swarm의 각 서비스에 대해 고유한 DNS 이름을 할당하고 이를 검색할 수 있도록 한다.

8. 로드 밸런싱 : 각 노드 간에 부하(Load)를 나눌 수 있게끔 Container를 배포할 수 있다.

9. 보안 : Swarm 클러스터 내의 노드들은 모두 TLS 상호 인증 및 암호화를 적용하여 서로 간의 통신에서 보안을 유지한다. 자체 서명 Root 인증서나 CA 인증서를 사용한다.

10. 롤링 업데이트 : 서비스 업데이트를 위한 배포 시 서비스가 항시 유지될 수 있도록 배포하고 배포 시 생기는 지연을 제어할 수 있다. 그리고 이전 버전으로의 롤백 업데이트를 지원한다.

# Docker Swarm Architecture

![image1](https://docs.docker.com/engine/swarm/images/swarm-diagram.png)

< Docker Swarm 구조 > ( 출처 : https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/ )

Docker Swarm은 다음과 같은 구조를 가지며 2가지 노드가 있다. Manager 노드와 Worker 노드이다.

Docker Engine을 가지고 있다면 두 역할을 모두 수행할 수 있다.

Manager 노드는 어플리케이션을 구성하는 서비스 정의를 받고 이를 관리하고 유지하는 기능을 가진 노드이다. Worker 노드들을 모니터링하면서 노드에게 서비스를 배포하고 스케일링한다.

Docker Swarm 클러스태 내에는 여러 Manager 노드가 존재한다. 그리고 이들은 Raft 합의 알고리즘을 통해 상태를 서비스의 내부 상태가 일관되게 유지할 수 있도록 한다.

Raft가 이루어지는 방식은 클러스터 내의 모든 작업을 기록하는 Leader Manager를 선출하는 방식으로 이루어진다. 

선출된 Leader Manager가 모든 작업을 로그로 남기면 Swarm은 다른 Manager 노드에게 복제한다. 그리고 Leader Manager가 예기치 않게 종료되면 다른 Manager가 선출되어 작업을 이어받는다.

Worker 노드는 Manager 노드에게 받은 작업을 수행하고 처리하는 노드이다. 

Worker 노드는 Manager 노드에게 현재 작업 상태를 보고하여 Manager 노드가 원하는 상태를 유지할 수 있도록 한다.

# Docker Swarm Service and Task

