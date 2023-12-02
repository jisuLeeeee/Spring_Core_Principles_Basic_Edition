### <2023-12-02>

<aside>
🚨 다음의 모든 내용들은 김영한님의 <b>“스프링 핵심 원리 - 기본편”</b> 강의를 토대로 작성한 것입니다.

</aside>

## Section3 - 스프링 핵심 원리 이해 2 - 객체 지향 원리 적용

### 📍관심사의 분리

애플리케이션을 하나의 공연이라 생각했을 때 현재 코드는 남자 주인공(인터페이스)이 여자 주인공을 하는 배우까지 직접 관여하고 있는 <b>“다양한 책임”</b>을 가지고 있음

**⇒ 관심사를 분리하자**

- 배우는 본인의 배역을 수행하는 것에만 집중해야 함
- 디카프리오는 어떤 여자 주인공이든 똑같이 공연할 수 있어야 함
- 공연의 구성과 배우를 담당하는 “공연 기획자”가 있어서 확실히 분리되어야 함

**AppConfig 등장**

애플리케이션의 전체 동작 방식을 구성(config)하기 위해, **구현 객체를 생성하고, 연결하는 책임을 가지는 별도의 설정 클래스를 만들어보자**

```java
package hello.core;

import hello.core.discount.FixDiscountPolicy;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

public class AppConfig {

    public MemberService memberService(){
        // 생성자 주입
        return new MemberServiceImpl(new MemoryMemberRepository());
    }

    public OrderService orderService(){
        return new OrderServiceImpl(new MemoryMemberRepository(), new FixDiscountPolicy());
    }
}
```

- AppConfig는 애플리케이션의 실제 동작에 필요한 **구현 객체를 생성**
    - MemberServiceImpl
    - MemoryMemberRepository
    - OrderServiceImpl
    - FixDiscountPolicy
- AppConfig는 생성한 객체 인스턴스의 참조(레퍼런스)를 **생성자를 통해서 주입(연결)** 해줌
    - MemberServiceImpl ⇒ MemoryMemberRepository
    - OrderServiceImpl ⇒ MemoryMemberRepository, FixDiscountPolicy

<aside>
💡 지금은 각 클래스에 생성자가 없어서 컴파일 오류가 발생하지만, 바로 다음에 코드에서 생성자를 만듬

</aside>