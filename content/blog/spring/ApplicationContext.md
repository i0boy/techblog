---
title: 'ApplicationContext'
date: 2020-08-27 04:16:00
template: "post"
category: 'spring'
draft: false
---
![IOC 컨테이너](/images/ApplicationContext/ApplicationContext_020925.png)

실습 환경 만들기 : 스프링 부트 프로젝트
두개의 클래스 만들기 : 북서비스 / 북레포지토리
![북레포](/images/ApplicationContext/ApplicationContext_021021.png)

스프링 부트로 프로젝트로 만드는 이유는 
spring-boot-web-starter만 넣어두면
스프링 예제 학습에 필요한 `의존성`들이 알아서 들어옴
![의존성](/images/ApplicationContext/ApplicationContext_021119.png)
어느 모듈에 어떤 클래스가 있느냐 까지 다 알 필요는 없음.... ㅋㅋㅋ
pom.xml 장황한 설정 필요 x
![pom.xml](/images/ApplicationContext/ApplicationContext_021226.png)


## 1. 빈 설정파일 이용하기.
1. xml 이용
![xml설정](/images/ApplicationContext/ApplicationContext_021512.png)
name은 setter 변수명 (빈을 주입받아 사용하는 빈의 property)
ref는 다른 빈 id
고전적인 방법이라 요즘 잘 안씀.
![xml로 AppCOntext](/images/ApplicationContext/ApplicationContext_021905.png)
설정파일을 사용하는 ApplicationContext 만들기
![](/images/ApplicationContext/ApplicationContext_022616.png)
타입캐스팅 해줘야 오류 안생김
![](/images/ApplicationContext/ApplicationContext_022644.png)
해당 내용 실행해보기
`의존성 주입이 되어있다`
해당 방법의 단점 - 너무 번거롭다 
그래서 등장한게 컴포넌트 스캔.
2. xml + component scan
![](/images/ApplicationContext/ApplicationContext_022752.png)
해당 패키지 이하의 모든 빈을 다 스캔해줌
빈 스캐닝을 할 때에는 4가지 애노태이션 / 컴포넌트라는 에노테이션 사용해서 등록 가능
4가지는 컴포넌트를 확장한 것.
해당 방법은 빈으로만 등록. `@Autowired`로 의존성을 주입해준다.

![빈 등록](/images/ApplicationContext/ApplicationContext_022949.png)
빈 등록

![의존성 주입 및 빈 등록](/images/ApplicationContext/ApplicationContext_022913.png)
의존성 주입 및 빈 등록

참고 : @Inject라는 방법도 있음

해당 패키지 이하에서 애노태이션 스캐닝을 통해 빈으로 등록

3. configuration 자바 파일 이용 (no xml)
원래 파일에서 어노테이션 다 삭제
파일 새로 만들기
1. setter 이용
![세터](/images/ApplicationContext/ApplicationContext_023308.png)
2. 메서드 파라미터 이용
![파라미터 이용](/images/ApplicationContext/ApplicationContext_023350.png)

실행 파일 바꿔주기 
![](/images/ApplicationContext/ApplicationContext_023434.png)

참고 : 롬복과 final을 이용한 생성자 주입이 bp로 알려져 있음.

![](/images/ApplicationContext/ApplicationContext_023554.png)

실행하면 위와 결과가 동일하다.

참고 : 패키지 구조
![](/images/ApplicationContext/ApplicationContext_023625.png)

빈으로만 등록이 된다면 알아서 autowired에 주입됨.
![](/images/ApplicationContext/ApplicationContext_023722.png)
![](/images/ApplicationContext/ApplicationContext_023736.png)

4. 끝판왕

해당 클래스 위치한 곳부터 컴포넌트 스캔을 해라
![문자열 활용](/images/ApplicationContext/ApplicationContext_023852.png)
이 방법은 문자열
![](/images/ApplicationContext/ApplicationContext_023926.png)
해당 방법은 클래스 (타입 세이프!)

각각 파일에 @Service / @Repository 통해 빈으로 등록
(가장 타입 세이프 한 방법 -> BP)

5. 최종 정리 (BP)
![](/images/ApplicationContext/ApplicationContext_024100.png)
우리는 직접 ApplicationContext를 만들어 사용하지는 않는다.
![](/images/ApplicationContext/ApplicationContext_024145.png)
SpringBootApplication 자체가 설정 파일! (설정파일 따로 불필요...)

학습 내용 정리 :
스프링 IoC 컨테이너의 역할
    ● 빈 인스턴스 생성
    ● 의존 관계 설정
    ● 빈 제공
AppcliationContext
    ● ClassPathXmlApplicationContext (XML)
    ● AnnotationConfigApplicationContext (Java)
