# Docker DNS

Docker 내부는 매우 동적이고 빠르게 변화하므로 IP 주소에 의존하는 DNS는 이러한 상황에 적합하지 않을 수 있다.

그렇기 때문에 Docker에서는 IP 주소만에 의존하는 것이 아닌 Naming을 통하여 DNS가 동작한다.

이러한 Docker DNS에 의해서 같은 내부 네트워크에 속해 있는 Container 끼리는 서로 이름으로 통신할 수 있다.

![image1]()

- 주의 : Nginx:latest에는 ping이 포함되어 있지 않으므로 apt update && apt install iputils-ping 을 exec 명령어로 실행해서 설치해야 진행할 수 있음.

Ping 결과를 보면 알겠지만, Docker DNS는 새로운 네트워크를 만들면 Naming DNS Resolution이 자동으로 동작하여 

같은 네트워크에 위치한 Container 끼리 이름으로 서로를 찾을 수 있다.

이러한 DNS 서버 방식을 채택한 이유는 Container 들이 서로 생성/소멸을 반복하면서 IP주소가 바뀔 수도 있기 때문이다.

Container의 이름은 많이 바뀌지 않으므로 이러한 DNS 방식이 더욱 효율적이라는 것이다.

또한, 여기서 알아두어야할 점은 기본 Bridge 네트워크에는 DNS 서버가 내장되어 있지 않다.

Bridge 네트워크에서는 Container를 만들 때 수동으로 --link 옵션을 사용하여 서로를 연결해줄 수 있다.

하지만 모든 링크를 설정해주기는 힘드므로 새로운 네트워크를 만들어서 할당하는 것이 보편적이다.

이후에는 Docker Compose를 이용하면 자동으로 새로운 네트워크를 작성하고 할당할 수 있다.

# DNS Round Robin

DNS Round Robint은 각 서버에 부하를 분산하기 위해 여러 IP 주소를 하나의 Alias로 설정하는 방법이다.

즉, 3개의 같은 웹 서버 Container가 있지만 DNS는 하나의 Alias로 묶어준다.

그리고 DNS 요청이 들어오면 3개 중 1개의 Container 주소를 반환한다. Round Robin과 같은 방식으로 반환하기 때문에 DNS Round Robin 이라고 한다.

Docker 에서는 1.11 Engine부터 적용이 되었으며 run 명령어를 할 때 --net-alias 옵션을 추가하여 같은 Alias로 묶어줄 수 있다.

```
# docker container run --network (NETWORK ID or NAME) --net-alias (ALIAS NAME)
```

alias로 여러 Container를 묶은 뒤 curl을 통해서 Container와 연결해보면 다른 Container에게 응답받는 것을 알 수 있다.
