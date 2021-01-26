# Docker Overlay Network

Docker에는 Bridge 네트워크 말고 Overlay 네트워크가 존재한다.

이는 멀티 호스트 환경에서 여러 노드에 있는 Container 끼리 통신하기 위한 네트워크이다.

Overlay 네트워크는 각 노드가 어떠한 네트워크에 속해있든 간에 컨테이너끼리 사설망을 묶어주는 것을 의미한다.

여기서 Docker Swarm을 구성하고 새로운 서비스를 생성할 때 Overlay 네트워크를 지정해주면 각 Container들은 Overlay 네트워크에서 IP를 할당받는다.

이러한 Overlay 네트워크를 사용하면 IPSec을 사용할 수 있는데, IP 패킷이 보안 상 문제도 있고 Tunneling도 지원하므로 좋은 옵션이다.

