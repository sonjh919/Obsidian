#Monitoring #Prometheus

## Prometheus
프로메테우스는 대상 시스템으로부터 **각종 모니터링 지표를 수집하여 저장하고 검색**할 수 있는 시스템으로, 오픈 소스이다.
![[prometheus.png]]

### 프로메테우스의 특징
- 시계열 DB를 내장하고 있고, 자체적인 질의 페이지 외에 시각화 기능은 없으나 [[Grafana]]를 통한 시각화를 지원한다.
- 완전한 오픈 소스 모델을 선택해 사용자 층이 넓고 자료, 플러그인이 풍부하며 각종 대시보드 도구나 메신저 등이 프로메테우스와의 연계를 지원하므로 직접 모니터링 시스템을 구축할 때 좋음
+ 쿠버네티스의 메인 모니터링 시스템으로 많이 사용된다.
+ 프로메테우스가 주기적으로 exporter(모니터링 대상 시스템)로부터 pulling 방식으로 [[Metric]]을 읽어서 수집한다.

> [!question]+ Pulling 방식?
> 중앙 서버에서 [[Agent]]의 데이터를 수집하는 방식이다. 쿠버네티스 환경에서 설치된 Agent를 통해 노드와 컨테이너 상태를 모두 수집해 모니터링이 가능하다.

- Agent를 통해 내부 Metric을 외부로 노출하기 때문에 사용자가 수집 대상에 접속할 수만 있다면 개인 컴퓨터에서도 Metric을 가져올 수 있음, 따라서 모니터링과 관련된 개발을 하기 용이함
- 일회성으로 모니터링 대상에 대한 세부적인 Metric도 간단하게 웹 브라우저로 확인 가능

### 프로메테우스 Agent
프로메테우스의 경우, **Exporter**라고도 불리는 에이전트가 사용된다. 
+ 모니터링하려는 시스템에서 실행되며, 해당 시스템의 상태 및 성능에 대한 정보를 수집하여 프로메테우스 서버로 보낸다. 
+ Exporter는 다양한 종류의 시스템에 대해 사용할 수 있으며, 각각의 시스템에 맞게 특정한 메트릭을 수집한다.

> [!summary]+ 
> 특정 환경에서 동작하여 **데이터 수집, 처리, 전송 등의 역할**을 수행하는 소프트웨어이다. 프로메테우스의 경우, Exporter가 에이전트 역할을 한다.