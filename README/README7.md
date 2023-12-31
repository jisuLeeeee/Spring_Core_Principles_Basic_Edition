### <2024-01-09>

<aside>
🚨 다음의 모든 내용들은 김영한님의 <b>“스프링 핵심 원리 - 기본편”</b> 강의를 토대로 작성한 것입니다.

</aside>

## Section3 - 스프링 핵심 원리 이해 2 - 객체 지향 원리 적용

### 📍전체 흐름 정리

**새로운 할인 정책 개발**

> 다형성 덕분에 정액 할인 정책에서 **새로운 정률 할인 정책 코드를  추가로 개발하는 것 자체는 아무 문제가 없음**
>

**But,** 실제 애플리케이션에 적용하려고 하니 문제점 발생

> **클라이언트 코드인 주문 서비스 구현체도 함께 변경해야함(⇒ OCP 위반)**
주문 서비스 클라이언트가 인터페이스인 DiscountPolicy 뿐만 아니라, **구체 클래스인 FixDiscountPolicy도 함께 의존함(⇒ DIP 위반)**
>

---

**관심사의 분리**

- 기존에는 클라이언트가 의존하는 서버 구현 객체를 직접 생성하고, 실행함
- 비유를 하면 기존에는 남자 주인공 배우가 공연도 하고, 동시에 여자 주인공도 직접 초빙하는 다양한 책임을 가지고 있음
- 공연을 구성하고, 담당 배우를 섭외하고, 지정하는 책임을 담당하는 별도의 <b>“공연 기획자(=AppConfig)”</b>가 나올 시점
- AppConfig는 애플리케이션의 전체 동작 방식을 구성하기 위해, **구현 객체를 생성**하고 **연결**하는 책임을 가짐
- 이제부터 클라이언트 객체는 자신의 역할을 실행하는 것만 집중하고 권한이 줄어듬

---

**AppConfig 리펙터링**

- 구성 정보에서 **역할과 구현을 명확하게 분리**
- 역할이 잘 들어나고 중복 제거가 됨

---

**새로운 구조와 정률 할인 정책 적용**

- Appconfig의 등장으로 애플리케이션이 크게 <b>“사용 영역”</b>과, 객체를 생성하고 <b>“구성(Configuration)하는 영역”</b>으로 분리
- 할인 정책을 변경해도 **AppConfig가 있는 구성 영역만 변경하면 됨**, 사용 영역은 변경할 필요가 없지만 물론 클라이언트 코드인 주문 서비스 코드도 변경하지 않음

---

### 📍좋은 객체 지향 설계의 5가지 원칙의 적용

1. SRP(Single responsibility principle) 단일 책임 원칙

   **한 클래스는 하나의 책임만 가져야 한다.**

    - 클라이언트 객체는 직접 구현 객체를 생성하고, 연결하고, 실행하는 다양한 책임을 가지고 있었음
    - SRP 단일 책임 원칙을 따르면서 관심사를 분리함
    - 구현 객체를 생성하고 연결하는 책임은 AppConfig가 담당하고 클라이언트 객체는 실행하는 책임만 담당
2. DIP(Dependency inversion principle) 의존 관계 역전 원칙

   **프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다. 의존성 주입은 이 원칙을 따르는 방법 중 하나다.**

    - 새로운 할인 정책을 개발하고 적용하려 하니 클라이언트 코드도 함께 변경해야했다. 왜냐하면, 기존 클라이언트 코드(OrderServiceImpl)는 DIP를 지키며 DiscountPolicy 추상화 인터페이스에 의존하는 것 같았지만, FixDiscountPolicy 구체화 구현 클래스에도 함께 의존했다.
    - 클라이언트 코드가 DiscountPolicy 추상화 인터페이스에만 의존하도록 코드를 변경했다.
    - 하지만 클라이언트 코드는 인터페이스만으로 아무것도 실행할 수 없기 때문에 **AppConfig가 FixDiscountPolicy 객체 인스턴스를 클라이언트 코드 대신 생성해서 클라이언트 코드에 의존관계를 주입**했다. 이렇게해서 DIP 원칙을 따르면서 문제도 해결 !!!
3. OCP(Open/closed principle)

**소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.**
- 다형성 사용하고 클라이언트가 DIP를 지킴
- 애플리케이션을 사용 영역과 구성 영역으로 나눔
- AppConfig가 의존관계를 FixDiscountPolicy → RateDiscountPolicy로 변경해서 클라이언트 코드에 주입하므로 클라이언트 코드는 변경하지 않아도 됨
- **소프트웨어 요소를 새롭게 확장해도 사용 영역의 변경은 닫혀 있다!**