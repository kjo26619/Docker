# Docker Network

Docker의 네트워크는 기존의 네트워크인 TCP/IP 네트워크와 유사하다.

그렇기 때문에 run 명령어에서 -p 옵션을 통해서 포트를 지정할 수 있는 것이다.

Docker에서는 내부에 네트워크 드라이버를 통한 가상의 네트워크가 이루어져 있다.

기본 Docker Engine에는 3 개의 기본 네트워크가 이루어져 있으며 이는 docker network ls 명령어로 확인할 수 있다.

```
# docker network ls
```

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network1.PNG)

기본 네트워크는 none, bridge, host로 이루어져 있으며 Container가 시작되면 bridge 네트워크에 포함된다.

bridge 네트워크에 대해서 자세하게 알기 위해 docker network inspect 명령어를 사용하면 정보를 얻을 수 있다.

```
# docker network inspect (NETWORK NAME)
```

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network2.PNG)

그리고 각 Container가 어느 네트워크에 속해 있는지 확인하려면 docker container inspect 명령어를 사용하여서 확인할 수 있다.

```
# docker container inspect (CONTAINER ID or NAME)
```

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network3.PNG)

너무 긴 inspect 결과에서 원하는 정보만을 확인하기 위해서는 --format 옵션을 사용할 수 있다.

예를 들어, docker contaienr inspect --format '{{ .NetworkSettings.IPAddress }}' (CONTAINER ID or NAME) 를 통해 IP 주소만을 확인할 수 있다.

# External Network

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network4.PNG)

< Docker Network > ( 출처 : https://www.docker.com/blog/understanding-docker-networking-drivers-use-cases/ )

Docker 내부에서는 bridge를 통해서 각 Container 들이 연결되어 있다.

그리고 Host의 IP를 통해서 외부와 연결된다. 이 때 -p 로 Port가 Forwarding 된 상태에서만 외부와 통신할 수 있다.

각 Container에 연결되어 있는 Port를 확인하기 위해서는 docker container port 명령어로 확인할 수 있다.

```
# docker container port (CONTAINER ID or NAME)
```

그 외에도 ls 명령어로 확인할 수 있다.

# Network Create

Docker에서는 내부 네트워크를 추가할 수 있게끔 설계하였다.

내부 Network를 추가하는 방법은 docker network create 명령어를 통해서 추가할 수 있다.

```
# docker network create (NETWORK NAME)
```

![image5](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network5.PNG)

run 명령어를 사용할 때에는 Container는 기본 bridge 네트워크에 속하게 된다.

이 때 네트워크를 바꿔주기 위해서는 --network 옵션을 사용하면 된다.

```
# docker container run --network (NETWORK NAME) (IMAGE NAME)
```

docker network inspect 명령어로 새로 만든 네트워크에 속해 있는 Container를 확인할 수 있다.

![image6](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network6.PNG)

그리고 docker network connect 명령어를 사용해서 기존의 Container를 다른 네트워크에 속할 수 있게끔 NIC를 추가해줄 수 있다. 

```
# docker network connect (NETWORK ID or NAME) (CONTAINER ID or NAME)
```

![image7](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network7.PNG)

![image8](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/network8.PNG)

새로 만든 Container를 기본 bridge 네트워크에 추가하여 Container가 2개의 NIC를 갖고 있는 것을 확인할 수 있다.

네트워크 연결을 없애기 위해서는 disconnect 명령어를 사용하면 된다.

```
# docker network disconnect (NETWORK ID or NAME) (CONTAINER ID or NAME)
```
