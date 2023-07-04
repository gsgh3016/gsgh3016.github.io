---
layout: single
title: (스터디 내용 정리) step07. 역전파 자동화
categories: DeepLearning
---

# 1. 개요
이전 step 같은 코드는 모델이 달라질 때마다 직접 backward 메소드를 함수마다 호출해줘야 한다. 이는 매우 비효율적인 방법이다. 이 책에서는 이를 Define-by-Run으로 구현한다. Define-by-Run은 데이터를 클래스로 감싸고 계산 그래프를 위한 연결을 자동으로 만드는 방법이다. 이번 step에서는 변수를 담은 Variable 클래스에서 backward 메소드 함수를 구현해서 역전파 연산을 구현한다.

이제까지 함수에서 변수를 바라보는 관점인 Function.input, Function.output으로 코드를 짰다. 이는 함수에서 변수로 자동 접근할 수는 있지만, 변수에서 함수로 자동 접근할 수는 없다. 이를 보완하기 위해, 변수에 creator라는 메소드를 추가한다. 결국 모든 변수는 어떤 함수의 출력이라고 생각하는 셈이다. 처음 입력 변수는 None 함수의 출력이라고 생각한다.
![](/assets/images/backprop_5.png)

```python
class Variable:
    def __init__(self, data):
        self.data = data
        self.grad = None
        self.creator = None
        # 처음 변수가 생성되면 creator는 없다. 함수의 출력값이 아니기 때문이다.

    def set_creator(self, func):
        self.creator = func
        # Function 메소드에서 .creator 메소드에 함수를 저장하기 위한 메소드 함수이다.
```
```python
class Function:
    def __call__(self, input):
        x = input.data
        y = self.forward(x)
        output = Variable(y)
        output.set_creator(self)
        # 출력 변수를 Variable로 감쌌으므로, 호출된 자신을 출력 변수의 creator 메소드에 저장한다.
        # python은 C++과 다르게 self는 인스턴스 그 자체를 가리키므로 바로 메소드를 호출할 수 있다.

        self.input = input
        self.output = output
        # 출력 변수를 저장해야 creator 메소드와 맞물려서 작동한다.
        return output
```
![](/assets/images/backprop_6.png)
<br>

***
# 2. 구현
역전파 각 단계는 다음과 같이 세분할 수 있다.
1. 함수를 가져온다
2. 함수의 입력을 가져온다
3. 함수의 backward 메소드를 호출한다.
4. backward 메소드로 업데이트된 .grad로 1.을 반복한다.
<br>

***
# 3. Variable.backward 메소드
이런 반복식을 재귀로 구현한다.
```python
class Variable:
    ...
    def backward(self):
        f = self.creator                    # 1. 함수를 가져온다.
        if f is not None:                   # 생성자가 None => 입력 변수
            x = f.input                     # 2. 함수의 입력을 가져온다.
            x.grad = f.backward(self.grad)  # 3. 함수의 backward 메소드 호출 (입력의 grad 업데이트)
            x.backward()                    # 4. 1.로 돌아가 반복
```
<br>

***