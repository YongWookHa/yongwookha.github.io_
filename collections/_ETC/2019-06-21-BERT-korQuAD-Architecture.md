---
layout: post
title: Java Mybatis로 DB다루기
subtitle: + DB 기본개념
tags: [ETC, JAVA, DB, MYBATIS]
image: /img/mybatis.jpg
comments: true
---

회사 솔루션이 Java로 구현되어 있어 신규 서비스 개발에서도 자바 이용은 필수적이다. 그동안 쉽고 강력한 Python으로만 개발해왔던 나에게 자바 공부라는 첫번째 시련이 주어지고 있다. 다행히 객체지향과 상속 개념은 많이 어려운 부분이 없었지만, Java에서 사용하는 개발 툴들에 익숙해지기까지는 아무래도 시간이 필요할 것 같다.

이번에 신규 서비스 개발 업무를 수행하면서 정말 오랜만에 DB를 다룰 기회가 생겨서 열심히 공부했다. DB는 학부 프로젝트 수행할 때와 정보처리기사 공부하던 시기가 거의 마지막이었던 것 같다. 이미 흐릿해진 기억속에 DAO, DTO 개념들이 '그래도 어디서 들어본 적 있는' 단어로 남아있다. 다시 선명히 만들고 나서, 공부했던 기록들을 여기에 기록한다.

*Java에서 Mybatis를 통해 Oracle DB를 관리하는 상황에서 정리한 개념.*


## Mybatis

Mybatis는 자바에서 DB를 다룰때 도움을 주는 'Persistence framework'다. 'Persistence framework'란 데이터의 저장, 조회, 변경, 삭제를 다루는 클래스 및 설정 파일들을 말한다. 이 프레임워크를 통해 일반적인 JDBC 프로그래밍의 복잡함과 번거로움을 상당부분 줄일 수 있고 효율적인 DB관리가 가능해진다. Java에서 Mybatis를 통해 DB제어하려면 DAO, DTO를 사용해야한다. 여기서 다음의 세가지 파일이 필요하다.

* Mapper.java : interface
* Mapper.xml
* DTO.java : class

각 파일의 작성법등의 세부 정보는 [마르시아 블로그](https://marshia.tistory.com/entry/Mybatis-%EA%B0%84%EB%8B%A8%ED%95%9C-%EB%A7%88%EC%9D%B4%EB%B0%94%ED%8B%B0%EC%8A%A4-CRUD-%EC%98%88%EC%A0%9C)에 읽기 편하게 설명되어 있으니 참고하자.

먼저 DAO와 DTO의 개념에 대해 간략하게 정리해보면, 다음과 같다. 
*필기 캡처를 올리려고 했으나 너무 악필이라 텍스트로 대신함*

* DAO(Data Access Object) 클래스<br>
DB접근 로직과 비즈니스 로직을 분리하여 관리한다.<br>
서비스 사용자가 다수일 때, 각 사용자가 DB connection을 따로 생성하지 않고, 미리 만들어진 DB connection 객체를 대여하여 사용하고 반환하는 개념이다.<br>
DB에 변경을 주는 경우 > [Singleton Pattern](https://blog.seotory.com/post/2016/03/java-singleton-pattern)으로 관리한다.<br>
+Persistence layer; DB에 [CRUD](https://ko.wikipedia.org/wiki/CRUD) (Java에서 Interface로 xml mapping)

* DTO(Data Transfer Object) 클래스<br>
Controller <-> View <-> Business layer <-> Persistence layer<br>
각 계층간의 data 교환을 위한 객체. -> getter, setter만 가진다.<br>
+VO와 개념상으로는 유사하나 쓰임새가 다르다.<br>
VO(Value Object): read only, 특정한 비즈니스 값을 담는 객체

## 예제

일정 관리 서비스를 만든다고 했을 때, 생성된 일정 정보를 Mybatis를 통해 Oracle DB에 저장하는 경우, 다음과 같은 구조를 생각할 수 있다.

<img width="703" alt="파일_008" src="https://user-images.githubusercontent.com/12293076/61504283-cbb23600-aa15-11e9-8512-5b7dde63afa8.png">

위 세줄의 이유로 `java.util.calendar`를 통해 시간정보를 생성하여 이것을 일정 포멧의 String으로 변환하여 DTO에 싣도록 했다. Oracle의 TO_DATE 함수를 이용해서 String을 DATE 타입으로 변환하여 저장한다.

.java 파일들을 작성하는 것은 어렵지 않았으나 쿼리 짜는데에도 익숙치 않고 Mybatis에서 DTO class의 변수와 Mapper가 연동되는 룰을 잘 알아보지 않은 상황에서 xml 파일에 쿼리 형식을 작성하는 것이 까다로웠다. 전체적인 과정은 다음과 같다.

```
Mybatis mapper의 쿼리 -> 파싱된 쿼리 -> Oracle에 입력된 쿼리 
```

Mybatis mapper의 xml 파일에 mapper interface에 선언된 각 함수들이 호출될 때 실행될 쿼리들을 작성할 수 있다. 해당 쿼리들은 입력인자로, 혹은 반환값으로 DTO를 쓸 수 있다. DTO에 선언된 변수들을 쿼리에 mapping하는 방법은 `#`을 쓰는 방법과, `$`를 쓰는 방법이 있다.

![파일_007](https://user-images.githubusercontent.com/12293076/61504407-31062700-aa16-11e9-9e5a-c9dacdfb393e.png)

![파일_006](https://user-images.githubusercontent.com/12293076/61504400-2ba8dc80-aa16-11e9-894f-1887248dc95b.png)

언뜻보면 `#`이 모든 상황에서 좋아 보이지만, 꼭 그렇지만도 않다. [규우82님 블로그](https://lng1982.tistory.com/246)에 각 상황별 예시가 잘 나와 있으니 참고하자. 

## 끝
대학원때부터 Machine Learning만 주구장창 팠는데 아직도 잘 모르겠다. 이 분야의 발전 속도는 매우 빠른데 나는 제대로 따라가고 있는가 하는 생각이 든다. 오랜만에 자바를 공부하면서 Deep Learning과 크게 관련이 없는 프로그래밍을 해봤다. 이것도 나름대로 꽤 흥미로운 경험이었다. 한 분야에만 몰두하다보면 주변을 둘러보기 힘들다. 컴퓨터라는 방대한 분야에서 AI는 하나의 파트일 뿐이다. 답답한 도심에서 잠시 벗어나 가벼운 산책을 하고 온 느낌이다. 다시 열심을 찾고 심기일전 해야겠다.
