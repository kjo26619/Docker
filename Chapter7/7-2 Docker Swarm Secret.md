# Docker Swarm Secret

Docker Swarm의 Secret 기능은 유저 네임, 패스워드, TLS Certificates 와 Key, SSH Key, 중요 데이터, 문자열(최대 500kB) 등을 보낼 때 암호화되지 않은 상태로 보낼 경우 보안에 문제가 생기기 때문에 생긴 기능이다.

Secret 기능은 Swarm 서비스에만 사용이 가능하다. 이 기능을 통하여 데이터를 중앙에서 관리하고 필요한 Container에게 암호화하여 전달하는 등의 일을 할 수 있다.

Swarm에서 Secret이 추가되면 TLS 연결을 통해 Swarm Manager에게 Secret을 보낸다. 그리고 Secret은 암호화된 Raft 로그에 저장이 된다. 

그리고 전체 Raft 로그는 나머지 Manager에게 복제되어 보내지게 된다.

만약, Swarm 서비스에 Secret을 추가하여 보내면 암호화된 파일이 Container 파일 시스템에 마운트되고 해독한다. 즉, 저장은 평문으로 된다.

Container 내의 저장되는 기본 마운트 지점은 /run/secrets/<secret_name> 에 저장된다.

언제든지 서비스를 업데이트하여 Secret을 추가하거나 추가된 Secret을 없앨 수 있다. 물론, 서비스를 종료시킨 다음 Secret을 제거해야 한다.

그리고 이러한 Secret에 대해서 별칭(Alias)를 지정할 수 있다. 즉, 같은 Secret이 다른 별칭으로 Container 내에 존재할 수 있다. 마운트 지점은 /run/secrets/<secret_alias> 이다.

# Secret Command

Swarm에서 Secret 만드는 명령어는 docker secret create 명령어이다.

먼저, Secret으로 만들 데이터가 필요하다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret1.PNG)

데이터가 있다면 명령어를 통해서 Secret을 만들어낸다.

```
# docker secret create (SECRET NAME) (SECRET FILE)
```

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret2.PNG)

기본 문자열이라면 echo와 파이프 라인을 이용해서 쉽게 만들 수도 있다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret3.PNG)

만든 Secret의 리스트를 보기 위해선 docker secret ls 명령어를 사용하면 된다.

```
# docker secret ls
```

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret4.PNG)

만든 Secret에 대해서 자세히 보기 위해서는 docker secret inspect 명령어를 사용한다.

```
# docker secret inspect (SECRET NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret5.PNG)

만든 Secret은 inspect 명령어를 통해 평문으로 확인할 수 있다.

그리고 이제 서비스를 만들 때 Secret을 적용할 수 있다.

docker service create 명령어에서 --secret 옵션을 사용하면 된다.

```
# docker serivce create --secret (SECRET NAME)
```

Secret을 지정하면 /run/secrets/<secret_name>에 저장이 된다. 이 때 DB의 유저 네임, 패스워드 같은 경우 -e 옵션을 이용해서 서비스를 생성하면서 유저/패스워드를 지정할 수 있다.

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret6.PNG)

docker container exec -it를 이용해서 만들어진 서비스 내에 Bash 셸로 들어간 다음 저장된 Secret을 확인해보면 평문인 것을 확인할 수 있다.

![image7](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret7.PNG)

Swarm 내에서는 Manager들이 쉽게 확인할 수 있지만, Container 간에 통신을 할 때는 모두 암호화되어서 보안 상 유리한 것이 바로 Secret 기능이다.

# Secret with Stack

당연한 이야기지만 Stack을 이용해서 YAML파일로 서비스를 구성할 때 Secret을 추가할 수 있다.

이는 YAML파일에서 secrets 섹션을 추가해주면 된다.

![image8](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret8.PNG)

위에서 만들었던 Postgresql을 docker-compose.yml 이라는 YAML파일로 작성한다.

secrets 섹션을 추가하여 각각 Secret 이름과 파일을 지정해준다. 그리고 서비스를 추가할 때 secrets를 추가하고 지정해준다.

환경 변수를 설정하면 위에서 했던 docker service create와 같게 되는 것이다.

![image9](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret9.PNG)

YAML파일에 작성한대로 Secret으로 만들 파일이 위치에 있어야 한다.

그리고 docker stack deploy 명령어를 이용하면 Stack이 Secret과 서비스를 만들어준다.

```
# docker stack deploy -c (YAML FILE) (STACK NAME)
```

![image10](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret10.PNG)

만들어진 Stack을 확인하고 Secret을 확인해보면 모두 자동으로 만들어진 것을 확인할 수 있다.

똑같이 exec를 이용해서 확인해보면 평문으로 저장되어 있는 것을 확인해볼 수 있다.

![image11](https://github.com/kjo26619/Docker/blob/main/Chapter7/Image/secret11.PNG)
