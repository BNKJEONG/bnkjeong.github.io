---
layout: post
title: "[Dart] as, show, hide의 차이점"
category: Dart(다트)
permalink: /dart/:year/:month/:day/:title/
tags: [Dart, 다트]
---

[What is the difference between “show” and “as” in an import statement?](https://stackoverflow.com/questions/19723063/what-is-the-difference-between-show-and-as-in-an-import-statement) 를 공부한 글이다.

### as

`as`는 라이브러리에게 이름을 주어서 다른 것들과 중복되지 않게 해당 라이브러리의 클래스나 함수에 접근하게 해주는 키워드이다. 다음과 같이 사용할 수 있다.

```dart
import 'package:google_maps/google_maps.dart' as GoogleMap;

/// 사용
GoogleMap.LatLng...
```

### show

`show`는 라이브러리에서 특정 클래스나 함수를 노출시키는 키워드이다. 따라서 나머지는 노출되지 않는다.

```dart
import 'package:google_maps/google_maps.dart' show LatLng;
```

### hide

`hide`는 `show`와 반대로 해당 클래스나 함수를 숨기고 나머지를 노출시키는 키워드이다.

```dart
import 'package:google_maps/google_maps.dart' hide LatLng;
```

### 혼합

섞어서 다음과 같이 사용할 수도 있다.

```dart
import 'package:google_maps/google_maps.dart' as GoogleMap show LatLng;
```



