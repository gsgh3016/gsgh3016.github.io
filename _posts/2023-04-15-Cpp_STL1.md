---
layout: single
title: 표준 템플릿 라이브러리(STL)(1) std::vector
categories: C++
---

C에서 가장 골치 아픈게 동적 할당이다.

작년 C언어 과목에서 학기 1/3이 동적 할당 관련 내용이었다.

나는 C를 배우고 들었지만, 다른 사람들은 많이 힘들어보였다.

처음 배울 때엔 이해하지 않고 외워서 썼던 기억이 난다.

C++가 좋은 이유 중 하나가 바로 이런 동적 할당에서 비교적 자유롭기 때문이다.

<br>
<br>

***
# 1. 컨테이너 공통 연산
```std::vector```를 예시로 든다.


***
## 1-1 생성
```cpp
std::vector<int> vec1;			// 기본 빈 벡터(컨테이너) 선언
std::vector<int> vec2(vec1);	// vec1 벡터(컨테이너)를 vec2를 선언하며 복사
std::vector<int> vec4{1, 2, 3};	// vec4 벡터(컨테이너)를 {1, 2, 3}으로 초기화
```


***
## 1-2 지우기
```cpp
vec1.clear();	// 원소 모두 삭제(메모리는 남아있음)
```

***
## 1-3 크기
```cpp
vec1.empty();	// 비어있으면 True, 원소가 있으면 False를 반환
				// .clear후 .empty를 하면 True 반환
				// 메모리와 상관없음
                
vec1.size();	// size_t type 반환
```
***
## 1-4 접근
```cpp
vec1.begin(), vec1.end();	// iterator 접근
							// 처음, 끝(마지막 원소보다 하나 뒤),
							// 참조(&) 역할 일부 수행
                            
vec1.cbegin(), vec1.cend();	// 상수 반복자
							// 값을 참조하지는 못함(변경 X)
                            
vec1.rbegin(), vec1.rend();	// 마지막 원소, 처음보다 하나 앞
```
***
## 1-5 원소할당 이동
```cpp
vec1 = vec2;			// vec2 값을 vec1에 할당
vec1 = std::move(vec2);	// vec2 값을 vec1로 이동, 이때 vec2는 clear 상태
vec1 = {10, 20, 30};	// vec1을 {10, 20, 30}로 초기화
```

<br>
<br>


***
# 2. vector
동적 배열을 캡슐화했다.

얘 때문에 C보다는 한결 편해졌다.

```.push_back``` 등으로 추가 원소를 저장할 수 있어 크기보다 더 큰 공간을 차지한다.
***
## 2-1 ```.size() vs .capacity()```
벡터에서 동적 할당을 덜 신경써도 되는 이유이다.

쉽게 설명하자면 다음과 같다.
> ```.size()``` : 벡터에 저장된 데이터(element) 개수
> ```.capacity()``` : 벡터에 할당된 메모리 크기. -> 동적 할당과 연관이 깊다.

```.size()```와  ```.capacity()```는 서로 연관될 수 밖에 없는데, 메모리 관리를 해야하는 경우라면 이를 꼼꼼히 알아두어야 한다.

![](https://velog.velcdn.com/images/gsgh3016/post/cfe24771-71a6-4941-8ebe-0070759b9c0f/image.jpg)

***
## 2-2 ```.push_back()```
```cpp
std::vector<int> num{3, 2, 5, 4, 7};	// num = {3, 2, 5, 4, 7}
num.push_back(8);						// num = {3, 2, 5, 4, 7, 8}
```
※ ```.pop_back()```과 같이 사용하면 ```vector```를 stack처럼 사용할 수 있다.
```cpp
#include <iostream>
#include <vector>

int main(){
    std::vector<int> num{3, 2, 5, 4, 7};
    std::cout << "*******************" <<std::endl; 
    std::cout << "before push_back" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl << std::endl;

    num.push_back(8);

    std::cout << "*******************" <<std::endl; 
    std::cout << "after push_back" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl;
    
    std::cout << "*******************" <<std::endl; 
    
    return 0;
}
```

![](https://velog.velcdn.com/images/gsgh3016/post/0fdf40c7-0c5b-417d-9454-ebf7a7ffe17b/image.png)

```num size```, ```num capacity```의 변화를 주목해서 살펴보자.
<br>

***
## 2-3 ```.insert()```
다음 초기화된 벡터를 기준으로 보자
```cpp
std::vector<int> num{3, 2, 5, 4, 7};
```
```.insert()```는 2개 혹은 3개의 argument를 넣을 수 있다.

<br>
<br>

```cpp
num.insert(num.begin()+3, 1);
```
argument가 2개인 경우는 특정 위치에 값을 하나 넣을 때 사용한다.

앞 argument에는 원소를 삽입할 위치(iterator로 표현)가 들어간다. 
위 예시로는 ```num.begin()+3``` 혹은 3번째 index 위치에 들어간다는 뜻이다.

그 뒤에는 삽입할 데이터가 들어간다.
위 예시로는 ```1```이 3번 index로 들어간다는 뜻이다.

```cpp
#include <iostream>
#include <vector>

int main(){
    std::vector<int> num{3, 2, 5, 4, 7};
    std::cout << "*******************" <<std::endl; 
    std::cout << "before insert" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl << std::endl;

    num.insert(num.begin()+3, 1);

    std::cout << "*******************" <<std::endl; 
    std::cout << "after insert" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl;
    
    std::cout << "*******************" <<std::endl; 
    
    return 0;
}
```

![](https://velog.velcdn.com/images/gsgh3016/post/8b47faf9-54ca-47ad-abaa-f75981bd3f6d/image.png)


```num size```, ```num capacity```의 변화를 주목해서 살펴보자.
<br>
<br>


```cpp
num.insert(num.begin()+3, 2, 1);
```
argument가 3개인 경우는 특정 위치에 같은 값 여러 개를 넣을때 사용한다.

맨 앞에는 원소를 삽입할 위치(iterator로 표현)가 들어간다.
위 예시로는 ```num.begin()+3``` 혹은 3번째 index 위치에 들어간다는 뜻이다.

중간에는 삽입할 데이터 개수가 들어간다.
위 예시로는 같은 데이터 2개가 3번 index부터 차례로 들어간다는 뜻이다.

맨 뒤에는 삽입할 데이터가 들어간다.
위 예시로는 ```1``` 2개가 3번 index부터 차례로 들어간다는 뜻이다.

```cpp
#include <iostream>
#include <vector>

int main(){
    std::vector<int> num{3, 2, 5, 4, 7};
    std::cout << "*******************" <<std::endl; 
    std::cout << "before insert" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl << std::endl;

    num.insert(num.begin()+3, 2, 1);

    std::cout << "*******************" <<std::endl; 
    std::cout << "after insert" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl;
    
    std::cout << "*******************" <<std::endl; 
    
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/78694e9f-c840-4517-9ae2-24a712d69407/image.png)

```num size```, ```num capacity```의 변화를 주목해서 살펴보자.
<br>

***

## 2-4 ```.erase()```
```.erase()```는 1개 혹은 2개의 argument를 넣을 수 있다.

```cpp
num.insert(num.begin()+3);
```
argument가 1개인 경우는 특정 위치 값을 없앨때 사용한다.
위 예시로는 ```num.begin()+3``` 혹은 3번 index 원소를 없앤다는 뜻이다.

```cpp
#include <iostream>
#include <vector>

int main(){
    std::vector<int> num{3, 2, 5, 4, 7};
    std::cout << "*******************" <<std::endl; 
    std::cout << "before erase" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl << std::endl;

    num.erase(num.begin()+3);

    std::cout << "*******************" <<std::endl; 
    std::cout << "after erase" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl;
    
    std::cout << "*******************" <<std::endl; 
    
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/14af9f95-cce2-4cb5-b20e-c936b466dad2/image.png)

```num size```, ```num capacity```의 변화를 주목해서 살펴보자.
<br>
<br>


```cpp
num.erase(num.begin()+3, num.begin()+5);

// num.begin()+5 대신 num.end() 가능
```
argument가 2개인 경우는 없앨 원소의 시작 위치와 끝 위치가 들어간다.

앞에는 원소를 없애기 시작할 위치가 들어간다.
위 예시로는 ```num.begin()+3``` 혹은 3번 index부터 없앤다는 뜻이다.

뒤에는 없애는 동작을 멈출 위치가 들어간다.
위 예시로는 ```num.begin()+5``` 혹은 5번 index에서 erase 동작을 멈춘다는 뜻이다.

※ 주의해야 할 점이 멈출 위치에서는 erase가 이루어지지 않는다는 뜻이다.
위 예시에서 ```num.begin()+5```는 ```num.end()```와 같은데, ```num```은 4번 index까지 밖에 없다.
마지막 iterator인 ```num.end()``` 혹은 ```num.begin()+5```는 벡터 원소를 가르키지 않는다. 

```cpp
#include <iostream>
#include <vector>

int main(){
    std::vector<int> num{3, 2, 5, 4, 7};
    std::cout << "*******************" <<std::endl; 
    std::cout << "before erase" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl << std::endl;

    num.erase(num.begin()+3, num.begin()+5);

    std::cout << "*******************" <<std::endl; 
    std::cout << "after erase" <<std::endl; 
    for(auto it: num){
        std::cout << it << ' ';
    }
    std::cout << std::endl << "num size: " << num.size() << std::endl;
    std::cout << "num capacity: " << num.capacity() << std::endl;
    
    std::cout << "*******************" <<std::endl; 
    
    return 0;
}
```
![](https://velog.velcdn.com/images/gsgh3016/post/6e956a20-5cf1-49c6-96d9-b96e43cdbde3/image.png)

```num size```, ```num capacity```의 변화를 주목해서 살펴보자.
<br>
<br>

***
이렇게까지 길어질 줄 몰랐다.

가장 중요한 ```vector```와 ```map```은 각각 포스팅 하나로 정리해야겠다.