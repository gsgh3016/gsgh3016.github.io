---
layout: single
title: (스터디 내용 정리) step06. 수동 역전파
categories: DeepLearning
---

# 1. Variable 클래스 추가 구현
기존에 Variable 클래스는 data만 가지고 있었다. 역전파를 구현하기 위해 grad 인스턴스를 추가한다.
```python
class Variable:
    def __init__(self, data):
        self.data = data
        self.grad = None
```
grad의 초기값을 None으로 설정했다. 역전파 연산이 시작되면 함수를 거슬러올라서 grad에 연산 결과를 채워넣는다.
<br>

***
# 2. Function 클래스 추가 구현
이전까지 구현했던 Function은 일반적인 계산(forward)만 지원한다. 이에 역전파 수행 메소드(backward), Function 호출시 input 저장 메소드(self.input)를 추가한다.
```python
class Function:
    def __call__(self, input):
        x = input.data
        y = self.forward(x)
        output = Variable(y)
        self.input = input
        return output

    def forward(self, x):
        raise NotImplementedError("Function forward not implemented")

    def backward(self, gy):
        raise NotImplementedError("Function backward not implemented")
```
![](/assets/images/backprop_2.png)
<br>

***
# 3. Square, Exp 클래스 추가 구현
Square, Exp를 구현하며 Function의 동작을 알아보자.
```python
class Square(Function):
    # Function 상속 -> .forward(), .backward() 구현 필요
    # self.input 사용 가능

    def forward(self, x):
        # 제곱 연산 수행
        y = x ** 2
        return y

    def backward(self, gy):
        # gy: 이전 역전파 과정까지 f'값들의 곱
        x = self.input.data
        gx = 2 * x * gy
        return gx

class Exp(Function):
    def forward(self, x):
        y = np.exp(x)
        return y

    def backward(self, gy):
        x = self.input.data
        gx = np.exp(x) * gy
        return gx
```
<br>

***
# 4. 역전파 구현
순전파(forward)는 다음과 같은 코드로 나타낼 수 있다.
```python
A = Square()
B = Exp()
C = Square()

x = Variable(np.array(0.5))
a = A(x)
b = B(a)
y = C(b)
```
forward 메소드가 __call__에 호출되므로 별도의 메소드 호출 없이 순전파 연산이 이루어진다.
![](/assets/images/backprop_3.png)

역전파는 다음과 같이 이루어진다.
```python
y.grad = np.array(1.0)
b.grad = C.backward(y.grad)
a.grad = B.backward(b.grad)
x.grad = A.backward(a.grad)
```
처음에 y.grad를 np.array(1.0)으로 초기화해야 한다. 초기화 이전 y.grad의 값은 None이고 이를 backward 연산으로 넣으면 연산이 이루어지지 않기 때문이다. 위 코드를 그래프로 나타내면 다음과 같다.
![](/assets/images/backprop_4.png)

<br>

***
