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

