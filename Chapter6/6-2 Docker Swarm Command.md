# Docker Swarm Command

Docker Swarm은 처음에는 활성화가 되어 있지 않다. 그렇기 때문에 실행을 해주어야 하는데 그 때 사용하는 명령어가 docker swarm init이다.

Docker Swarm은 시작하면서 PKI 및 보안 작업을 수행한다.

Manager 노드는 Swarm에 참여하는 다른 노드와의 통신을 보호하는 데 사용되는 키 쌍과 함께 새로운 Root Certificate Authority (CA)를 만든다. 

그리고 추가 노드를 Swarm에 참여시킬 때 사용할 두 개의 토큰, Worker와 Manager 토큰을 만든다. 각 토큰은 Root CA 인증서를 통해 만들어진다.

그리고 새로운 노드가 Swarm에 참여하면 Manager 노드는 참여하는 노드에게 인증서를 발급해준다. 

```
# docker swarm init
```

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm1.PNG)

이제 Docker Swarm이 시작되면서 Manager 노드가 생성이 되고, 노드를 docker node ls 명령어로 확인할 수 있다.

```
# docker node ls
```

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm2.PNG)

현재는 하나의 Manager 노드만 생성되고, 이 Manager 노드가 Leader 노드임을 표시하고 있다.

Swarm 전체 설정이나 노드의 참여에 대해서 확인하기 위해서는 docker swarm (commands) 를 사용하면 되고 Swarm 내의 노드를 확인하거나 관리하기 위해서는 docker node (commands)를 사용하면 된다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm3.PNG)

help 옵션으로 확인해보면 두 명령어 모두 Swarm과 관련된 명령어임을 확인할 수 있다.

다음으로 Swarm에서 사용하는 서비스를 만들어야 한다. 이는 docker service create 명령어를 사용한다.

```
# docker service create (OPTIONS) (IMAGE) (COMMAND) (ARG)
```

docker service를 만드면서 사용할 Container Image를 지정하고 Container에서 수행할 COMMAND를 지정한다.

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm4.PNG)

서비스를 만들게 되면 docker service ls 를 통해서 서비스 목록을 확인할 수 있다.

그리고 나온 서비스 목록에서 서비스의 상태 등을 확인하고 싶으면 docker serivce ps 명령어를 사용하면 된다.

```
# docker service ls
```

```
# docker service ps (SERVICE ID or NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm5.PNG)

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm6.PNG)

![image7](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm7.PNG)

docker serivce ls와 ps를 통해서 현재 동작 중인 서비스에 대해서 자세히 살펴보고 docker container ls 명령어를 통해서 현재 작동중인 Container도 확인할 수 있다.

그 다음 서비스에 대해서 수정한 뒤 업데이트할 것이 있다면 docker service update 명령어를 사용하면 된다.

```
# docker service update (OPTIONS) (SERVICE ID or NAME)
```

![image8](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm8.PNG)

서비스의 Replica를 3개로 업데이트 한 뒤 서비스 목록 등을 확인해보면 Replica가 증가하고 작동하고 있음을 확인할 수 있다.

여기서, docker service update --help 명령어를 해보면 docker update와 유사하나 할 수 있는 옵션이 많은 것을 확인할 수 있다.

실제로 Swarm을 쓰는 이유 중 하나로 각 서비스에 기존 docker update 보다 많은 옵션을 사용할 수 있다는 장점이 있다.

![image9](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm9.PNG)

이 때, docker container rm 명령어로 한 Container를 삭제해본 뒤 서비스의 상태를 확인해본다.

그러면 처음에는 사라졌다가 3개를 유지하기 위해서 새로운 Container를 만들어내는 것을 확인할 수 있다.

이를 통해서 Swarm이 원상태 유지를 할 수 있다는 것을 확인할 수 있다.

![image10](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm10.PNG)

마지막으로 서비스를 삭제하고 싶으면 docker service rm 명령어를 통해 삭제하면 된다.

```
# docker service rm (SERVICE ID or NAME)
```

서비스가 사라진 뒤 시간이 지나면 Container도 같이 종료되고 삭제되는 것을 확인할 수 있다.

![image11](https://github.com/kjo26619/Docker/blob/main/Chapter6/Image/swarm11.PNG)

