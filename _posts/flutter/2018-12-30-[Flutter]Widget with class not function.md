---
layout: post
title: "[Flutter] 위젯을 생성할 땐 함수가 아닌 클래스로 만들어야 한다.(번역)"
category: Flutter(플러터)
permalink: /flutter/:year/:month/:day/:title/
tags: [플러터, 다트]
comments: true
---

[What is the difference between functions and classes to create widgets?](https://stackoverflow.com/a/53234826/10238194) 를 번역한 글이다.

요약해서, **절대 재사용할 수 있는 위젯트리를 만드는데 함수를 쓰지 말라.** 항상 [StatelessWidget](https://docs.flutter.io/flutter/widgets/StatelessWidget-class.html)으로 대신 써라.

-------

클래스 대신 함수를 쓰는 것은 *엄청난* 차이점이 있다: 프레임워크는 함수를 인지하지 못하지만 클래스는 인지하기 때문이다.

위젯을 생성하는 함수를 보자:

```dart
Widget functionWidget({ Widget child}) {
  return Container(child: child);
}
```

이걸 아래 방식으로 사용한다고 하자:

```dart
functionWidget(
  child: functionWidget(),
);
```

이제 동일하게 동작하는 클래스이다:

```dart
class ClassWidget extends StatelessWidget {
  final Widget child;

  const ClassWidget({Key key, this.child}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      child: child,
    );
  }
}
```

마찬가지로:

```dart
new ClassWidget(
  child: new ClassWidget(),
);
```

사실상, 똑같은 일을 한다: 2개의 `Container`를 중첩하는 방식으로 생성한다. 그러나 실제로 약간 다르다.

함수의 경우, 생성된 위젯트리는 다음과 같이 생겼다:

```dart
Container
  Container
```

반면에 클래스의 경우, 위젯트리는 아래와 같다:

```dart
ClassWidget
  Container
    ClassWidget
      Container
```

이것은 **매우** 중요한데 그 이유는 위젯을 업데이트 시킬 때 근본적으로 프레임워크가 동작하는 방식을 결정하기 때문이다. 중요한 차이점들을 몇가지 보자:

* `ClassWidget`이 위젯트리에 들어간 후에 [Element](https://docs.flutter.io/flutter/widgets/Element-class.html)와 연관되고 그에 따라, [BuildContext](https://docs.flutter.io/flutter/widgets/BuildContext-class.html)를 갖으므로 `Key` 값을 가진다. 따라서 `ClassWidget`이 다른 위젯과 독립적으로 업데이트 될 수 있기 때문에 더 나은 성능을 갖게 된다.
* `ClassWidget`은 hot-reload 될 수 있지만 `functionWidget`은 안된다. 프레임워크는 `functionWidget`을 볼 수 없기 때문에 코드를 변경해도 hot-reload 할게 있는지 알 수 없다.
* `functionWidget`을 사용하게 되면 업데이트를 한 후에 사용하려고 할 때 이상한 버그를 발견할 수 있다. 이건 프레임워크가 볼 수 없어서 새로운 걸 만들지 않고 이전 위젯을 사용하기 때문이다.
* 클래스를 사용하면, 레이아웃의 각 부분들이 모두 생성자를 이용해서 사용된다. 또한 구현의 세부사항을 추상화시킨다. 이것이 중요한 이유는 [StatefulWidget](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)으로 바꾸려고 많이 바꿔야 하기 때문이다.

결론은 확실하다:

**절대 위젯을 생성하는데 함수를 사용하지 말 것!!**