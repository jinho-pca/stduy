# 람다

익명 함수를 간단하게 표현 하는 방법

우리 집에 하나 밖에 없는 강아지

## 구현해야 할 추상 메소드가 하나뿐이 인터페이스

## 람다 표현식

- 애로우 펑션을 참고해 볼 것

## Java

- 타입 추론 가능 경우 생략
- 매개변수 하나인 경우 생략
- 하나의 명령문 생략가능
- 함수의 몸체가 하나의 return 문으로만 이루어진 경우에는 생력 불가
- return 문 대신 사용 가능 단 이때, 세미 콜론 X

### 함수형 인터페이스

Java

1 2 3 4 5

```
@FunctionalINterface java.util.function    IntFunction    BinaryOperator
```

## 메소드 참조

불필요한 매게 변수를 제거하고 사용할 수 있음

메소드를 참조를 사용하면 불필요한 매개변수를 제거하고  :: 를 사용한다.

### 생성자 참조

Java

1 2

```
(a) -> {return new Object(a)} Object:: new
```

## StreapAPI

- 스트림은 외부 반복을 통해 작업하는 컬력션과 달리 내부 반복을 통해 작업 수행
- 스트림은 재사용이 가능한 컬력션과 달리 한번 사용
- 원본 데이터 변경하지 않음
- 스트림 연산은 필터-맵 기반 API 사용 지연 연산으로 성능 최적화
- 스트림은 parallelStrea() 통해 병렬 지원

Java

1 2 3 4 5

```
example.strea().filter(x -> x<2).count(); IntStream.range(1, 11).filter(i -> i%2 == 0)    .forEach(System.out.println) 
```

## 0부터 9가 들어있는 객체를 출력

Java

1 2 3 4 5 6 7

```
List<Integer> array = Arrays.asList(0, 1, 2, 3, 4, 5, 6, 7, 8, 9); for(int i=0; i<array.size(); i++){    System.out.println(array.get(i)) } IntStream.range(0, 10).forEach(value -> System.out.println(value)); IntStream(0, 10).forEach(System.out::println) 
```

------

- 익명 클래스에서 사용이 가능
- 8버전에서 사용이 가능
- () => {} -> 자바 스크립트
- 람다식의 가장 큰 장점은 병렬처리를 하는 것을 말한다.