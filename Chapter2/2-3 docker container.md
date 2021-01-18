# Container vs. VM

Chapter 1에서 언급한 바와 같이 Container와 VM은 Hypervisor와 격리 수준의 차이이다.

그래서 Docker로 Container를 구성하고 Host OS에서 Process들을 확인하면 Container의 Image Process가 실행 중인 것을 확인할 수 있다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container1.PNG)

MongoDB Container를 생성한다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container2.PNG)

현재 Host OS에서 ps aux를 사용하여 Process를 확인해보면 mongo daemon이 실행 중인 것을 확인할 수 있다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container3.PNG)

Container를 멈추고 다시 확인해보면 Process가 사라진 것을 확인할 수 있다.

# Docker Container Inspect

Container의 자세한 정보를 확인하고 Monitoring 할 수 있는 명령어들이 있다.

1. docker container logs

    ```
    # docker container logs (CONTAINER ID or NAME)
    ```

    2-2에서 언급한 바와 같이 logs 명령어는 Container의 Log를 확인할 수 있다. 

    특히, MySQL DB Container를 생성할 때 비밀번호를 무작위로 생성할 수 있도록 할 수 있다.

    이는 run 명령어에서 -e 옵션을 사용해서 Container의 환경 변수를 지정해주는 방법이다.

    ![image4](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container4.PNG)

    ![image5](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container5.PNG)

    환경 변수 중 MYSQL_RANDOM_ROOT_PASSWORD=true를 통해서 비밀번호를 무작위로 생성한 다음 logs 명령어를 통해서 생성된 비밀번호를 확인할 수 있다.

    이런 식으로 필요한 Log를 체크할 때 사용한다.
  
2. docker container top

    ```
    # docker container top (CONTAINER ID or NAME)
    ```

    2-2에서 언급한 바와 같이 top 명령어는 Container의 현재 Process를 확인할 수 있다.
  
3. docker container inspect
  
    ```
    # docker container inspect (CONTAINER ID or NAME)
    ```

    inspect 명령어는 Container의 metadata를 보여준다. 
  
    ![image6](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container6.PNG)
  
    metadata에는 Config 부터 Path, Volumes, Networking 등이 있다.

    명령어의 결과는 JSON 파일로 반환되며 자세한 설정을 확인할 수 있다.
  
4. docker container stats

    ```
    # docker container stats
    ```
  
    stats는 Host OS에서 현재 Container가 사용하고 있는 CPU, Memory 등의 자원을 확인할 수 있다.

    어느 한 Container로 지정하려면 이름이나 ID를 뒤에 쓰면 되고 없이 쓰면 모든 Container를 확인할 수 있다.
  
    ![image7](https://github.com/kjo26619/Docker/blob/main/Chapter2/Image/container7.PNG)
    
# Docker Container Shell

Docker Container에 Shell로 진입하여 대화형 모드로 작업을 해야할 때가 생길 수 있다.

이를 위해 만든 옵션이 -it 옵션이다.

여기서 -i 옵션은 --interative 옵션으로, 컨테이너와 연결하지 않았음에도 STDIN이 열려 있는 것을 유지하라는 것이라는 의미인데 그냥 대화형 모드라고 보면 된다.

그리고 -t 옵션은 --tty 옵션으로, TTY 모드를 사용한다는 것이다.

이 두 옵션을 통해서 상호작용하는 대화형 모드를 사용할 수 있다.

```
# docker container run -it (IMAGE NAME) (SHELL)
```

run을 이용하여 생성하면서 바로 대화형 모드로 만들 수 있다.

기존에 만들어진 Container를 실행하면서 대화형 모드로 진입해야 할 경우에는 start 명령어에서 -ai 옵션을 사용한다.

```
# docker container start -ai (CONTAINER ID or NAME) 
```

-a 옵션은 --attach 옵션으로 Container와 연결하는 것이고 -i 옵션은 STDIN과 연결하는 것이다.

그리고 현재 실행 중인 Container에 대화형 모드로 진입해야 할 경우에는 exec 명령어에서 -it 옵션을 사용한다.

exec 명령어는 Container에서 추가 명령어를 실행할 때 사용하는 명령어이다.

```
# docker container exec -it (CONTAINER ID or NAME) (SHELL)
```

