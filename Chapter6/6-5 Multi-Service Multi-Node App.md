# Multi-Service Multi-Node Web App

여러 노드에서 여러 서비스으로 이루어진 웹 어플리케이션을 구동해본다.

웹 어플리케이션은 Docker Samples에 있는 example-voting-app 을 활용한다.

https://github.com/dockersamples/example-voting-app

그리고 총 3개의 노드를 Play-with-docker에서 만들어서 Swarm으로 클러스터링해서 사용한다.

# Voting App

![image1](https://github.com/dockersamples/example-voting-app/raw/master/architecture.png)

< example-voting-app > ( 출처 : https://github.com/dockersamples/example-voting-app )

example-voting-app은 다음과 같이 구성되어 있다.

투표를 할 수 있는 Python vote 서비스가 있고 이 vote는 redis 서비스와 연결되어 있다.

vote와 redis 서비스는 frontend 라는 Overlay 네트워크에 묶여 있다.

그리고 투표 결과를 보여주는 Node.js result 서비스가 있고 이 result는 PostgreSQL로 이루어진 db 서비스와 연결되어 있다.

result와 db 서비스는 backend 라는 Overlay 네트워크에 묶여 있다.

그리고 .NET으로 이루어진 worker 서비스가 frontend와 backend 네트워크 모두에 위치해있어서 각 네트워크와 서비스를 연결한다.

정확한 스펙은 다음과 같다.

1. vote
  - image : dockerexamples/examplevotingapp_vote:before
  - port : 5000->80
  - network : frontend
  - replicas : 2
  
2. result
  - image : dockerexamples/examplevotingapp_result:before
  - port : 5001->80
  - network : backend
  - replicas : 1
  
3. worker
  - image : dockerexamples/examplevotingapp_worker
  - network : frontend, backend
  - replicas : 1
  
4. redis
  - image : redis:alpine
  - network : frontend
  - replicas : 1
  
5. db
  - image : postgres:9.4
  - environment : POSTGRES_USER="postgres", POSTGRES_PASSWORD="postgres"
  - volumes : db-data:/var/lib/postgresql/data
  - network : backend

# Service Create

Voting App을 만들기 위해서는 DB 서비스들인 redis와 db 서비스를 만들어야 한다.

그리고 그 전에 frontend와 backend 네트워크를 먼저 구축한다.

docker network create 명령어로 구축하여 --driver나 -d 옵션을 사용하여 Overlay 네트워크로 구축한다.

![image2]()

다음으로 redis 서비스를 만들기 위해 docker service create 명령어를 사용한다.

![image3]()

redis는 frontend 네트워크에 속해야 하므로 --network 옵션을 이용하여 지정해준다.

다음으로 db 서비스를 만든다.

![image4]()

db에서 Volume을 설정하는 방법은 https://docs.docker.com/storage/volumes/ 를 참고하면 된다. --mount 옵션을 사용하여 type과 source, target을 설정한다.

source는 db-data이고 target은 /var/lib/postgresql/data 이다.

그리고 환경 변수를 설정하기 위해 -e 옵션을 사용하여서 지정해준다. 그리고 네트워크는 backend에 속한다.

패스워드 환경 변수 설정이 안되거나 볼륨 지정 시 문제가 있을 경우 작업이 실행이 안될 수 있으니 유의해서 명령어를 작성해야 한다.

다음으로 vote 서비스를 만든다.

![image5]()

vote 서비스는 frontend 네트워크에 속하며 Replicas가 2개 이므로 --replicas 옵션으로 지정을 해주어야 한다.

그리고 -p나 --published-port 옵션을 이용해서 포트를 지정해주어야 한다.

다음으로 result도 거의 유사하게 서비스를 만들면 된다.

![image6]()

마지막으로 worker 서비스를 만든다.

![image7]()

worker 서비스는 네트워크가 frontend, backend 모두에 속하므로 네트워크를 두 개 지정해주어야 한다.

모든 서비스 생성이 완료되면 docker service ls, ps 명령어를 통해서 현재 상태를 확인해볼 수 있다.

![image8]()

각 서비스들이 3개의 노드에 분배되어 위치해 있는 것을 확인할 수 있다.

문제가 생겼을 경우에는 ps 명령어를 통해서 무슨 문제인지 확인할 수도 있다.

![image9]()

![image10]()

결과는 다음과 같이 나온다. 

그리고 어느 IP에서든 포트 5000은 vote로, 포트 5001은 result로 연결 되는 것을 확인할 수 있다.
