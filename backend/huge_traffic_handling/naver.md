# 네이버의 메인 페이지의 트래픽 처리방식

웹 서핑을 한번이라도 해본사람들은 네이버 메인페이지에 접속해봤을 것이다. 네이버는 우리나라 사람들이 거의 모두 사용하는 만큼 평소에도 트래픽이 많지만, 대용량 트래픽도 자주 발생하는 편이다.
예를들면 지진이 일어났거나, 한국 유명 축구선수가 골을 넣었을 때를 예측해볼 수 있을 것이다.


네이버 메인 페이지 개발팀은 분산 처리 기술과 모니터링 체계를 통해서 네이버 메인 페이지를 끊김없이 안정적으로 서비스하고, 급격한 트래픽 변화에 대응하고 있다. 이제 네이버 메인 페이지의 트래픽
처리에 사용하는 분산처리 기술과 모니터링 기술에 대해서 알아보자.


## 네이버 메인 페이지의 분산 처리
로드 밸런서를 사용하는 일반적인 3계층(3-Tier) 분산처리 모델은 구성 요소에 문제가 생겼을 때 해결하기에 어려운 문제점이 있다고 한다. 그래서 네이버 메인 페이지는 서비스에 가장 적합한 분산처리 모델을 구축하여 사용하고 있다.

### 일반적인 분산처리 모델

다음 그림은 로드 밸런서로 구성한 일반적인 3계층(3-Tier) 분산처리 모델을 표현한 도식이다.

> 로드 밸런서: 부하를 분산해주는 것으로, 컴퓨터 자원들에게 작업을 나누어 수행하도록 해주는 것

일반적인 분산처리 모델의 동작원리는 다음과 같다.

- 클라이언트의 트래픽이 로드 밸런서를 통해서 각 웹 서버로 분산된다. 이 때, WAS(Web Application Server)는 동일한 데이터베이스를 참조한다.

이런 분산처리 환경에서 각 구성요소에 문제가 생긴다면 어떻게 처리해야 할까?

로드 밸런서에 문제가 생긴다면 DNS 라운드 로빈 방식등을 사용해서 문제를 처리할 수 있다. 하지만 WAS에 문제가 생긴다면 어떻게 처리해야 할까? WAS에 문제가 생긴다면 다음과 같은 점을 고려해야 한다.

> DNS 라운드 로빈: 별도의 하드웨어 혹은 로드밸런싱 장비를 사용하지 않고, DNS만을 이용하여 도메인 레코드 정보를 조회하는 시점에서 트래픽을 분산하는 기법

- WAS에 문제가 생기면 웹 서버가 다른 WAS를 찾게 하도록 해야한다.
- 사용자가 로그인한 상태라면 WAS에 세션 클러스터링을 설정해야 한다.
- 세션 클러스터링 설정을 위한 추가작업이 필요하고 관리 지점이 증가한다.

> 세션 클러스터링: 대체된 WAS에게도 세션을 공유할 수 있게 하는 기술

데이터베이스에서는 다음과 같은 점을 고혀해야 한다.

- 데이터 동기화 등의 문제 때문에 데이터 스토리지 레이어는 다중화가 특히 어려운 부분에 속한다.
- 사용하는 데이터베이스의 종류에 따라서 어떻게 다중화를 할것인지, 데이터를 어떻게 분산할 것인지 깊이 고민해야한다.



