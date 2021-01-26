# Docker Overlay Network

Docker에는 Bridge 네트워크 말고 Overlay 네트워크가 존재한다.

이는 멀티 호스트 환경에서 여러 노드에 있는 Container 끼리 통신하기 위한 네트워크이다.

Overlay 네트워크는 각 노드가 어떠한 네트워크에 속해있든 간에 컨테이너끼리 사설망을 묶어주는 것을 의미한다.

여기서 Docker Swarm을 구성하고 새로운 서비스를 생성할 때 Overlay 네트워크를 지정해주면 각 Container들은 Overlay 네트워크에서 IP를 할당받는다.

이러한 Overlay 네트워크를 사용하면 IPSec을 사용할 수 있는데, IP 패킷이 보안 상 문제도 있고 Tunneling도 지원하므로 좋은 옵션이다.

Docker Overlay 네트워크를 확인하기 위해 기존에 했던 5-2에서의 Drupal 과 Postgresql 의 연결을 Swarm 3 Node에서 진행한다.

네트워크를 만들기 위해서 docker network create를 사용하며, Overlay임을 지정해주기 위해 --driver 옵션을 사용한다.

```
# docker network create --driver overlay (NETWORK NAME)
```

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/overlay1.PNG)

docker network ls 명령어를 통해 Overlay 네트워크가 제대로 만들어졌는지 확인할 수 있다.

그리고 docker service create 명령어를 사용하여서 서비스를 만든다. 이 때, --network 옵션을 사용하면 만들면서 특정 네트워크에 속하게 할 수 있다.

```
# docker service create --network (NETWORK NAME) (IMAGE)
```

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/overlay2.PNG)

--name 옵션은 서비스의 이름을 지정해주는 것이고 -e 옵션은 환경 변수를 지정해 주는 것이다.

Web의 역할을 하는 Drupal과 DB의 역할을 하는 Postgresql 서비스를 각각 만든다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/overlay3.PNG)

Play-with-docker 에서 진행하면 열린 포트가 위와 같이 나오게 된다. 가상 노드를 생성하거나 웹 노드를 만들었다면, 브라우저에서 IP:포트 로 접속하면 5-2에서 했던 Drupal 설정이 나온다.

모든 설정을 마치고 나서 Play-with-docker에서 다른 노드를 클릭해보면 역시 8080으로 포트가 열려있음을 확인할 수 있다. 포트로 들어가면 Drupal 홈페이지가 나온다.

가상 노드나 웹 노드의 경우, 다른 노드의 IP:포트 로 접속하여도 역시 Drupal 홈페이지가 나온다는 것을 확인할 수 있다.

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/overlay4.PNG)

서비스의 Overlay 네트워크를 이용해서 각 Container들이 다른 노드에 존재해도 서로 통신하기 때문에 아무 노드의 IP로 접속을 해도 같은 결과를 얻을 수 있는 것이다.

docker network inspect 명령어를 사용해서 네트워크에 대해 자세히 확인해보면 각 Container들이 같은 대역의 네트워크를 할당받았음을 확인할 수 있다.

그리고 같은 서비스를 가진 노드들의 주소를 Peers 부분에서 확인해볼 수 있다.

```
# docker network inspect (NETWORK NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/overlay5.PNG)

# Routing Mesh

Overlay 네트워크가 Container들 끼리 같은 네트워크로 묶고 서로 통신하는 것을 의미한다면 내부와 외부는 어떻게 통신하는가가 의문이 생긴다.

외부에서 포트로 들어오는 것을 허용하고 알맞은 Container를 찾아서 유저와 서비스를 연결해주는 것은 Routing Mesh 라는 기능으로 이루어진다.

Routing Mesh는 Docker Swarm에서 외부 포트와 내부적으로 Container들을 찾아서 연결해주는 역할을 한다.

![image6](https://docs.docker.com/engine/swarm/images/ingress-routing-mesh.png)

< Routing Mesh > ( 출처 : https://docs.docker.com/engine/swarm/ingress/ )

Docker Swarm을 사용하면 모든 노드는 ingress 라는 Routing Mesh에 참여한다.

이러한 ingress 네트워크는 Overlay 네트워크이다.

그리고 서비스를 생성할 때 -p 명령어를 통해서 Swarm의 서비스 용 포트를 열게 된다. 그림에서의 8080 포트와 같다.

외부에서 IP와 포트를 통해서 서비스를 요청하면 Swarm Load Balancer가 적절한 Container로 연결을 해준다. 

유의해야될 점은 이러한 로드 밸런서가 Stateless 라는 점이다. 즉, 세션을 유지하지 않는다. 세션을 유지해야 한다면 추가 기능을 사용해야 한다. 

