# Docker Swarm 3 Node

Docker Swarm은 각자 노드를 클러스터링하고 관리하는 목적을 가지고 있다.

학습을 위해서 3가지 노드를 가진 Docker Swarm을 사용해본다.

가장 쉬운 방법은 3가지 가상 노드를 만드는 방법이다.

docker-machine을 이용해서 Virtualbox로 노드를 만들면 된다. 이는 꽤 높은 메모리를 요구한다.

자세한 사항은 http://docs.docker.oeynet.com/machine/get-started/#create-a-machine 이 곳을 참고하는 것이 좋다.

그 외 방법은 Docker에서 지원해주는 웹 CLI인 Play-with-docker를 사용하는 것이다. 

https://www.docker.com/play-with-docker

여기서 lab환경에 들어가서 가볍게 docker를 다룰 수 있다. 조금 느리고 불편한 환경이지만 Docker Hub 아이디만 있으면 사용할 수 있다.

그리고 돈을 내고 웹노드를 만들어주는 Digital Ocean이나 클라우드를 사용하는 방법이 있지만, 학습 목적이므로 Play-with-docker면 충분하다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm1.PNG)

처음 들어가면 세션이 4시간 유지되며 노드를 늘릴 수 있고 노드를 늘리면 CLI 환경이 나오는 것을 확인할 수 있다.

만든 노드에서 Docker Swarm을 구성하고 각 노드가 Swarm에 참여하는 것을 보기 위해 먼저, docker swarm init을 이용한다.

```
# docker swarm init
```

이 환경에서는 두 가지 이더넷이 있어서 eth가 2개 있어서 Advertise 주소를 지정해주어야 된다는 에러 구문이 나올 것이다. 

노드들끼리는 192.168.0.x 네트워크에 묶여있으므로 --advertise-addr 옵션을 이용해서 Advertise 주소를 192.168.0.x로 지정한다. 

주소는 세션에 따라 바뀔 수 있다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm2.PNG)

init이 완료되면 Worker 노드로 Swarm에 참여하는 토큰과 명령어가 나온다. 이 명령어를 복사하여 다른 노드에서 사용해본다.

브라우저에서 사용하는 CLI 환경이다 보니 Ctrl+C 복사가 먹히지 않는다. 크롬에서는 Ctrl+Insert 로 복사하고 Shift+Insert로 붙여넣으면 된다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm3.PNG)

붙여넣은 명령어를 다른 노드에서 사용해보면 Swarm에 Worker 노드로 참여했음을 보여준다.

참여가 끝난 뒤 docker node ls를 사용해보면 현재 Swarm에 참여한 노드들의 상태를 확인할 수 있다.

```
# docker node ls
```

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm4.PNG)

Worker로 참여한 노드를 Manager 노드로 바꿀 수 있는 명령어가 있다.

이는 docker node update 명령어의 옵션을 사용하면 된다.

```
# docker node update --role (manager or worker) (NODE NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm5.PNG)

Manager 역할을 지정하고 docker node ls를 사용해보면 MANAGER STATUS가 Reachable로 바뀌는 것을 확인할 수 있다.

이 방법 외에도 아예 Swarm에 참여할 때 Worker 참여 토큰이 아닌 Manager 참여 토큰을 이용해도 된다.

이는 Swarm init한 Leader에서 docker swarm join-token manager 명령어를 사용하면 Worker와 똑같지만 토큰이 다른 명령어가 나오고 이를 다른 노드에서 사용하면 된다.

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm6.PNG)

이제 서비스를 만들어보면 Swarm으로 묶여있는 3개의 노드에 어떻게 서비스와 Container가 배치되는 지 확인할 수 있다.

docker service create 명령어를 사용하여 서비스를 만든다. 그 때, --replicas 옵션을 사용하면 만들 때 Replica의 수를 정할 수 있다.

```
# docker service create --replicas (NO. OF REPLICA) (IMAGE) (COMMAND)
```

![image7](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm7.PNG)

docker service ls 명령어를 사용하면 서비스가 생성된 것을 확인할 수 있다.

그러고 docker node ps 명령어를 사용하면 현재 노드에서 실행 중인 서비스에 대해서 확인할 수 있다.

사용할 때 뒤에 노드의 이름을 적어주면 특정 노드에서 실행 중인 서비스를 확인할 수 있다.

```
# docker node ps

# docker node ps (NODE ID or NAME)
```

![image8](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm8.PNG)

그런데 서비스 이름 뒤에 숫자가 붙으면서 각 노드마다 다른 서비스가 진행되고 있음을 확인할 수 있다.

서비스에 대해서 자세히 확인하기 위해서는 docker service ps 명령어를 사용한다.

```
# docker service ps (SERVICE ID or NAME)
```

![image9](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm9.PNG)

이 명령어를 통해서 각 서비스가 어느 노드들에 위치해있는 지를 확인할 수 있다. 각 노드 3개에 서비스 Replica 3개가 나누어져서 들어가있는 것이다.

실제로, docker container ls를 사용해보면 하나의 노드에 하나의 Container만 만들어져있음을 확인할 수 있다.

![image10](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm10.PNG)

![image11](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/3swarm11.PNG)

노드 1과 2 모두 같은 Swarm 내에 있고 같은 서비스를 가지고 있다. 하지만, 서비스의 Replica가 나누어져서 들어가고 Container는 하나만 만들어져있는 것을 확인할 수 있다.

이를 통해서 Docker Swarm이 노드에 배포할 때는 한 곳에 집중하는 것이 아닌 로드 밸런싱을 하고 있는 것을 확인할 수 있다. 
