# Kubernetes Pods

Kubernetes에서는 Pods라는 것이 나온다. 이는 Kubernetes에서 관리할 때 사용하는 가장 작은 단위이다.

Pods는 하나 혹은 여러개의 Container로 구성되며 사용하는 저장소, 네트워크, Container 실행법 등이 정의되어 있다.

대부분 하나의 Container로 이루어지지만 매우 밀접한 관계를 가진 Container 끼리는 묶어서 하나의 Pod로 설정할 수 있다. ( init, helper 등 )

이러한 Pods를 구성하기 위해서는 YAML 파일을 이용해서 정의해야 한다.

YAML 파일을 보고 Pods를 구성하고 관리하는 것이 Kubernetes의 주 목표이다.

# Replication Controller & Set

Kubernetes의 Controller 중에는 Replication Controller와 Replication Set이 존재한다.

Replication은 Docker Swarm과 마찬가지로 Pods의 복제이다.

이러한 Replication들의 로드 밸런싱과 스케일링을 담당하는 것이 Replication Controller & Set이다.

Replication의 수를 늘리거나 줄여서 유지할 수 있게끔 한다.

Replication Controller와 Replication Set의 차이는 그렇게 많지 않다.

Controller는 Pod의 Labels가 정확하게 일치하는 Pods를 관리한다.

Set은 일치하는 Labels에 여러 조건을 걸 수 있다.

둘의 기능에는 차이가 없지만 Replica Controller보다 Pods를 관리하는 영역이 더 큰 Replica Set으로 대체되고 있다.

# Deployments

Deployments는 인스턴스의 업데이트, 생성 등을 묶어서 관리하는 것이다.

Deployments를 통해서 묶인 Pods는 Docker Swarm에서 서비스로 묶인 Container와 같이 롤링 업데이트를 지원한다.

Deployments에서 Pods를 업데이트하면 자동으로 롤링 업데이트를 해서 다운되는 시간을 없애준다. 그리고 업데이트 수행 도중 문제가 발생하면 다시 롤백해준다.

Deployments는 기본으로 Replica Set을 구성하여 Pods의 수를 관리하고 Replication들을 생성/제거한다.

Kubernetes에서 Pods를 만들고 관리하는 것은 Deployments를 이용하도록 권장하고 있다.
