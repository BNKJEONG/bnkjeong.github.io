---
layout: post
title: "[Design Pattern] MVC, MVP와 MVVM"
category: 디자인 패턴
permalink: /design_pattern/:year/:month/:day/:title/
tags: [design pattern, mvc, mvp, mvvm]
comments: true
---

플러터를 공부하면서 소프트웨어 아키텍쳐에 대해 공부하게 되었는데 가장 기본이 되는 MVC, MVP, MVVM을 알지 못했던 상황이라 한 번 정리를 하고자 한다. 먼저 각자 뭐의 약자인지 알아보자.

* MVC: Model - View - Controller
* MVP: Model - View - Presenter
* MVVM: Model - View - ViewModel

비슷비슷하게 생겨서 헷갈릴 수 있지만 각 아키텍쳐의 성격이 다르고 소프트웨어를 디자인 할 때 의존성을 줄이면서 견고하게 만들기 위해서 상당히 중요하게 작용하기 때문에 확실히 이해해야 한다.

## MVC(Model-View-Controller)

<img src="../../assets/post-img/design pattern/mvc.png" width="600px">

각 컴포넌트가 무엇을 의미하며 어떤 동작을 하는지는 아래와 같다.

* **Model**: 모델은 데이터를 나타낸다고 보면 되며 비즈니스 로직을 처리하는 부분이다.
* **View**: 뷰는 말 그대로 화면에 UI로 나타나는 부분이다.
* **Controller**: 컨트롤러는 사용자의 요청에 따라 모델과 상호작용하여 요청을 처리한 후 뷰에 선택하는 역할을 하는 부분이다.

1. 사용자가 Controller에게 작업을 요청한다.
2. Controller는 해당 작업을 해석하여 Model을 호출하고 작업을 처리한다. (Model의 업데이트)
3. Model은 처리한 작업의 결과를 Controller에게 반환한다.
4. Controller는 받은 결과를 바탕으로 View를 선택한다. (하나의 Controller는 여러개의 View를 관리할 수 있으므로 1:N 관계라고도 한다.)
5. Controller가 선택한 View는 Model에 업데이트 된 데이터를 기반으로 UI가 업데이트된다.
6. 사용자는 업데이는 한 UI를 보게 된다.

5번에서 View를 업데이트 할 때 Model과의 상호작용을 통해 업데이트를 해야 하기 때문에 View와 Model은 의존성이 여전히 있다는 단점이 있다.

## MVP(Model-View-Presenter)

<img src="../../assets/post-img/design pattern/mvp.png" width="600px">

Model과 View는 동일하지만 Controller가 Presenter로 바뀌었고 View와 Model은 Presenter를 통해 동작한다.

1. 사용자가 View에 요청을 받는다. 즉, Controller가 아닌 View가 요청을 받는다.
2. View는 받은 요청을 Presenter에게 전달한다.
3. Presenter는 Model에 처리할 작업을 요청한다.
4. Model은 Presenter에게 받은 작업을 처리하고 Presenter에게 결과를 반환한다.
5. Presenter는 Model에게 받은 정보를 바탕으로 View를 업데이트한다.
6. View는 업데이트된 정보로 사용자에게 응답한다.
7. 사용자는 업데이트된 UI를 보게 된다.

Model과 View 사이의 의존성은 없어졌지만 View와 Presenter간의 의존성이 생겼기 때문에 여전히 문제가 있다. 또한 View와 Presenter는 1:1 관계이므로 그 의존성도 상당히 큰 편이다.

## MVVM(Model-View-ViewModel)

<img src="../../assets/post-img/design pattern/mvvm.png" width="600px">

Presenter가 ViewModel로 바뀌었는데 ViewModel이 이 아키텍쳐의 핵심이다. 

> *ViewModel은 View를 추상화한 모델로 View는 ViewModel을 알지만 ViewModel은 View를 알지 못한다. ViewModel은 뷰의 상태(ViewState)를 유지하여 View의 상태를 추상화하며 해당 상태를 기반으로 View가 업데이트 된다. 또한 명령(Commands)를 가지기 때문에 View를 통해서 ViewModel에게 명령을 요청할 수 있다. 마지막으로, 값 변환기(ValueConverter)를 가져서 Model로부터 받은 정보를 해당 View에 맞게 원하는 대로 변형하여 업데이트 할 수 있다.* (이규원님 블로그에서 발췌)

1. 사용자가 View에 요청을 한다.
2. View는 **커맨드 패턴(Command Pattern)**을 활용하여 ViewModel에 사용자의 요청을 전달한다.
3. ViewModel은 해당 요청을 바탕으로 Model에 작업 처리를 요청한다.
4. Model은 작업을 처리하여 ViewModel에 해당 결과를 반환한다.
5. ViewModel은 반환된 정보를 가공하여 저장한다.
6. View는 ViewModel과 **데이터 바인딩(Data Binding)**이 되어있기 때문에 해당하는 정보가 업데이트 되면 자동으로 업데이트 된다.
7. 사용자는 업데이트 된 View로부터 응답을 받는다.

위 과정에서 말한 커맨드 패턴은 처음에 ViewModel이 명령을 가진다고 한 의미와 동치이며, 데이터 바인딩은 View와 ViewModel이 바인딩되어 양방향으로 정보가 갱신되는 것을 의미한다. 즉, ViewModel의 상태가 변경되면 View가 갱신되고 View가 변경되면 ViewModel이 갱신되는 것이 보장된다.

## 출처

* [이규원님 블로그 - MVVM 아키텍처 패턴](https://justhackem.wordpress.com/2017/03/05/mvvm-architectural-pattern/)
* [이승현님 브런치 - MVC, MVP, MVVM, MVI](https://brunch.co.kr/@oemilk/113)
* [마기님 블로그 - MVC, MVP, MVVM 비교](https://magi82.github.io/android-mvc-mvp-mvvm/)