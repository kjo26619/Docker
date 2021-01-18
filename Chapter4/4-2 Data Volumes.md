# Data Volumes

Volume은 외부 파일 시스템에 고유 데이터를 저장할 수 있는 위치를 따로 만드는 방법이다.

이러한 Volume은 3-1에서 설명한 바와 같이 Dockerfile에서 지정할 수 있다.

그래서 MySQL 같은 데이터베이스 Container는 Dockerfile에 VOLUME이 지정되어 있는 것을 확인할 수 있다.

https://github.com/mysql/mysql-docker/blob/mysql-server/8.0/Dockerfile

정확한 Image에 대한 정보를 확인하기 위해서는 먼저, docker image pull 명령어를 통해서 image를 가지고 온다.

```
# docker image pull (IMAGE NAME)
```

그리고 docker image inspect 명령어를 통해 Image에 대한 자세한 정보를 확인할 수 있다. 이 정보에는 Volume에 대한 정보도 있다.

```
# docker image inspect (IMAGE NAME)
```

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/volume1.PNG)

그리고 Container를 만든 뒤 docker container inspect를 통해서 Container의 자세한 Volume 상태를 확인할 수 있다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/volume2.PNG)

Mounts 항목에 있으며 여기서 Source는 Host에 있는 디렉토리를 Container 내부에 Destination으로 연결한다는 것이다.

그리고 docker volume ls를 통해서 현재 있는 docker volume list에 대해서 확인할 수 있다.

```
# docker volume ls
```

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/volume3.PNG)

이러한 Volume은 Container를 삭제해도 유지가 된다.

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/volume4.PNG)

그리고 run 명령어를 사용할 때 Volume을 지정할 수 있다.

이는 -v 옵션을 이용해서 지정하면 된다.

```
# docker container run -v (VOLUME NAME:DESTINATION) (IMAGE NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/volume5.PNG)

Volume의 이름을 지정하게 되면 Host의 Mount 위치도 긴 ID가 아닌 Name으로 지정이 된다.

역시 ls 명령어로 확인할 때도 Name으로 나오게 된다.

Volume 이름을 지정했을 때 Volume이 존재하지 않을 경우 새로운 Volume을 만드는 것이다.

만약 이미 존재한다면 이 기존 Volume에 연결해준다.

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/volume6.PNG)

이렇게 고유 데이터를 유지하고 Container 업데이트를 할 수 있게끔 하는 것이 Volume이다.

