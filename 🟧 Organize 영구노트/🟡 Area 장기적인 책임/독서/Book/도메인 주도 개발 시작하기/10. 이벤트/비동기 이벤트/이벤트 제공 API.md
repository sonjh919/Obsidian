#Architecture #DDD #Book

## 이벤트 제공 API
- API 방식과 [[이벤트 저장소와 이벤트 포워더|포워더]] 방식의 차이는 이벤트를 전달하는 방식에 그 차이가 있다.
- 포워더 방식에서는 이벤트를 어디까지 처리했는지를 추적하는 역할이 포워드라면, API는 이벤트 목록을 요구하는 외부 핸들러가 자신이 어디까지 이벤트를 처리했는지 기억해야한다.

> [!tip]+ 
> IAM에서는 위와 비슷한 방식으로 동기화를 제공하고 있다.


![[event7.png]]



