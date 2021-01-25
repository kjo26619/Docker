# Docker Compose Build

Docker Compose 는 Custom Image를 Build할 수 있게끔 지원한다.

build는 docker-compose up 명령어를 사용했을 때마다 Build해주며 설정을 바꾸고 새로 Build하고 싶을 때에는 docker-compose build 명령어를 사용한다.

혹은 docker-compose up --build 옵션을 사용할 수 있다.

```
# docker-compose build

# docker-compose up --build
```

docker-compose build를 통해 Custom Image를 만들기 위해서는 Dockerfile이 필요하다.

Chapter 3에서 설명한 바와 같이 Dockerfile을 만들고 YAML 파일에 추가하면 된다.

```
version: '3.1'

services:
  servicename:
    image:
    build:
```

services 에 사용할 Container를 지정할 때, build를 추가하면 된다.

같은 디렉토리 내에 Dockerfile 이라는 이름을 가진 Dockerfile이 있으면 build: . 만 하여도 그 Dockerfile을 가지고 Image를 Build한다.

그렇지 않을 경우, 다양한 하위 옵션들을 이용해서 Dockerfile을 지정할 수 있다.

```
services:
  servicename:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
```

context를 통해서 디렉토리나 URL을 지정하고, dockerfile을 통해서 Dockerfile의 이름을 지정할 수 있다.

그 외에도 다양한 옵션들이 있는데 https://docs.docker.com/compose/compose-file/compose-file-v3/ 에서 확인할 수 있다.

