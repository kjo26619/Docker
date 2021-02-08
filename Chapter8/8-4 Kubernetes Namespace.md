# Kubernetes Namespace

Kubernetes에서는 하나의 클러스터만을 구성할 수 있는 것이 아니라 여러 개의 가상 클러스터를 구축할 수 있다.

이것이 바로 Namespace 이다. 

Namespace 끼리는 서로 독립되어 있다. 즉, 같은 Deployments 구성이여도 Namespace가 다르다면 둘은 다른 Deployments가 된다. 

Namespace 들은 모두 고유한 이름을 가져야 한다. kube- 로 시작하는 Namespace는 Kubernetes 시스템 용으로 예약되어 있으므로 사용하면 안된다.

또한 Namespace는 리소스 할당을 통해서 가상 클러스트에게 리소스를 분할해줄 수 있다.

기본 Namespace는 defalut, kube-node-lease, kube-public, kube-system이 있다. 각각은 다음과 같은 역할을 한다.

1. default : Namespace가 없는 객체들의 기본 Namespace (Pod, Service, Deployment 등을 생성할 때 Namespace를 지정하지 않으면 default Namespace로 만들어진다.)

2. kube-system : Kubernetes 시스템에서 필요한 객체들을 위한 Namespace

3. kube-public : 모든 사용자(인증되지 않은 사용자도 포함)가 읽을 수 있는 Namespace. 클러스터 정보, 콘텐츠를 외부에 노출할 때 필요하다.

4. kube-node-lease : 노드 하트비트의 성능을 향상시키기 위한 리스 오브젝트 용 Namespace.

# Namespace DNS

각 Namespace끼리는 DNS가 있어서 통신이 가능하다.

대신, 다른 Namespace 내의 Container에게 통신하거나 연결하기 위해서는 Namespace를 지정해주어야 한다.

이 형식은 <service-name>.<namespace-name>.svc.cluster.local 이다.

# Namespace Command

Namespace를 만들기 위해서는 Deployments, Pods, ReplicaSet 등과 같이 kubectl create 명령을 이용하면 된다.

YAML파일을 이용하거나 직접 명령어로 만들어줄 수 있다.

```
# kubectl create -f (YAML FILE)

# kubectl create namespace (NAMESPACE NAME)
```


