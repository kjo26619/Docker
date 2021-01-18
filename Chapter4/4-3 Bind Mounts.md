# Bind Mounts

Bind Mounts는 Host의 파일이나 디렉토리를 Container의 파일이나 디렉토리에게 연결해주는 방법이다.

Volume과는 다르게 Bind Mounts는 Dockerfile에서 지정할 수 없다.

Bind Mounts는 run 명령어를 할 때 -v 옵션을 통해서 지정할 수 있다.

```
# docker container run -v (Host File or Directory:Container File or Directory) (IMAGE NAME)

Container가 만들어 질 때 Bind Mounts 되면서 서로 연결되게 된다.

즉, Host에서 수정을 하면 Container 역시 수정된다.

![image1](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/bind1.PNG)

Bind Mounts를 확인하기 위해서 Host에서 Bind Mounts 용 폴더를 생성한다.

그리고 -v 옵션을 이용해서 Bind할 위치를 지정한다.

![image2](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/bind2.PNG)

위 결과를 보면 Container 내부에 원래는 없어야할 /bind_test가 생긴 것을 확인할 수 있다. 

이는 -v 옵션에서 Container 파일 혹은 디렉토리 위치가 없어서 생긴 것이며, 기존에 있는 파일이나 디렉토리를 할 경우 그 파일이나 디렉토리와 연결된다.

![image3](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/bind3.PNG)

Container 내부에서 확인해보면 Host에 있던 파일들이 모두 존재하는 것을 확인할 수 있다.

그리고 Container 내부에서 새로운 파일을 만들고 나서 Host에서 확인해보면 달라지는 것을 확인할 수 있다.

![image4](https://github.com/kjo26619/Docker/blob/main/Chapter4/Image/bind4.PNG)

이를 통해서 Container와 Host 디렉토리 간에 Bind Mounts 된 것을 확인할 수 있다.

이렇게 고유 데이터를 유지하고 Container를 변경하고 재배포할 수 있도록 하였다.
