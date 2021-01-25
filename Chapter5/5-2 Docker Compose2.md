# Docker Compose

조금 더 심화적인 부분을 다루기 위해서 Drupal Container와 Postgresql Container를 Docker Compose를 통해 만들고 둘을 연결하는 작업을 진행해본다.

Drupal에 대한 Image 내용은 https://github.com/docker-library/drupal 에서 확인할 수 있다.

Postgresql에 대한 Image 내용은 https://github.com/docker-library/postgres 에서 확인할 수 있다.

구성에 대한 내용은 Docker Hub에서 확인할 수 있으며, 각각 https://hub.docker.com/_/drupal/ 과 https://hub.docker.com/_/postgres 에서 확인할 수 있다.

Docker Compose를 위해 YAML 파일을 구성한다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal1.PNG)

ports는 run에서의 -p 옵션과 같으며 포트 포워딩을 지정한다. volumes는 Drupal의 Docker Hub에 들어가면 설정하는 방법이 적혀있다.

역시 postgres의 environment도 Hub에서 확인할 수 있다.

docker-compose up을 이용하여 만들면 기본 Network, 설정한 Volume, Container 생성을 하는 것을 확인할 수 있다.

그리고 생성된 Volume과 Container를 ls 명령어로 확인한다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal2.PNG)

생성이 모두 끝났으면 localhost:8080으로 접속한다. 접속하면 Drupal 설정이 나오는 것을 확인할 수 있다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal3.PNG)

계속하면 DB 설정이 나온다. 여기서 사용한 Postgres로 지정한다.

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal4.PNG)

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal5.PNG)

설정할 때 유의할 점은 DB의 Password를 YAML파일에서 environment로 지정했기 때문에 똑같이 써주어야 연결할 수 있다.

그리고 Advanced Options에서 Host가 localhost로 되어 있는데, localhost가 맞다고 착각할 수 있다.

Container는 YAML파일을 통해 생성되어 drupal과 postgres라는 이름으로 네트워크 내에서 동작하고 있다. 

즉, Docker DNS에서 설명한 것과 같이 Host는 postgres라는 이름으로 해야 찾을 수 있다.

마지막으로, 사이트를 지정하면 모두 완성된다.

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal6.PNG)

![image7](https://github.com/kjo26619/Docker/blob/main/Chapter5/Image/drupal7.PNG)

Drupal과 Postgresql 외에도 다양한 서버를 지정하고 만들어낼 수 있다.
