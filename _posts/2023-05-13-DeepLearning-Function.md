---
layout: single
title: (스터디 내용 정리) step02. 상자로서의 변수
categories: DeepLearning
---

지난 step에서 변수를 ```Variable``` 클래스로 간단하게 구현했다.
이번 step에는 함수를 ```Function``` 클래스로 구현한다.
<br>

***
# 1. 함수란
함수의 수학적 정의도 좋지만, 좀 더 직관으로 바라보자.
함수는 입/출력의 관계이다.
함수 내부에 자체적인 연산을 포함해서 그림으로 표현하면 다음과 같다.
![](https://velog.velcdn.com/images/gsgh3016/post/2ae555ca-3897-411a-b728-f66b88d6dbda/image.png)

<br>

***
# 2. ```Function``` 클래스 구현
```python
class Function:
    def __call__(self, input):
        x = input.data
        y = self.forward(x)
        output = Variable(y)
        return output
```
```y = self.forward(x)```는 함수의 연산을 고정하지 않고 상속을 통해 구체화 하기 위한 방법이다.

```__call__``` 문법이 눈에 띈다.
```__init__```과 다르게 ```__call__```은 호출이 일어나면 동작이 일어난다.
더 쉬운 차이로 다음과 같다.
```python
x = Variable(np.array(10))
f = Square()				# Square: Function에서 상속받은 자식 클래스
y = f(x)
```

```x```의 초기화 및 선언으로 ```Variable``` 객체(인스턴스)가 ```x```에 저장되었다.
하지만 ```f```는 ```Square```의 동작이 선언되었다.
그 결과 ```y = f(x)```에서 ```x```가 ```f``` 동작을 따라 ```y```에 저장됐다.

정리하면,
<ul>
  <li>Function 클래스는 Variable 인스턴스를 입력받아 Variable 인스턴스를 출력.
  <li>Variable 인스턴스의 실제 데이터는 내부 메소드 변수인 data에 저장.
</ul>

이런 ```__call__``` 메소드는 Define-by-Run을 가능하게 하는 요소이다.
Define-by-Run은 추후에 자세히 설명할 예정이다.
<br>

***
# 3. ```Function``` 클래스 이용
![](https://velog.velcdn.com/images/gsgh3016/post/14e5909f-de77-49ff-bf13-6522f3b379b6/image.png)
<ul>
  <li> Function``` 클래스는 공통 기능을 구현한 기반 클래스.
  <li> 구체적인 연산은 자식 클래스에서 구현. 
</ul>

이를 위해서 위 ```Function``` 클래스 구현 코드를 살짝 수정하자.
```python
class Function:
    def __call__(self, input):
        x = input.data
        y = self.forward(x)
        output = Variable(y)
        return output
    # 추가한 부분
    def forward(self, x):
        raise NotImplementedError()
```
이는 ```Function```을 상속받은 자식 클래스가, 즉 구체적인 함수가 ```forward```연산을 구현하지 않으면 에러를 발생하게 하는 부분이다.

실제 상속은 다음과 같이 이루어진다.
```python
class Square(Function):
    def forward(self, x):
        return x**2
```

<br>

***
이번 step에서 ```__call__``` 메소드 활용을 알게 됐다.
파이썬에 이런 기본 메소드를 잘 활용하면 효율적으로 동작하는 코드를 구현할 수 있겠다.

[Github](https://github.com/gsgh3016/Deep-Learning-from-Scratch3/blob/main/gamchan/part_1/step02.py)