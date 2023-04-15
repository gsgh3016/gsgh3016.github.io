---
layout: single
title: Hello World!
categories: C++
---


개강한지 한 달이 지났지만, 이제라도 지금까지 배운 내용을 정리한다.

C++프로그래밍과 실습이라는 과목인데, 진행방식이 다른 수업과는 달랐다.

개념 강의는 전부 녹화 강의로 대체했고, 수업 시간에는 실습으로 진행한다.

특히 실습 중에 실력 밸런스를 맞춰서 짝을 짓고 서로 질문하며 실습을 진행하는데, 학생 입장에서 가르칠 기회는 많이 없기에 좋은 경험이라고 생각한다.

***
## Hello World!
### 스트림(stream)
```cpp
#include <iostream>

int main(){
	std::cout << "Hello World!" << std::endl;
	return 0;
}
```

C였다면 ```stdio.h```를 포함했다.

또한 ```<<``` 연산자와 ```::``` 연산자를 볼 수 있다.

C++의 중요한 특징은 스트림(stream)이다.

![](https://slidesplayer.org/slide/16554349/96/images/3/C%2B%2B+%EC%9E%85%EC%B6%9C%EB%A0%A5+%EC%8A%A4%ED%8A%B8%EB%A6%BC.jpg)

C에서 ```<<``` 연산자는 비트 연산자였다.

C++에서도 ```<<``` 연산자는 비트 연산으로 사용하지만, 입출력 연산으로 더욱 많이 사용한다.

C++는 객체 지향 프로그래밍(OOP) 언어이기에 오버로딩을 지원한다. (자세한 이야기는 이후 포스팅에서 계속한다.)

사용자 입력이 비트 흐름(istream, input stream)으로 C++ 프로그램에 들어가고, 프로그램 결과가 다시 비트 흐름(ostream, output stream)으로 콘솔 창에 나타난다.

그런 이유로 ```<<``` 연산자를 오버로딩하지 않을까 싶다.
### Namespace
또 다른 Hello World 예제로 다음과 같이 쓸 수 있다.

```cpp
#include <iostream>

using namespace std;

int main(){
	cout << "Hello World!" << endl;
	return 0;
}
```
```std::``` 연산을 ```using namespace std;```로 대체했다.

```namespace```를 조금 쉽게 풀자면, 변수나 클래스, 함수 등이 선언된 범위라고 볼 수 있다.

```using namespace std```는 ```std``` 범위(scope)에 선언된 이것 저것 가져다가 쓰겠다는 의미인 셈이다.

하지만, ```using namespace std```는 사용하지 않는 편이 좋다.

namespace간 충돌이 일어날 수 있기 때문이다.

더 자세한 내용은 추후에 포스팅하려고 한다.

***
## 입출력 포맷팅(formating)
소수 연산을 하면, 기본 값으로 6자리가 출력 된다.
```cpp
#include <iostream>

int main(){
    float a = 1.0, b = 3.0;
    std::cout << a/b << std::endl;
    return 0;
}
```
```
0.333333
```
C언어는 ```printf("%.nf", f);```로 소수점 자리수 출력을 조절한다.

반면, C++에서는 어떻게 출력할까?

```cpp
#include <iostream>
#include <iomanip>

int main(){
    float a = 1.0, b = 3.0;

    std::cout << std::fixed << std::setprecision(3) << a/b << std::endl;
    return 0;
}
```
```
0.333
```

```std::fixed```, ```std::setprecision()```을 사용하려면, ```iomanip```을 include해야한다.

아마 io manipulator의 앞 5글자 같다.

```std::fixed```는 출력 스트림 버퍼를 고정하고, ```std::setprecision```은 고정된 스트림 버퍼에서 소수점 자리를 얼마나 표현할 지 정한다.

