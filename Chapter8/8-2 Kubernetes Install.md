# Kubernetes Install

Kubernetes는 Swarm과 같이 노드 간에 클러스터를 구성하고 Container 들을 관리하는 Orchestration이다.

그렇기 때문에 학습을 목적으로 사용한다면 Swarm과 똑같이 여러개의 VM이 필요하다.

아무래도 학습 목적으로 사용하는 컴퓨터에서 여러개의 VM을 만들고 실행하는건 무리가 있으니 Play-with-docker 처럼 브라우저에서 Kubernetes를 사용할 수 있는 방법들이 있다.

https://labs.play-with-k8s.com/

https://www.katacoda.com/

기존에 Play-with-docker를 사용했다면 Play-with-k8s가 같은 환경이고 Docker 아이디로 사용할 수 있으니 추천한다.

하지만, 브라우저는 자신의 작업이 세션이 종료되면 사라지게 되는 문제가 있다.

이러한 문제를 해결하기 위해 싱글 노드에서 Kubernetes를 테스트하기 위한 도구가 있다. 그것은 Kind, Minikube, Microk8s이다.

https://kind.sigs.k8s.io/docs/user/quick-start/

https://minikube.sigs.k8s.io/docs/start/

https://microk8s.io/

Kind는 Docker Container를 노드로 사용할 수 있게 하여 Kubernetes 클러스터로 실행하는 도구이다. 자체 테스트 혹은 Local 및 CI 테스트 용으로 사용한다.

Minikube는 Kind와 마찬가지로 Local Kubernetes 클러스터를 빠르게 설정해준다. 쉽게 설치할 수 있으며 Docker 뿐만 아니라 다양한 가상화 툴들을 지원한다.

Microk8s는 경량 Kubernetes를 목표로 한다. 그래서 Local 개발, Prototyping, CI/CD 용으로 많이 사용된다. Linux에서만 사용 가능하다.

# Kubeadm

여러 개의 노드를 구성하였고 Kubernetes를 설치한다면 Kubeadm이라는 클러스트 구축 도구가 있다.

apt나 yum같은 패키지 관리자로 kubeadm을 설치한 다음, kubeadm init을 이용하면 된다.

Docker Swarm처럼 원하는 Advertise Address를 설정하려면 --apiserver-advertise-address 옵션을 사용하면 된다.

```
# kubeadm init

# kubeadm init --apiserver-advertise-address (IP ADDRESS)
```

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter8/Image/install1.PNG)

kubeadm init으로 클러스터 구성이 완료되고 시작한 노드가 Control Plane이 되었다면 Docker swarm과 마찬가지로 밑에 join하는 방법이 나올 것이다.

그리고 Root가 아닌 사용자일 경우 kubectl을 작동하기 위한 명령어도 위에 나올 것이다.

참고로 kubectl이나 kubelet의 경우 kubeadm과 같이 설치되지 않으므로 사용하려면 반드시 설치해주어야 한다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter8/Image/install2.PNG)

Worker 노드를 지정하기 위해 join 명령어를 사용해서 지정해주면 된다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter8/Image/install3.PNG)

이와 같이 구성하면 노드가 Kubernetes에 속하게 되는 방법이다.

더욱 자세한 내용은 https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/ 에서 확인할 수 있다.

# Kubectl & Kubelet

kubectl은 Kubernetes용 Command Line 도구이다.

패키지 관리를 이용한 설치 법은 https://kubernetes.io/docs/tasks/tools/install-kubectl/ 에서 자세히 설명해준다.

kubelet은 클러스터 내의 모든 노드에서 실행되고 Pod 및 Container 시작과 같은 작업을 하는 요소이다.

kubectl과 같은 방법으로 설치하면 되며 노드에서 다양한 기능을 kubelet으로 지정할 수 있다. 

join 역시 kubelet이 필요하므로 kubeadm, kubectl, kubelet은 사용하는 노드에서는 반드시 설치해주는 것이 좋다.
