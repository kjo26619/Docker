# Dev, Build, Deploy

잠시 소프트웨어 공학으로 넘어가서 개발 환경에 대해서 알아본다.

소프트웨어 개발 프로세스에서 흔히 나오는 말은 Local, CI, Production 이다.

1. Local : 개발자 자신의 로컬 환경.

2. CI (Continuous Integration) : 빌드 및 테스트 수행 후 개발자가 코드 변경 사항을 많은 팀원이 접속할 수 있는 저장소에서 정기적으로 병합하는 방식

3. Production : 실제로 서비스되는 환경.

실제로 이러한 개발 환경들이 모두 다르다면 충분히 문제는 발생한다.

예를 들어, Local에서는 여러 노드가 없을테니 Swarm을 쓸 이유가 없다. 하지만 Swarm을 사용하지 않으면 Stack과 Secret은 사용할 수 없다. CI환경도 유사하다고 한다. 

그렇다면 무얼 사용해야 하는가? 어떻게 하면 쉽게 구성할 것인가. 그래서 나온 것이 Docker Compose이다.

# Docker Compose

Docker Compose는 설명을 했지만, 개발을 위해서 나온 도구이다.

실제로 Docker Engine에 기본 탑재 되어 있지 않다.

Docker Compose를 이용하면 Stack처럼 쉽게 서비스를 구축한다. 그리고 Secret을 지원해준다.

물론, 파일 기반 Secret만 지원하며 외부에서 정의하는 Secret은 사용할 수 없다. 그리고 Secret이 동작하는 것도 아니다. YAML 파일에서 secrets 섹션이 있어도 돌아가는 것이다.

Docker Compose는 Container의 /run/secrets/<secret_name> 에 평문화된 파일들을 추가해준다. 

대신, 이를 통해서 Swarm을 사용하지 않는 환경에서도 여러 서비스를 한번에 구축하고 Secret을 적용할 수 있다.

이제 Compose에서의 문제는 YAML 파일이다. Local, CI, Production 모두 제각기의 YAML 파일이 필요하다.

그래서 Docker Compose에서는 여러 Compose 파일이 있는 것을 허용한다.

# Docker Compose Multiple Compose File

여러 Compose 파일은 기본적으로 사용하는 docker-compose.yml 파일을 기반으로 나머지 설정을 할 수 있게끔 해주는 방법이다.

이에 대한 자세한 설명은 https://docs.docker.com/compose/extends/ 이 곳을 읽어보면 된다.

docker-compose.yml 에는 필요한 서비스의 이름, 서비스들의 기본 Image 구성을 넣는다. 

그 다음으로 docker-compose.override.yml 이다.

override.yml 파일은 기본 docker-compose.yml 파일의 설정을 가지고 와서 더 알맞게 사용한다.

그래서 환경에 알맞게 YAML 파일을 자세히 작성하고 서비스를 구축할 수 있다.

예를 들어, Swarm을 사용하지 않는 Local 환경에서는 Secret을 사용할 수 없기 때문에 파일 기반 Secret 으로 설정해야 한다.

하지만 Swarm을 사용하는 Production 환경에서는 외부에서 입력한 Secret을 사용할 수 있게끔 YAML 파일을 작성할 수 있다.

이렇게 override 를 통하여 여러 Compose 파일을 만들고 환경에 맞게 작성할 수 있다.

override.yml 파일은 Docker Compose에서 기본적으로 지원하는 파일이기 때문에 docker compose up 만 해주어도 알아서 override.yml 파일이 있으면 읽어서 적용한다.

그 외에 다른 override 파일을 만들고 싶다. 예를 들어서, CI 용 test.yml, Production 용 prod.yml 등을 만들 수 있다.

하지만 이는 Docker Compose에서 기본적으로 지원하는 파일 형식이 아니기 때문에 지정을 해주어야 한다.

지정의 경우에는 -f 옵션을 사용하면 된다.

```
# docker-compose -f (BASE FILE) -f (OVERRIDE FILE) up
```

BASE FILE의 경우에는 docker-compose.yml 이라고 보면 된다. 그리고 OVERRIDE FILE은 자신이 원하는 docker-compose.test.yml, docker-compose.prod.yml 등이다.

이렇게 하여 각 환경에서도 알맞은 YAML 파일을 구축하고 Docker Compose를 사용할 수 있다.
