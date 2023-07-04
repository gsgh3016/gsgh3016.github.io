---
layout: single
title: (스터디 내용 정리) step09. 함수를 더 편리하게
categories: DeepLearning
---

# 1. 파이썬 함수로 이용하기
현재 코드는 클래스로 정의됐으므로 코드 모양이 직관적이지 않다.
```python
x = Variable(np.array(0.5))
f = Square()
y = f(x)
# y = Square()(x) => 클래스 인스턴스 호출 후 다른 인자로 x가 들어간다.
```
그래서 따로 함수를 선언한다.
```python
def square(x):
    return Square()(x)
```
<br>

***
# 2. backward 메소드 간소화
역전파 연산 시 출력 변수 y에는 grad 메소드에 None이 저장되어있다. 이전에는 이를 y.grad = np.array(1.0)으로 초기화해줘야 하지만, 이를 Variable.backward 메소드 함수에 추가한다.
```python
class Variable:
    ...
    def backward(self):
        if self.grad is None:
            self.grad = np.ones_like(self.data) # data와 같은 타입으로 만들어주기 위함.
```
<br>

***
# 3. np.ndarray만 취급하기
현재 Variable은 ndarray 인스턴스만 취급하도록 디자인됐다. 하지만 Variable(1.0) Variable(3) 같이 다른 형태의 인스턴스가 들어올 수 있다. 이를 보완하는 코드를 작성한다.
```python
class Varialbe:
    def __init__(self, data):
        if data is not None:
            if not isinstance(data, np.ndarray):
                raise TypeError('{} is not supported'.format(type(data)))
```

또한 넘파이는 0차원 ndarray, 즉 스칼라를 제곱하면 numpy.float64가 되버린다.  
<br>

***