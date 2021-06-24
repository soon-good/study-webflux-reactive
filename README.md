# spring webflux, reactive 스터디

아직은 목차를 어떻게 구분할지도 결정하지는 못했다. 웹플럭스에 대해 거의 처음 접해보는 중이다. 참고한 책은 [스프링 부트 실전 활용 마스터](http://www.yes24.com/Product/Goods/101803558) 이다.<br>

사실 스터디를 자율적으로 하면서 회사생활을 하는게 쉽지 않은것 같다. 개발자는 새로운 기술을 배우는 것이 회사의 업무시간에 맞춰서 보고까지 해야하니 쉽지 않은게 현실이긴 하다. 내 나름대로 찾은 방식은 이렇게 아침에 1시간 일찍와서 1시간 정도 책을 읽고, 출퇴근 시간마다 책을 틈틈히 읽고, 집에와서 깨작깨작 조금씩 정리한다. <br>

사실 예전엔 혼자 스터디 하는 것도 눈치보면서 했던 것 같다. 고참 개발자들의 왠지 가소롭다는 눈빛(니가? 이런 눈빛)을 견디기가 쉽지 않았던것 같다. 무슨 말만 해도 윽박지르기만하고 그런 고참들 사이에서 4년 가까이 일하면서 이상하게 눈치를 보고, 주눅드는게 습관되었나보다. 지금은 그 회사들을 나온지 3년 가까이 되었으니 나름 회복된게 아닐까 싶기도 하다. 3년 전쯤 중소기업에 있을때 새로운 프로젝트를 직접 스프링 부트로 시작해서 완료했다. 이 때에도 부정적인 시각이 많았던것 같다. 평상시 조금이라도 고통스럽게 노력하지 않는 사람들이 자신이 모르는 것을 접할때 어떻게 나쁘게 행동하는지 보였던것 같다.<br>

나중에 언젠가 스프링 웹 플럭스를 적용해서 새로운 프로젝트를 해볼 때가 있을 것 같아 그 때를 위해 미리 공부를 해놓아야 할 것 같다.<br>

일단은 메모만 해둘 예정이다.<br>

<br>

## 2021/06/24

### 1장 스프링 부트 웹 애플리케이션 만들기

집에가서 더 정리해야 겠지만, 기억나는 것만 빠르게 떠올려서 메모해보면 이렇다.<br>

스프링 웹 플럭스 기반의 컨테이너는 임베디드 톰캣이 아니라 네티 컨테이너를 사용한다.<br>

요청 헤더는 `text/event-stream` 이다. 특이하다.<br>

```java
class PoliteServer{
  private final KitchenService kitchen;
  
  PoliteServer(KitchenService kitchen){
    this.kitchen = kitchen;
  }
  
  Flux<Dish> doingMyJob(){
    return this.kitchen.getDishes()
      .doOnNext(dish -> System.out.println("주문하신 " + dish.getName() + " 가 나왔습니다."))
      .doOnError(error -> System.out.println("[에러] " + error.getMessage()))
      .doOnComplete(()-> System.out.println("감사합니다."))
      .map(Dish::deliver);
  }
  
  public static void main(String [] args){
    PoliteServer server = new PoliteServer(new KitchenService());
    
    server.doingMyJob().subscribe(
      dish -> System.out.println("Consuming " + dish);
      throwable -> System.err.println(throwable);
    );
  }
}
```

subscribe() 를 호출하지 않으면 doingMyJob() 을 호출하더라도 아무 일도 일어나지 않는다. 리액터 애플리케이션에서는 구독하기 전 까지는 아무일도 일어나지 않는다. main() 메서드 안에서 subscribe() 가 호출되어야 그 때부터 뭔가가 동작하기 시작한다.<br>

**Future 인터페이스와 Flux의 차이점**<br>

이 부분에서 조금 오? 하는 생각이 들었다. 일단은 집에서 더 정리할 생각으로 받아 적어놓는다.

- 하나 이상의 Dish(요리) 포함 가능
- 각 Dish(요리)가 제공될 때 어떤 일이 발생하는지 지정 가능
- 성공과 실패의 두 가지 경로 모두에 대한 처리 방향 정의 가능
- 결과 폴링 (polling) 불필요
- 함수형 프로그래밍 지원

<br>

나머지는 집에서... 의외로 웹 플럭스 말고도 spring, maven plugin 등에 대해 유용한 것들을 차분하게 설명해주고 있어서 정리할 만한 가치가 있는 책이라는 생각이 많이 들었다.

<br>