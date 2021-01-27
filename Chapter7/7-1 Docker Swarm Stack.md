# Docker Swarm Stack

6-5를 진행하다보면 docker service create명령어에서 여러 가지 옵션을 이용해서 길게 명령어를 작성해야 한다는 것을 알 수 있다.

아니면 서비스를 만들고 변경해가면서 업데이트를 해줄 수 있지만, 이러한 작업은 서비스의 구성을 보기 쉽지 않고 비효율적이다.

더욱 이런 상황에서 효율적인 방법은 Docker Compose를 사용하는 방법이 있었다.

이러한 Docker Compose와 같이 YAML파일을 이용하는 방법이 Swarm에서는 Stack 이라는 기능으로 존재한다.

Stack 역시 services, volumes, networks 로 이루어진다. 대신 네트워크는 Swarm 특성상 Overlay 네트워크로 구성된다.

6-5에서 진행했던 서비스들을 만드는 것이 https://github.com/dockersamples/example-voting-app/blob/master/docker-stack-simple.yml 에 모두 정의되어 있음을 확인할 수 있다.

이제 이 YAML 파일을 가지고 단 하나의 명령어만 사용하면 볼륨, 네트워크, 서비스 모두 구성된다.

먼저, 구성된 YAML 파일 등을 가져오기 위해 git clone을 활용해서 example-voting-app을 가져온다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/stack1.PNG)

그리고 Stack을 이용해서 어플리케이션을 만든다.

명령어는 docker stack deploy 명령어이다.

```
# docker stack deploy (STACK NAME)
```

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/stack2.PNG)

명령을 사용할 때 --compose-file 혹은 -c 옵션을 사용해서 이용할 YAML 파일을 지정할 수 있다.

생성된 서비스, 볼륨, 네트워크를 ls 명령어로 모두 확인해보면 자동으로 생성된 것을 확인할 수 있다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/stack3.PNG)

하지만 다른 서비스도 존재할 수 있고 여러 개의 Stack으로 만든 어플리케이션이 구동될 수 있다.

그렇기 때문에 원하는 특정 스택에 대해서 확인하기 위해서는 docker stack ps 명령어와 docker stack services 명령어를 사용한다.

```
# docker stack ps (STACK NAME)

# docker stack services (STACK NAME)
```

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/stack4.PNG)

각 Stack의 리스트를 보기 위해서는 docker stack ls 명령어를 사용하면 된다.

```
# docker stack ls
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/stack5.PNG)

마지막으로, docker stack rm 명령어를 사용해서 Stack을 지울 수 있다.

Stack이 지워지면서 서비스, 서비스를 구성하는 Container, 네트워크가 모두 같이 삭제된다.

```
# docker stack rm (STACK NAME)
```

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/stack6.PNG)

하지만 볼륨은 다시 사용하거나 이미 연결된 Container 들이 있을 수 있으므로 삭제 되지 않는다.

# Stack vs. Compose

Docker Swarm의 Stack 기능은 Docker Compose와 매우 유사하다.

Swarm 내에서 구동한다는 차이가 있지만 중요한 차이가 하나 있다.

바로 Stack은 Compose 파일에서의 build를 지원하지 않는다.

즉, Custom Image를 Stack으로 Build하면서 서비스를 구축하는 것은 불가능하다.

Custom Image를 사용하기 위해서는 먼저 Build를 하고 만들어진 Image를 가져와서 사용해야 된다.

그 외에도 YAML 파일의 버전 2는 지원하지 않는다.
