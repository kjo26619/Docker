# Container Lifetime

Container는 보통 Immutable(불변)하고 Ephemeral(일시)적이라고 한다.

Immutable의 뜻은 Container를 Deployment 할 때 더이상 변하지 않는다는 의미이다. 

Ephemeral의 뜻은 일회용을 의미한다. Container는 Image에 의해서 재 생성되기는 하지만 재사용하는 일은 없다.

그래서 Container를 업데이트하고 재배포하기 위해서는 Image를 수정하고 새로 생성을 해야한다.

이러한 Container의 특성은 신뢰성과 일관성을 유지할 수 있고 변경된 사항들을 확인할 수 있다.

하지만 이에 따른 문제점이 발생한다.

어플리케이션이 생성할 고유한 데이터나 데이터베이스 또는 Key-Value 저장 등의 데이터는 Container가 삭제되면 다 같이 사라지게 된다.

이렇게 되면 DB Container는 업데이트를 할 수 없는 상태가 되어 버린다.

이러한 고유 데이터를 'Persistent Data'라고 하며 이를 유지하는 방법이 Container에서 매우 중요한 사항이 되었다.

Container에서는 기본적으로 고유 데이터가 포함이 되면 안된다. 이를 'Separation of Concerns'라고 한다.

# Persistent Data

Docker에서는 2가지 방법을 가지고 고유 데이터를 유지할 수 있도록 한다.

1. Volumes : 외부 파일 시스템에 고유 데이터를 저장하는 특수한 위치를 생성하는 Container 구성 옵션.

2. Bind Mounts : Host의 디렉토리 또는 파일을 Container에 공유하거나 마운트하는 방법.
