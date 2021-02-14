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

YAML 파일은 apiVersion이 v1이고 kind를 Namespace로 설정한 뒤 metadata에서 이름을 설정해주면 된다.

![image1]()

![image2]()

만들어진 Namespace의 목록을 확인하고 싶으면 kubectl get namespace 명령어를 사용하면 된다.

```
# kubectl get namespace
```

![image3]()

그리고 Namespace를 정하고 Pod, ReplicaSet, Deployment 등을 만들기 위해서는 기존의 명령어에서 --namespace 옵션을 추가해주면 된다.

Pod를 만들기 위해 임의의 YAML 파일을 지정한다.

```
# kubectl create -f (YAML FILE) --namespace=(NAMESPACE NAME)

# kubectl get pods --namespace=(NAMESPACE NAME)
```

![image4]()

![image5]()

--namespace 옵션을 추가하면 원하는 Namespace의 상태를 확인할 수 있다.

Kubernetes에서 처음 Namespace는 default로 설정되어 있어서 Namespace를 지정해주지 않으면 default Namespace에서 모든 작업이 이루어진다.

만약 기본으로 처리되는 Namespace 설정을 바꾸고 싶다면 kubectl config 명령어를 통해서 지정할 수 있다.

```
# kubectl config set-context $(kubectl config current-context) --namespace=(NAMESPACE NAME)
```

![image6]()

처음에 kubectl get pods 명령어로 확인할 때는 아무런 Pod가 없다. 왜냐하면 dev Namespace에 Pod를 만들었고 default에는 만들지 않았기 때문이다.

그래서 --namespace 옵션으로 dev를 지정해서 확인해보면 myapp-pod가 있다.

그리고 kubectl config 명령을 이용하여 dev가 기본 Namespace가 되도록 설정하면 kubectl get pods 명령어에서 --namespace 옵션을 지정하지 않아도 Pod가 확인이 된다.

# Resource Quota

가상 클러스터인 Namespace는 자원에 제한을 줄 수 있다.

이것이 제한하는 할당량을 의미하는 Quota이며 Kubernetes에서는 ResourceQuota라고 한다.

ResourceQuota 역시 YAML 파일로 지정할 수 있다. 그리고 생성할 때는 kubectl create -f 명령어를 사용하면 된다.

![image7]()

ResourceQuota를 확인할 때에는 kubectl get resourcequota 명령어를 사용하면 된다.

```
# kubectl get resourcequota
```

![image8]()

ResourceQuota에 대한 자세한 설정은 https://kubernetes.io/docs/concepts/policy/resource-quotas/ 에서 확인할 수 있다.

Compute 뿐만 아닌 Gpu, Storage 설정과 Namespace의 적용 범위 등이 더욱 자세히 설정해줄 수 있다.
