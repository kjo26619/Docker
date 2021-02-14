# Kubernetes Services

Kubernetes에서 Service는 Docker Swarm에서 의미하는 Service와는 조금 다르다. 

Docker Swarm에서 Service는 여러 Container를 묶고 한번에 관리하는 것이지만 Kubernetes의 경우에는 다른 Pod들 간에 연결과 통신을 위한 기능이다.

즉, Kubernetes에서 Services는 네트워크 서비스이며 Pod에 고유한 IP 주소와 고유 DNS 이름을 제공한다. 그리고 이들에게 들어오는 요청들을 분산해줄 수 있다.

Kubernetes의 노드들의 내부와 외부를 연결해주는 것이 Services이다.

# Services Types

Service는 총 3가지의 타입이 존재한다. ClusterIP, NodePort, LoadBalancer이다.

1. ClusterIP

    CluterIP는 Service의 기본 타입으로 클러스터 내부를 연결해주는 역할을 한다.
    
    각 Service는 Virtual IP를 가지게 되는데 이들이 가진 Port가 Pod와 연결된다.
    
    이렇게 Serivce를 사용하면 Pod끼리의 연결이 용이해지며 부하를 분산할 때도 효과적이다.
    
    ![imageci](https://d33wubrfki0l68.cloudfront.net/27b2978647a8d7bdc2a96b213f0c0d3242ef9ce0/e8c9b/images/docs/services-iptables-overview.svg)
    
    ( 출처 : https://kubernetes.io/docs/concepts/services-networking/service/ )

2. NodePort

    NodePort는 노드 내의 Pod와 외부를 연결해주는 역할을 한다.
    
    NodePort는 노드 내의 포트로 기본적으로 30000 - 32767 범위로 지정된다. 이 범위 내에 Port로 요청이 들어오면 해당하는 Service로 이동하게 된다.
    
    해당되는 Service로 이동하면 Service에서 Pod 까지의 Port를 찾는다. Service가 가지고 있는 Port와 연결된 Pod의 TargetPort로 이동한다.
    
    이러한 Service는 Kubernetes의 노드 전체에 존재하며 지정된 NodePort로 들어오는 요청을 각 노드에 분산시켜서 연결한다.
    
    ![imagenp](https://theithollow.com/wp-content/uploads/2019/01/image-20.png)
    
    ( 출처 : https://theithollow.com/2019/02/05/kubernetes-service-publishing/ )
    
3. LoadBalancer
    
    
    

