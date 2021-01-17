# Docker Command 1

1. docker version

    Docker Engine의 버전을 확인하는 명령어

2. docker info

    Docker Engine의 정확한 구성을 확인하는 명령어
    
3. docker command line

    이 전에는 'docker <command> (options)'를 통해서 command를 사용하였지만 점점 많아지는 command 때문에 계층적 구조가 필요하게 되었다.
    
    그래서 주요 기능(container, swarm, network 등)을 Management commands로 추가하고 command line을 바뀐 것이 'docker <management-command> <sub-command> (options)'이다.
