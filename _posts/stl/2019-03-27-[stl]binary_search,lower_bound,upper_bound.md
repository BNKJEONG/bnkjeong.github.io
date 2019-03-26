---
layout: post
title: "[C++ STL] binary_search, lower_bound, upper_bound"
category: STL
permalink: /ps/:year/:month/:day/:title/
tags: [STL, binary_search, lower_bound, upper_bound]
comments: true
---

## std::binary_search

[Definition](https://en.cppreference.com/w/cpp/algorithm/binary_search)

```c++
template< class ForwardIt, class T, class Compare >
constexpr bool binary_search( ForwardIt first, ForwardIt last, const T& value, Compare comp );
```

이분탐색을 실행하여 $O(lgN)​$만에 해당하는 값이 컨테이너에 있는지 여부를 알려주는 함수이다. 기본적으로 오름차순 기준으로 설정되어 있으며 내림차순 기준으로 변경할 수 있다.

* **오름차순**

```c++
int main(void)
{
    vector<int> v = {1, 2, 3, 5, 7};

    cout << (binary_search(v.begin(),v.end(),3) ? "Found" : "Not Found") << endl;
    cout << (binary_search(v.begin(),v.end(),4) ? "Found" : "Not Found") << endl;

    return 0;
}
```

* **내림차순**

```c++
int main(void)
{
    vector<int> v = {7, 5, 3, 2, 1};

    cout << (binary_search(v.begin(),v.end(),3,greater<int>()) ? "Found" : "Not Found") << endl;
    cout << (binary_search(v.begin(),v.end(),4,greater<int>()) ? "Found" : "Not Found") << endl;

    return 0;
}
```

## std::lower_bound

[Definition](https://en.cppreference.com/w/cpp/algorithm/lower_bound)

```c++
template< class ForwardIt, class T, class Compare >
constexpr ForwardIt lower_bound( ForwardIt first, ForwardIt last, const T& value, Compare comp );
```

이분탐색을 실행하여 $O(lgN)$ 만에 해당하는 값 이상의 값의 위치를 가리키는 반복자(Iterator)를 리턴하는 함수이다. 아래와 같이 사용할 수 있으며 **벡터의 반복자를 이용하여 인덱스를 구하는 것은 현재 반복자에서 시작위치의 반복자를 빼는 것과 같다는 것을 알아두자.**

```c++
int main(void)
{
    vector<int> v = {1, 2, 3, 5, 7};

    vector<int>::iterator it = lower_bound(v.begin(),v.end(),4);
    auto it2 = lower_bound(v.begin(),v.end(),5);

    cout << "index of number which is greater than equal to 4: " << it - v.begin() << endl;
    cout << "index of number which is greater than equal to 5: " << it2 - v.begin() << endl;

    return 0;
}
```

`std::binary_search`와 같이 내림차순으로도 쓸 수 있다.

## std::upper_bound

[Definition](https://en.cppreference.com/w/cpp/algorithm/upper_bound)

```c++
template< class ForwardIt, class T, class Compare >
constexpr ForwardIt upper_bound( ForwardIt first, ForwardIt last, const T& value, Compare comp );
```

`std::lower_bound`와 동일하지만 해당 값 이상에서 해당 값 초과로 바뀐 것, 즉 찾고자 하는 값이 3이라면 3보다 큰 값이 있는 위치의 반복자를 리턴하는 함수이다.

```c++
int main(void)
{
    vector<int> v = {1, 2, 3, 5, 7};

    vector<int>::iterator it = upper_bound(v.begin(),v.end(),4);
    auto it2 = upper_bound(v.begin(),v.end(),5);

    cout << "index of number which is greater than 4: " << it - v.begin() << endl;
    cout << "index of number which is greater than 5: " << it2 - v.begin() << endl;


    return 0;
}
```

실행해보면 `std::lower_bound`와는 다른 인덱스 값이 나오는 것을 볼 수 있다.

## References

* https://en.cppreference.com/w/
* https://www.youtube.com/watch?v=rXuqUtifDU8

