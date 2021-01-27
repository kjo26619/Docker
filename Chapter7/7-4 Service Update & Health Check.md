# Service Update

Docker에서 Swarm을 이용하여 서비스를 구축하고 실행하고 있으면 서비스를 수정하고 업데이트를 해야되는 순간이 온다.

Swarm에서는 기본적으로 업데이트를 할 때 Rolling 업데이트를 지원한다. 

즉, 서비스 내에 Container나 작업이 있다면, 하나씩 제거하고 수정한 Container나 작업을 구동시킨다. 서비스가 끊어지는 것을 방지하기 위해서이다.

그렇다면 어떻게 Docker Swarm에서 업데이트를 할 수 있는가? 이는 docker service update 명령을 사용하면 된다.

```
# docker service update (SERVICE NAME)
```

이 업데이트는 엄청나게 많은 옵션을 가지고 있다.

이미지를 수정하고 싶으면 --image 옵션, 환경 변수를 추가하거나 삭제하고 싶으면 --env-add, --env-rm 옵션, 포트를 추가하거나 없애고 싶으면 --publish-add, --publish-rm 옵션 등이 있다.

그리고 Replica를 수정하는 방법은 update 보다는 docker service scale 명령어를 사용한다.

```
# docker service scale (SERVICE NAME)=(NO. OF REPLICAS)
```

하지만, Swarm Stack을 사용하면 다르다. Swarm Stack은 이미 YAML 파일에서 상세한 명세를 모두 정의해두기 때문에 YAML 파일을 수정해야 한다.

YAML 파일을 수정하고 똑같이 docker stack deploy 를 사용하면 Swarm이 알아서 서비스에 대해 차이점을 비교하고 Rolling 업데이트를 진행한다.

```
# docker stack deploy -c (YAML FILE) (STACK NAME)
```

# Health Check

2016 년에 추가된 기능인 Healthchecks는 Dockerfile, Compose YAML, docker run, Swarm Stack 등에서 지원한다.

Healthchekcs는 설정한 주기마다 Container 내부에서 정해진 명령어를 사용하고 그 명령어의 수행 상태에 따라서 0(OK) 아니면 1(ERROR)을 반환한다.

즉, Container 상태에 문제가 생겼으면 알려주는 기능이다.

이러한 명령어는 기본적으로 crul localhost 이지만 수정이 가능하다.

이러한 Healthchecks는 docker container inspect 명령어 결과에서 "Health" 하위 옵션에서 확인할 수 있다.

3 가지 상태가 있으며 starting, healthy, unhealthy 이다. 이름과 같이 starting은 시작 중이어서 Healthchecks가 사용되지 않는 상태, healthy는 에러가 없는 상태, unhealthy는 에러가 있는 상태이다.

Dockerfile에서는 HEALTHCHECK 라는 명령을 사용하며 이에 대한 내용은 https://docs.docker.com/engine/reference/builder/#healthcheck 에서 확인해볼 수 있다.

Docker 에서 Container run을 할 때 Dockerfile에 설정된 HEALTHCHECK 내용을 바꿀 수 있는데, 이는 https://docs.docker.com/engine/reference/run/#healthcheck 에서 확인해볼 수 있다.

docker container run 할 때 --health-cmd 옵션을 추가하면 된다.

```
# docker container run --health-cmd="(HEALTHCHECK)"
```

마지막으로, Compose File에서 Healthchecks를 정의하는 방법은 https://docs.docker.com/compose/compose-file/compose-file-v3/#healthcheck 에 자세히 나와 있다.

