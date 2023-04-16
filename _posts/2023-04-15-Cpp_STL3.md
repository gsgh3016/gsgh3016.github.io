---
layout: single
title: 표준 템플릿 라이브러리(STL)(3) 반복자(iterator)
categories: C++
---
# 1. iterator vs pointer
https://en.cppreference.com/w/cpp/iterator

cppreference에서는 iterator를 다음과 같이 서술한다.

> Iterators are a generalization of pointers that allow a C++ program to work with different data structures in a **uniform manner**.

> Iterator는 C++가 다른 구조의 데이터를 **동일한 방식**으로 처리하게끔 도와주는 포인터의 일반 형태이다.

C에서 포인터를 쓰면 종종 Segmentation Fault가 발생하는데,
많은 경우 NULL 포인터(주소 값이 0x0000)를 가리키면서 발생한다.

메모리에 직접 접근하는 Low level 언어는 이런 오류가 종종 있다.

조금 더 안전한 프로그래밍을 위해 iterator를 고안하지 않았나 싶다.
<br>
iterator는 pointer의 추상화 버젼인만큼 따로 생성자가 있다.
직접적인 정보(주소)를 숨기면서 메모리 주소 값을 따로 호출한다.

```cpp
#include <iostream>

int main() {
    int* num;
    std::cout << num << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/2963d949-7cca-4a16-83c9-0828090a6694/image.png)

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec;
    std::cout << vec.begin() << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/10b0308c-9d28-43f1-867a-46b697ad69f2/image.png)
ChatGPT한테 표로 만들어 달라고 부탁했다.

<table>
  <tr>
    <th></th>
    <th>Pointers</th>
    <th>Iterators</th>
  </tr>
  <tr>
    <td>Similarities</td>
    <td>Used to access and manipulate memory addresses</td>
    <td>Used to access elements of containers</td>
  </tr>
  <tr>
    <td></td>
    <td>Allow traversal of data structures</td>
    <td>Allow traversal of container elements</td>
  </tr>
  <tr>
    <td></td>
    <td>Support pointer arithmetic</td>
    <td>Support iterator arithmetic</td>
  </tr>
  <tr>
    <td>Differences</td>
    <td>Low-level construct in C and C++</td>
    <td>Higher-level abstraction in C++ STL</td>
  </tr>
  <tr>
    <td></td>
    <td>Directly access memory addresses of variables</td>
    <td>Access container elements through an interface</td>
  </tr>
  <tr>
    <td></td>
    <td>Can point to any memory location, including invalid or uninitialized ones</td>
    <td>Designed to work only with valid elements in a container</td>
  </tr>
  <tr>
    <td></td>
    <td>Dereferencing a null pointer results in undefined behavior</td>
    <td>Dereferencing an invalid or end iterator usually results in a runtime error or exception</td>
  </tr>
</table>
<br>

***
# 2. ranged-based-for
ranged-based-for는 C에서보다 오류에 안전한 반복문 형식이다.
만약 벡터를 다음과 같이 접근하면,
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3};
    std::cout << "--------" << std::endl;
    for(int i = 0; i <= vec.size(); i++){	// 원소 개수보다 하나 더 실행됨
        std::cout << vec[i] << ' ';
    }
    std::cout << std::endl << "--------" << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/e3b963b8-b47e-4b5e-8579-aed0d8d98563/image.png)
```i```가 ```3```일때, 더미 값이 출력된다.
이를 방지하기 위해 ranged-based-for문을 사용한다.
<br>
ranged-based-for문의 형식은 다음과 같다.

> ```for(element: Container)```

위 코드를 ranged-based-for문으로 수정해보자.
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3};
    std::cout << "------" << std::endl;
    for(auto it: vec){						// 이때 auto == int
        std::cout << it << ' ';
    }
    std::cout << std::endl << "------" << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/fc4ded61-0fe5-4597-859d-3c1ea21a3b82/image.png)

```for```문을 보면 ```for(auto it: vec)```으로 표현했다.
이때 일반 iterator와는 다른 성질을 보인다.
바로 변수 참조를 하지 않는다는 것이다.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3};
    std::cout << "---------------" << std::endl;
    std::cout << "before increase" << std::endl;
    for (auto it: vec) {
        std::cout << it << ' ';
    }
    for (auto it: vec) {						// no reference
        it += 1;
    }
    std::cout << std::endl << "---------------" << std::endl;
    std::cout << "after  increase" << std::endl;
    for (auto it: vec) {
        std::cout << it << ' ';
    }

    std::cout << std::endl << "---------------" << std::endl;
    return 0;
}
```

![](https://velog.velcdn.com/images/gsgh3016/post/a7b2e4c5-33a3-45e8-92ad-7fd11467a028/image.png)

이는 ```&``` 연산자로 변수 참조를 명시해주면 해결된다.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3};
    std::cout << "---------------" << std::endl;
    std::cout << "before increase" << std::endl;
    for (auto it: vec) {
        std::cout << it << ' ';
    }
    for (auto& it: vec) {                       // with reference
        it += 1;
    }
    std::cout << std::endl << "---------------" << std::endl;
    std::cout << "after  increase" << std::endl;
    for (auto it: vec) {
        std::cout << it << ' ';
    }

    std::cout << std::endl << "---------------" << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/87338417-7d5a-4854-b541-301d05c1c408/image.png)
<br>

***
# 3. iterator 활용
## 3-1 iterator 연산자
iterater는 여러 연산이 지원된다.
```->```, ```*```, ```++```, ```--```, ```==```, ```!=``` 연산 등이 지원된다.

하지만 컨테이너에 따라 지원되기도, 되지 않기도 하는 연산이 있다.
예를 들어 ```std::map```에서 + 연산이 지원되지 않는다.
이는 ```std::map```이 트리 구조 자료형이기 때문이다.

코드로 알아보자.
```cpp
#include <iostream>
#include <map>
#include <typeinfo>
#include <cxxabi.h>

int main() {
    std::map<int, int> M = {
        {1, 11},
        {3, 22},
        {5, 33},
        {7, 44},
    };
    int status;
    char* demangled1 = abi::__cxa_demangle(typeid(M.begin()).name(), 0, 0, &status);
    std::cout << "type of M.begin(): " << demangled1 << std::endl;
    free(demangled1);
    char* demangled2 = abi::__cxa_demangle(typeid(M.cbegin()).name(), 0, 0, &status);
    std::cout << "type of M.cbegin(): " << demangled2 << std::endl;
    free(demangled2);
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/5435ef57-9033-4b84-9ca5-8f807d06260b/image.png)

결과가 tree_iterator임에 주목하자.

```_Rb_tree_iterator```는 Red-Black-tree 구조의 iterator임을 의미하는데, 이는 ```std::map```의 자료구조와 관련있다.

```+``` 연산자를 사용해서 코드를 구성하면 다음과 같은 결과를 볼 수 있다.

```cpp
#include <iostream>
#include <map>

int main() {
    std::map<int, int> M = {
        {1, 11},
        {3, 22},
        {5, 33},
        {7, 44},
    };
    auto it = M.begin();
    std::cout << it->first << ": " << it->second << std::endl;
    it = it + 2;
    std::cout << it->first << ": " << it->second << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/6f5465c8-9f30-49b3-95b5-b347f6cb1f22/image.png)

반면 ```std::vector```의 경우 ```+``` 연산자가 지원된다.
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec = {1, 2, 3};
    auto it = vec.begin();
    std::cout << *it << std::endl;
    it = it + 2;
    std::cout << *it << std::endl;
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/e3a30828-a93e-4399-8f01-d62e1bb58d83/image.png)
iterator를 잘 활용하려면, container의 자료 구조를 파악해야 한다.
<br>

***
## 3-2 함수
다음 코드를 보자.
```cpp
#include <iostream>
#include <map>

int main() {
    std::map<std::string, int> M{
        {"bbb", 1},
        {"aa", 2},
        {"c", 3}
    };
    auto it = M.begin();
    std::cout << "-----------------" << std::endl;
    std::cout << "it : index = 0" << std::endl;
    std::cout << it->first << ": " << it->second << std::endl;

    it = std::next(it);          // std::next == it++
    std::cout << "-----------------" << std::endl;
    std::cout << "it : index = 1" << std::endl;
    std::cout << it->first << ": " << it->second << std::endl;

    it = std::prev(it);          // std::prev == it--
    std::cout << "-----------------" << std::endl;
    std::cout << "it : index = 0" << std::endl;
    std::cout << it->first << ": " << it->second << std::endl;

    std::advance(it, 2);    // std::advance == +
    std::cout << "-----------------" << std::endl;
    std::cout << "it : index = 1+2" << std::endl;
    std::cout << it->first << ": " << it->second << std::endl;

    int dist = std::distance(it, M.end());  // std::advance == - (between iterator)
    std::cout << "-----------------" << std::endl;
    std::cout << "M.end() - it" << std::endl;
    std::cout << dist << std::endl;

    return 0;
}
```
실행 결과는 다음과 같다.

![](https://velog.velcdn.com/images/gsgh3016/post/ce8c3d34-9134-4a1e-930d-ace298fdd04d/image.png)
 
```std::next(it)```
```it++```와 같은 역할이다.
argument를 참조하지 않고, iterator를 return한다.
<br>
```std::prev(it)``` :
```it--```와 같은 역할이다.
argument를 참조하지 않고, iterator를 return한다.
<br>
```std::advance(it, n)``` :
```+```와 같은 역할이다.
iterator와 상수간 연산을 지원한다.
argument를 참조하고, void를 return한다.
<br>
```std::distance(it1, it2)``` :
```-```와 같은 역할이다.
iterator간 연산을 지원한다.
argument를 참조하지 않고, int를 return한다.
<br>

***
stream에 관련된 내용은 다음에 포스팅할 예정이다.