# Docker Swarm Secret

Docker Swarm의 Secret 기능은 유저 네임, 패스워드, TLS Certificates 와 Key, SSH Key, 중요 데이터, 문자열(최대 500kB) 등을 보낼 때 암호화되지 않은 상태로 보낼 경우 보안에 문제가 생기기 때문에 생긴 기능이다.

Secret 기능은 Swarm 서비스에만 사용이 가능하다. 이 기능을 통하여 데이터를 중앙에서 관리하고 필요한 Container에게 암호화하여 전달하는 등의 일을 할 수 있다.

Swarm에서 Secret이 추가되면 TLS 연결을 통해 Swarm Manager에게 Secret을 보낸다. 그리고 Secret은 암호화된 Raft 로그에 저장이 된다. 

그리고 전체 Raft 로그는 나머지 Manager에게 복제되어 보내지게 된다.

만약, Swarm 서비스에 Secret을 추가하여 보내면 암호화된 파일이 Container 파일 시스템에 마운트되고 해독한다. 즉, 저장은 평문으로 된다.

Container 내의 저장되는 기본 마운트 지점은 /run/secrets/<secret_name> 에 저장된다.

언제든지 서비스를 업데이트하여 Secret을 추가하거나 추가된 Secret을 없앨 수 있다. 물론, 서비스를 종료시킨 다음 Secret을 제거해야 한다.

그리고 이러한 Secret에 대해서 별칭(Alias)를 지정할 수 있다. 즉, 같은 Secret이 다른 별칭으로 Container 내에 존재할 수 있다. 마운트 지점은 /run/secrets/<secret_alias> 이다.

