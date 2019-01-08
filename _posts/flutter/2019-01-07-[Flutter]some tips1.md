---
layout: post
title: "[Flutter] 여러가지 팁들1"
category: Flutter(플러터)
permalink: /flutter/:year/:month/:day/:title/
tags: [플러터, 다트]
comments: true
---

## 오늘 배운 여러가지 팁들

* Material 위젯

  * `elevation`이나 `borderRadius` 같은 속성이 있어서 다른 위젯의 모양 틀을 만드는데 유용하다.

* InputDecoration 위젯의 `suffix` 속성

  * 입력되는 값 이후에 놓을 위젯을 1개 선택할 수 있다.
  * 예를 들어, 검색모양을 만든다 치면 검색하고 서치하는 아이콘을 놓을 수 있는 것.

* Colors 위젯의 `withOpacity` 속성

  * 위젯을 조금 옅은 박스로 감싸는데 사용했었는데 투명도 조절하는데 좋다.

* MainAxisAlignment 위젯의 여백 속성들

  * `spaceAround` : 여백이 크기의 반이 안되게 첫번째 자식~ 마지막 자식을 고르게 배치한다.
  * `spaceBetween` : 자식들간에 여백을 고르게 배치한다.
  * `spaceEvenly` : 첫번째 자식~마지막 자식을 고르게 배치한다.
  * [레이아웃 치트시트 참고](https://medium.com/jlouage/flutter-row-column-cheat-sheet-78c38d242041)

* 배경색으로 그라데이션 넣고 싶을 때

  * Container에 BoxDecoration 넣고 `gradient` 속성에 LinearGradient 위젯을 넣어주면 된다.

  * ```dart
    Container(
        height: 400.0,
        decoration: BoxDecoration(
            gradient: LinearGradient(
                colors: [firstColor,secondColor]
            )
        )
    )
    ```

* PopupMenuButton은 누르면 선택할 수 있는 목록들이 나오는 버튼으로 PopupMenuItem과 같이 쓰인다.

* Text에 `textAlign` 속성이 있다는 걸 잊지 말자.

* InkWell은 부모가 반드시 Material 위젯이어야 하며 터치했을 때 이벤트를 발생시켜야 하는데 커스터마이징한 컴포넌트일 경우 주로 사용한다.

* Spacer는 Row,Column 또는 Flex에서 여백을 주기 위해 사용한다. Expanded에 컨테이너 넣어서 사용하는 것보다 간편하다 :)