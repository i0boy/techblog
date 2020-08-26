---
title: '스프링 프레임워크 핵심 기술 0'
date: 2020-08-27 04:16:00
template: "post"
category: 'spring'
draft: false
---
![](/images/core0/core0_011725.png)
백기선님의 인프런 강의 [스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core) 강의를 읽고 정리한 내용입니다.
복습을 위한 핵심 내용만 요약하였으므로, 더 세부적인 설명은 직접 강의를 수강하시기 바랍니다.

## 들어가며

스프링 프레임워크(이하 스프링) 5.1 버전이 출시 되었습니다.
버전이 올라갈 수록 스프링은 다양한
프로그래밍 기법과 기능을 제공하지만 스프링의 핵심 기술은 크게 변하지 않았습니다.

```TEXT
따라서 스프링 핵심 기술을 이해한다면
JDBC, 테스트, MVC 관련 기능,
스프링부트, 스프링 데이터 JPA와 같은 여러 다른 스프링 프로젝트도 빠르고 정확히 이해할 수 있습니다.
```

## 다룰 기술들

- 스프링 IoC 컨테이너와 빈 (컨테이너의 기능 활용, 빈을 활용한 의존 관계 주입)
- 스프링 AOP (Aspect 모듈화)
- 스프링 추상 API
- 스프링 PSA
  - Resource
  - Validation
  - 데이터 바인딩
- SpEL
- Null 관련 유틸리티


### 용어 정리
Portable Service Abstraction : 
환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하려는 추상화 구조
POJO 원칙을 철저히 따른 Spring 의 기능
사용 라이브러리와 관계 없이 동일한 인터페이스로 동일한 구동. 의존성 고려 불필요 
Spring 은 이렇듯 특정 기술에 직접적 영향을 받지 않게끔 객체를 POJO 기반으로 
한번씩 더 추상화한 Layer =를 갖고 있으며 이를통해 일관성있는 서비스 추상화를 만들어낸다.

출처: https://jins-dev.tistory.com/entry/Spring-PSAPortable-Service-Abstraction의-개념 [Jins' Dev Inside]

Aspect Oriented Programming
Inversion Of Control
이 두 용어는 추후 강의에서 설명됨.