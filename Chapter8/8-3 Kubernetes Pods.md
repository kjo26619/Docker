# Kubernetes Pods

Kubernetes에서는 Pods라는 것이 나온다. 이는 Kubernetes에서 관리할 때 사용하는 가장 작은 단위이다.

Pods는 하나 혹은 여러개의 Container로 구성되며 사용하는 저장소, 네트워크, Container 실행법 등이 정의되어 있다.

대부분 하나의 Container로 이루어지지만 매우 밀접한 관계를 가진 Container 끼리는 묶어서 하나의 Pod로 설정할 수 있다. ( init, helper 등 )

이러한 Pods를 구성하기 위해서는 YAML 파일을 이용해서 정의해야 한다.

YAML 파일을 보고 Pods를 구성하고 관리하는 것이 Kubernetes의 주 목표이다.

Pods를 만드는데 사용하는 YAML 파일의 구성은 다음과 같다.

![image1]()

Pods를 만드는 YAML 파일에는 기본적으로 4가지 섹션( apiVersion, kind, metadata, spec )이 있다.

apiVersion은 API Version을 지정하는 것으로 Pods를 만들 때에는 v1으로 설정해야 한다.

그리고 kind는 만드는 객체의 종류를 지정하는 것으로 Pod로 설정해주면 된다.

metadata는 객체가 가지고 있는 이름과 Labels를 지정한다.

그리고 spec에서 사용할 Container들을 지정한다.

그리고 kubectl create 명령을 사용해서 Pods를 만들어내면 된다.

-f 옵션은 파일을 지정하는 옵션이다.

```
# kubectl create -f (YAML FILE)
```

![image2]()

현재 있는 Pods의 리스트를 확인하기 위해서는 kubectl get pods 명령을 사용하면 된다.

그리고 kubectl describe 옵션을 사용하면 현재 Pods의 자세한 상태와 설정을 확인해볼 수 있다.

```
# kubectl get pods

# kubectl describe pod (POD NAME)
```

![image3]()

자신이 사용하는 도구에 따라서 어느 노드가 가져가는 지 확인할 수 있다. ( 저는 Kind를 사용하여서 kind-worker2에 들어간 것을 확인해볼 수 있습니다. )

Pods를 삭제하고 싶을 때에는 kubectl delete 명령을 사용하면 된다.

```
# kubectl delete pods (POD NAME)
```

![image4]()

# Replication Controller & Set

Kubernetes의 Controller 중에는 Replication Controller와 Replication Set이 존재한다.

Replication은 Docker Swarm과 마찬가지로 Pods의 복제이다.

이러한 Replication들의 로드 밸런싱과 스케일링을 담당하는 것이 Replication Controller & Set이다.

Replication의 수를 늘리거나 줄여서 마이크로 서비스가 유지할 수 있게끔 한다.

Replication Controller와 Replication Set의 차이는 그렇게 많지 않다.

Controller는 Pod의 Labels가 정확하게 일치하는 Pods를 관리한다.

Set은 일치하는 Labels에 여러 조건을 걸 수 있다.

둘의 기능에는 차이가 없지만 Replica Controller보다 Pods를 관리하는 영역이 더 큰 Replica Set으로 대체되고 있다.

Replica Set을 만들기 위한 YAML 파일 구성은 다음과 같다.

![image5]()

구성은 Pods와 똑같다는 것을 알 수 있다.

그런데 여기서 Replica Set은 apiVersion이 apps/v1 으로 설정해야지만 만들 수 있다.

그리고 kind는 Replicaset으로 지정해주면 된다.

metadata는 Pods와 다르지 않으며 자신이 원하는 이름과 Labels를 설정해주면 된다.

그리고 spec은 template 옵션을 이용해서 사용할 Pods를 설정할 수 있다. 

그리고 spec에서 필요한 Replica 수를 replicas 옵션으로 설정할 수 있으며, selector 옵션을 통해서 이 Replica Set에 관리될 Pods의 Labels를 지정할 수 있다.

만드는 명령어는 kubectl create 명령어로 Pods와 같다.

![image6]()

대신, Replica Set을 확인하기 위해서는 kubectl get 에서 pods가 아닌 replicaset으로 지정을 해주어야 한다.

```
# kubectl get replicaset
```

YAML 파일에서 지정한 바와 같이 Pods가 3개로 유지되고 있으며 Pods를 삭제하여도 Replica Set이 다른 Pod를 추가하여 수를 유지한다.

![image7]()

그리고 Replica Set을 이용해서 Pods의 Replica 수를 수정할 수 있다. 이를 스케일링이라 하며 kubectl scale 명령어를 사용하면 된다.

```
# kubectl scale --replicas=(NO. OF REPLICA) replicaset (REPLICASET NAME)
``` 

![image8]()

Replica Set은 스케일링 되면 이를 유지하기 위해 Pod를 생성/제거 한다.

kubectl scale 명령어를 사용할 때 -f 옵션을 통해서 YAML 파일을 지정할 수 있다.

```
# kubectl scale --replicas=(NO. OF REPLICA) -f (YAML FILE)
```

![image9]()

Replica Set 역시 kubectl delete 명령어를 사용하면 된다. 그러나, 뒤에 붙는 pods를 replicaset으로 수정해주면 된다.

```
# kubectl delete replicaset (REPLICASET NAME)
```

![image10]()

Replica Set이 삭제되면 속해 있던 Pods도 모두 종료되고 이후에는 삭제되는 것을 확인할 수 있다.

# Deployments

Deployments는 인스턴스의 업데이트, 생성 등을 묶어서 관리하는 것이다.

Deployments를 통해서 묶인 Pods는 Docker Swarm에서 서비스로 묶인 Container와 같이 롤링 업데이트를 지원한다.

Deployments에서 Pods를 업데이트하면 자동으로 롤링 업데이트를 해서 다운되는 시간을 없애준다. 그리고 업데이트 수행 도중 문제가 발생하면 다시 롤백해준다.

Deployments는 기본으로 Replica Set을 구성하여 Pods의 수를 관리하고 Replication들을 생성/제거한다.

Kubernetes에서 Pods를 만들고 관리하는 것은 Deployments를 이용하도록 권장하고 있다.

Deployments를 만드는 명령어도 kubectl create로 같다.

![image14]()

Deployments의 YAML 파일은 Replica Set 구성과 큰 차이가 없다.

kind가 Deployment로 바꾸면 되며, 따로 Replica Set을 지정하지 않아도 자동으로 생성된다.

리스트를 얻을 때는 위와 마찬가지로 kubectl get 을 이용하되 deployments라고 지정해주면 된다.

```
# kubectl get deployments
```

![image11]()

Kubernetes에 있는 모든 Pods, Replica Sets, Deployments 등을 보고 싶다면 kubectl get all을 사용하면 모든 리스트를 한번에 보여준다.

```
# kubectl get all
```

![image12]()

Deployments도 스케일링 할 때는 똑같이 kubectl scale 명령을 사용하면 된다. 

삭제할 때도 kubectl delete를 사용하면 되며 deployments라고 지정을 해주어야 한다.

```
# kubectl delete deployments (DEPLOYMENT NAME)
```

![image13]()
