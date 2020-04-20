---
title: JPA ORM JDBC 란
date: 2020-04-20 16:48:48
category: spring-boot & Java
---

Spring-boot를 처음 사용하는데 처음 듣는 단어들이 너무 많았다. 왜 이렇게 어렵고 공부할께 많은건지..

DB 연동을 하면서 JPA, ORM, JDBC에 대하여 알게 되었고 간단히 공부를 해보았다. 어떤 차이가 있는지 정도?

# JPA (Java Persistent API)

JPA는 자바 ORM 기술에 대한 표준 명세를 정의한 것이다. 표준 명세라는 단어는 스펙이라고 표현할 수도 있겠다. JPA는 표준 명세일 뿐이라서 JPA 만으로 실제 무언가를 할 수는 없다. JPA 표준 명세를 실제로 구현한 구현체가 바로 Hibernate이고, JPA 구현체 또는 JPA provider라고 부른다.

# ORM (Object Relational Mapping)

이름 그대로 객체와 관계형 DB를 맵핑해주는 것이다. 개발자가 데이터 접근보다 로직 자체에 집중할 수 있게 해준다. ORM이 없을 때는 DB 작업을 위해서 SQL문을 직접 만들었다. 하지만 ORM을 사용하면 SQL문에서 자유로워지고 유지보수가 편해진다. 

ORM 기술에 대한 표준 명세가 바로 JPA이고, JPA 표준을 구현한 프레임워크가 바로 Hibernate이다. 

# JDBC (Java Database Connectivity)

자바 프로그램 내에서 SQL 문을 실행하기 위한 자바 API

데이터베이스마다 SQL문이 조금씩 다르기 때문에 여러 DB를 사용할 경우 혼란이 생길 수 있습니다. 이를 막기 위해서 JDBC가 탄생하였고, 어느 DB를 사용하든 해당 DB의 Driver만 설치한 후 JDBC에 알려주면 문제없이 문제없이 사용이 가능합니다.

JDBC가 하는 역할은 다음과 같습니다.

* JDBC 드라이버 로딩
* DBMS 연결
* SQL문을 DB에 전송하고 결과 값 return

JDBC Driver는 Java 프로그램의 요청을 DBMS가 이해할 수 있는 프로토콜로 변환해주는 클라이언트 사이드 어댑터이다. 커넥션 연결, SQL 쿼리 전송등의 작업을 수행하는데 핵심 키워드는 아래와 같다.

* Connection: 데이터베이스 연결(세션)
* Statement: SQL문을 실행하거나 SQL문의 결과를 반환하는데 사용





### Reference

* https://hzoou.tistory.com/64
* https://brunch.co.kr/@springboot/105