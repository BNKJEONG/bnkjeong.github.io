---
layout: post
title: "[JAVA]String vs StringBuffer vs StringBuilder"
category: java
permalink: /ps/:year/:month/:day/:title/
tags: [java, string, stringbuffer, stringbuilder]
comments: true
---

## String

* [String](https://docs.oracle.com/javase/9/docs/api/java/lang/String.html)

일반적인 문자열 클래스이며 접합(concatenation)연산이 가능하다. 그러나 접합연산에 있어서 기존의 객체를 버리고 다시 새로운 객체를 생성한 뒤에 복사해서 접합하기 때문에 상당히 비효율적이다. 따라서, 많은 양의 문자열을 접합할 경우에는 쓰지 않는 것이 좋다. 하지만 불변(immutable)한다는 특징을 갖고 있어 단순히 읽는 연산에서는 다른 클래스보다 성능이 좋기 때문에 읽기연산에서는 String 을 사용하는 것이 좋다.



## StringBuffer vs StringBuilder

* [StringBuffer](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuffer.html)
* [StringBuilder](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuilder.html)

StringBuffer와 StringBuilder는 모두 가변적(mutable)이고 String 과 다르게 접합연산에서 다시 객체를 생성하지 않는다. 따라서 많은 양의 문자열을 접합할 경우에 훨씬 효율적이다. StringBuffer의 경우 thread-safe하며 StringBuilder는 thread-safe하지 않다. 따라서, 멀티쓰레드 환경이라면 StringBuffer를 사용하고 싱글쓰레드 환경이라면 StringBuilder를 사용하는 것이 좋다.



## 결론

* 문자열이 변하지 않을 경우 String을 사용하자.
* 문자열이 변하는데 싱글 쓰레드일 경우 StringBuilder를 사용하자.
* 문자열이 변하는데 멀티 쓰레드일 경우 StringBuffer를 사용하자.



## Refrences

* https://stackoverflow.com/a/2971343/9437175
* https://jeong-pro.tistory.com/85
* 