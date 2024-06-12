# AOP

> Advice Oriented Programming, 관점 지향 프로그래밍
> 
- 핵심 기능과 부가 기능으로 나누어 각각 모듈화
    - 핵심 기능: 해당 객체가 제공하는 고유의 기능, 핵심 비즈니스 로직
    - 부가 기능: 핵심 기능을 보조하기 위해 제공되는 기능 (예: 로깅, 파일 입출력 등)
- 부가 기능을 핵심 기능으로부터 분리하여 재사용하겠다는 취지
<br><br>

## 주요 용어

- Advice: 부가 기능 자체
- JoinPoint: advice가 적용될 수 있는 모든 위치
- Pointcut: advice를 실제 적용할 위치를 필터링 하는 기능
- Aspect: advice+pointcut
- Target: aspect를 적용한 곳, 부가 기능을 추가할 객체
