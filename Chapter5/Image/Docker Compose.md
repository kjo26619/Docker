# Docker Compose

Docker Compose는 여러 컨테이너를 정의하고 관리할 수 있는 도구이다.

Docker Compose는 YAML 파일을 사용하여 서비스를 구축하고 단일 명령으로 서비스를 시작할 수 있다.

YAML 파일에는 사용할 Container, Network, Volume을 설정할 수 있다.

# Docker Compose YAML File

YAML 파일은 3가지 주요 섹션으로 이루어져있다. services, networks, voulmes 이다.

맨 처음에는 version을 필요로 한다. 그 다음으로 services, volumes, networks에 대해 기술하면 된다.

기본적으로 사용하는 YAML파일은 다음과 같다.

```
version: '3.1'

services:
  servicename:
    image:
    command:
    environment:
    volumes:
  servicename2:

volumes:

networks:
```

1. version

    현재까지는 1에서부터 2.x, 3.x까지 나와있으며, version을 기입하지 않을 경우 1로 간주한다.

    버전에 따라서 기능들이 추가가 되는데 기능이 필요할 경우 2.x, 3.x를 명시하여 사용한다.

    정확한 버전에 대한 내용은 https://docs.docker.com/compose/compose-file/compose-versioning/ 에서 확인할 수 있다.
  
2. services

    services에는 사용할 Container 서비스를 나열한다. 여러 개의 서비스를 나열할 수 있다.
    
    1) servicename : Container의 이름이며 Docker DNS에서 사용하는 이름이다. 원하는 이름으로 지정하면 된다. 
    
    2) image : Container가 사용할 Image를 선택한다. run과 흡사하여 'redis' 이렇게 사용하면 latest를 정의한다. 뒤에 버전을 붙일 수 있다. ex) 'nginx:1.11' 
    
    3) command : Image에서 사용하는 기본 CMD를 대체할 command를 설정한다. (Optinal)
    
    4) environment : docker run 할 때 -e 옵션과 같으며, 환경 변수 값을 지정할 수 있다. (Optinal)
    
    5) volumes : docker run 할 때 -v 옵션과 같으며, 사용할 volume이나 bind mounts를 지정할 수 있다. (Optinal)
    
3. volumes (Optinal)
    
    docker volume create 명령어와 같으며, 서비스에서 사용할 volume을 생성한다.

4. networks (Optinal)

    docker network create 명령어와 같으며, 서비스에서 사용할 network를 생성한다.
    
이 외에도 Container 구성을 위한 많은 설정들이 있다. 정확한 설정은 위에 있는 버전에 대한 docs를 참고하면 된다.
