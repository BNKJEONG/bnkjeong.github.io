---
layout: post
title: "[C++] equal_range, distance"
category: C++
permalink: /c++/:year/:month/:day/:title/
tags: [STL, equal_range, distance]
comments: true
---

## std::equal_range

[Definition](https://en.cppreference.com/w/cpp/algorithm/equal_range)

```c++
template< class ForwardIt, class T >
std::pair<ForwardIt,ForwardIt> 
	equal_range( ForwardIt first, ForwardIt last,const T& value );
```

2개의 반복자를 기준으로 주어진 범위 내에서 `value`를 만족하는 범위를 나타내는 2개의 반복자를 리턴한다. 컨테이너는 정렬된 상태여야 한다.  리턴되는 1번째 반복자는 `value` 이상이며 2번째 반복자는 `value` 초과이다. 다시 말해서 1번째 반복자는 `std::lower_bound`로 얻을 수 있고 2번째 반복자는 `std::upper_bound`로 얻을 수 있다. 시간복잡도는 여전히 $O(lgN)$이다.

```c++
int main(void)
{
    vector<int> v = {1, 1, 2, 3, 3, 4, 5};
    pair<vector<int>::iterator, vector<int>::iterator> p = equal_range(v.begin(),v.end(),3);
    cout << *p.first << ' '<< *p.second << '\n';
    return 0;
}
```

결과는 3과 4가 나옴을 알 수 있다.

## std::distance

[Definition](https://en.cppreference.com/w/cpp/algorithm/distance)

```c++
template< class InputIt >
typename std::iterator_traits<InputIt>::difference_type 
    distance( InputIt first, InputIt last );
```

1번째 반복자와 2번째 반복자를 포함해서 그 사이에 있는 원소들의 개수를 구한다.

```c++
int main(void)
{
    vector<int> v = {1, 1, 2, 3, 3, 4, 5};
    cout << distance(v.begin(),v.end()) << '\n';
    return 0;
}
```

7이 나온다.

