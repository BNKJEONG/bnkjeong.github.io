---
layout: post
title: "[Flutter] Stateful Widget의 생명주기"
category: Flutter(플러터)
permalink: /flutter/:year/:month/:day/:title/
tags: [플러터, 다트]
comments: true
---

[Stateful Widget Lifecycle](https://flutterbyexample.com/stateful-widget-lifecycle/)을 공부하며 정리한 글이다.

StatefulWidget은 그 자체로 immutable(변경할 수 없는) 위젯이지만 그에 따른 State 객체를 가지고 있고 해당 State 객체는 mutable(변경할 수 있는) 객체이기 때문에 State를 변경하는 것을 통해 동적으로 상태를 변화시키면서 UI를 업데이트 할 수 있다. 이제 StatefulWidget과 State의 생명주기를 살펴보자.

## 1. createState()

StatefulWidget이 만들어지자마자, `createState()`가 호출되며 이 메소드는 반드시 존재해야 한다. `createState()`는 State 객체를 생성하고 BuildContext(위젯 트리에서 위젯이 어디에 위치하는지를 나타내는 클래스)는 해당 State 객체를 할당받는다. 위젯 트리가 State 객체를 할당 받을 때 모든 위젯에 있는 `mounted` 속성은 `true`로 된다. 이 속성은 `setState()` 메소드를 사용할 때 유용한데 그 이유는 `mounted`값이 `false`인데  `setState()`를 호출하면 에러가 발생하기 때문이다. 

## 2. initState()

위젯이 만들어진 후, 그러니까 생성자가 호출되고 첫번째로 호출되는 메소드이다. `initState()`는 단 한번만 호출되며 반드시 `super.initState()`를 호출하여야 한다. 아래와 같은 상황에 오버라이딩하는 것이 좋다.

* 특정한 BuildContext에 의존하는 데이터들을 초기화하려고 할 때
* 위젯 트리에서 "부모" 위젯에 의존하는 속성들을 초기화하려고 할 때
* 위젯의 데이터를 변화시킬 수 있는 Stream, ChangeNotifier나 다른 객체들을 구독하려고 할 때

## 3. didChangeDependencies()

`initState()`가 호출되고 나서 바로 호출되는 메소드이며 위젯이 데이터를 의존하는 다른 위젯이 업데이트 되었을 때도 호출된다. 예를 들어, 이전 `build()` 메소드 호출에서 InheritedWidget을 참조하고 있다가 나중에 업데이트가 되면 프레임워크는 현재 위젯에 변화를 알리기 위해서 이 메소드를 호출한다.

하지만 보통 프레임워크는 변화를 다 반영한 후에 `build()`를 호출하기 때문에 서브클래스에서 오버라이딩 하는 경우는 거의 없지만 네트워크에서 데이터를 가져오는 값비싼 작업에서 오버라이딩 하기도 한다.

## 4. build()

실제적인 위젯을 생성해내는 메소드로 현재 위젯의 서브트리를 제거하고 새로운 서브트리로 교체하거나 아니면 업데이트 하는데 사용된다.

## 5. didUpdateWidget(Widget oldWidget)

부모 위젯이 다시 생성되고 위젯 트리에서 현재의 위치가 동일한 `runtimeType`과 `Widget.key`를 가진 채로 새로운 위젯을 나타내기를 요청하면, 프레임워크는 State 객체의 widget 속성을 업데이트하고 이전 위젯을 인자로 하여 이 메소드를 호출한다. 프레임워크는 이 메소드를 호출한 뒤에 `build()`를 호출하며 이 말은 이 메소드에서 `setState()`를 호출할 경우 중복된다는 것이다.

## 6. setState()

상당히 자주 호출되는 메소드로 프레임워크에게 데이터의 변화를 알림과 동시에 현재의 BuildContext가 rebuild되어야 한다는 것을 말해주는 메소드이다. 이 메소드는 콜백함수를 포함하는데 해당 콜백함수는 비동기여서는 안된다.

## 7. deactivate()

거의 사용되지 않으며 State가 트리에서 제거될 때 호출된다. 하지만 현재 프레임의 변화가 끝나기 전에 다시 트리에 삽입되기도 하기 때문에 호출되었다고 해서 제거된 것으로 볼 수는 없다. 위젯 트리의 다른 위치로 이동했을 수도 있기 때문이다.

## 8. dispose()

State 객체가 영구적으로 제거될 때 호출되며 애니메이션이나 스트림에 대한 구독을 취소할 때 사용한다. State가 다시 마운트될 수 없는 경우 `mounted`값은 `false`가 된다.

