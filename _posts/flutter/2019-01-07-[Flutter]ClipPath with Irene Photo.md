---
layout: post
title: "[Flutter] ClipPath로 아이린 사진 커브 만들기"
category: Flutter(플러터)
permalink: /flutter/:year/:month/:day/:title/
tags: [플러터, 다트]
comments: true
---

UI관련해서 공부를 하다보니 Canvas, Clipper, Path 들에게 익숙해져야 하는 것을 깨달았다. 그래서 이번엔 ClipPath 위젯에 대해 공부한 바를 정리해보려고 한다. [Clipping widgets with bezier curves in Flutter](Clipping widgets with bezier curves in Flutter)를 공부해서 정리하는 글이다.

## 클립을 하자.

클립을 한다는 것은 이미지나 특정 도형을 원하는 모양으로 자르는 것을 말한다. 이 때 사용할 수 있는 것이 [CustomClipper](https://docs.flutter.io/flutter/rendering/CustomClipper-class.html) 위젯이며 해당 종류로 여러가지 위젯이 있지만 그 중에 [ClipPath](https://docs.flutter.io/flutter/widgets/ClipPath-class.html)를 사용할 것이다. CustomClipper는 말 그대로 자신만의 커스터마이징한 클립을 적용하기 위함이며 ClipPath는 그 클립을 할 때 [Path](https://docs.flutter.io/flutter/dart-ui/Path-class.html)를 활용한다는 것이다. 

Path는 주어진 이미지를 `current point`에서 다양하게 변형할 수 있는 여러가지 subpath를 제공한다. 그럼 subpath가 무엇인지 또 뭐지? 할텐데 subpath는 선,곡선, 호 같은 어떤 모양을 가진 세그먼트들을 말한다. `current point`는 초기에 왼쪽 위인 (0,0)이지만 subpath를 통해 계속 이동시키게 되면 업데이트가 된다. 그러니까 정리하자면 `current point`에서 시작해서 subpath를 이용해 이동시키면서 이미지를 변형시키는 객체를 말한다. 따라서 Path를 활용하면 클립이 가능해지는 것이다.

## 기본세팅

아이린 이미지를 준비하고 누구나 하는 기본세팅을 해주자. 이걸 하면서 상태바를 제거하려고 검색을 해보니 다음과 같은 꿀팁이 있었다.

```dart
import 'package:flutter/services.dart';

SystemChrome.setEnabledSystemUIOverlays([]);
```

이걸 적용해서 이미지만 띄워주자.

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  SystemChrome.setEnabledSystemUIOverlays([]);
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Clock',
      theme: ThemeData(
        primarySwatch: Colors.blue
      ),
      home: IreneClip(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class IreneClip extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Image.asset('assets/irene.jpg'),
    );
  }
}
```

<img src="https://user-images.githubusercontent.com/35518072/50767200-44924180-12bf-11e9-8220-d468ade46976.png" width="400px">

## ClipPath 적용

ClipPath로 이미지 부분을 감싸주고 `clipper` 속성에 커스터마이징하는 클리퍼를 적용해주면 된다.

```dart
class IreneClip extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ClipPath(
        child: Image.asset('assets/irene.jpg'),
        clipper: IreneClipper(),
      ),
    );
  }
}
```

## CustomClipper 만들기

이제 CustomClipper를 만들어야 하는데 이걸 만들 땐 `CustomClipper<Path>`를 상속하는 클래스를 하나 정의해주면 된다. 이 클래스는 `getClip()`과 `shouldReclip()` 메소드를 오버라이딩 해야 하는데, `getClip()`은 Path로 클립을 할거니 해당 Path 객체를 넘겨주면 되고 `shouldReclip()`은 프레임워크에게 해당 클립이 다시 적용되어야 하는지 여부를 반환하는 메소드이다.

주의할 점은 이것 때문에 계속 삽질했는데 이걸 `true`값으로 하지 않으면 hot reload로 볼 수가 없다! 그러니 테스트하기 위해선 꼭 `true`값으로 설정해주자.

```dart
class IreneClipper extends CustomClipper<Path> {
  @override
  Path getClip(Size size) {
    Path path = Path();
    return path;
  }

  @override
  bool shouldReclip(CustomClipper<Path> oldClipper) {
    return true;
  }
}
```

먼저 Path 객체를 생성해서 그냥 리턴해주고 있다. 아무런 subpath도 사용하지 않았기 때문에 변화가 없다. 이제 subpath 세그먼트를 활용하여 변형을 적용해주도록 하자. 여기서 사용할 메소드는 `lineTo()`와 `quadraticBezierTo()`라는 메소드이다. 두번째는 상당히 기괴해보이는데 나도 처음엔 쫄았지만 걱정할 필요가 없다. 우선 어떻게 동작하는지 살펴보자.

`lineTo()`는 주어진 현재점(current point)에서 좌표를 설정해서 선을 긋는 것이다. 현재 점에서 위 사진을 대각선으로 두 동강내기 위해선 다음과 같이 적용하면 된다.

```dart
path.lineTo(size.width, 0.0);
path.lineTo(size.width, size.height);
```

현재점이 (0.0,0.0)부터 시작하는데 거기서부터 이미지의 너비만큼, 즉 x축 기준 마지막 값까지 선을 긋는다는 것이 위쪽 코드이고 아래쪽 코드는 거기서부터 다시 y축 기준 마지막 값까지 선을 긋는다는 것이다. 따라서 아래 그림과 같이 나타난다.

<img src="https://user-images.githubusercontent.com/35518072/50767667-06961d00-12c1-11e9-963e-94f7c5941668.png" width="400px">

그럼 이제 `quadraticBezierTo()` 만 이해하면 되는데 이건 그림으로 하는게 더 쉽다.

<img src="https://user-images.githubusercontent.com/35518072/50767909-e87cec80-12c1-11e9-96c4-e73a9737c839.png">

잘 못그렸지만, 위처럼 Control Point를 움직이면서 변형시킨다는 말이다. 이걸 이용해서 코드를 짜보면 아래와 같이 짤 수 있다.

```dart
class IreneClipper extends CustomClipper<Path> {
  @override
  Path getClip(Size size) {
    Path path = Path();
    path.lineTo(0.0, size.height-140);

	// Control Point와 End Point 설정
    final firstControlPoint = Offset(size.width/4, size.height-60);
    final firstEndPoint = Offset(size.width/2.15, size.height-100);
    path.quadraticBezierTo(firstControlPoint.dx, firstControlPoint.dy, 
      firstEndPoint.dx, firstEndPoint.dy);

    final secondControlPoint = Offset(size.width-size.width/4, size.height-140);
    final secondEndPoint = Offset(size.width, size.height-80);
    path.quadraticBezierTo(secondControlPoint.dx, secondControlPoint.dy, 
        secondEndPoint.dx, secondEndPoint.dy);

    path.lineTo(size.width, 0.0);
    path.close();
    return path;
  }

  @override
  bool shouldReclip(CustomClipper<Path> oldClipper) {
    return true;
  }
}
```

복잡해보이지만 정말 아무것도 아니다, `quadraticBezierTo()`의 1,2번째 인자는 control point의 좌표이고 3,4번째 인자는 end point의 인자인 것을 알면 아까 `lineTo()`를 적용했던 것처럼 `size.width`와 `size.height` 값을 이용해 변형을 시킬 수 있다. 결과는 아래와 같다.

<img src="https://user-images.githubusercontent.com/35518072/50767998-3e519480-12c2-11e9-9a23-93534a74ee4a.png" width="400px">