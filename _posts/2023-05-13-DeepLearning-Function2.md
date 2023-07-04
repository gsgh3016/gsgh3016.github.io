---
layout: single
title: (스터디 내용 정리) step03. 함수 연결
categories: DeepLearning
---

지난 step에서 함수를 나타내는 ```Function``` 클래스와 이를 상속받아 실제 연산하는 ```Square``` 클래스를 구현했다.
이번 step에서는 또 다른 실제 연산 클래스를 구현하고 두 클래스간의 연결을 수행한다.
<br>

***
# 1. Exp 함수 구현
${y} = {e}^{x}$를 ```Exp``` 클래스로 구현하자.

```python
class Exp(Function):
    def forward(self, x):
        return np.exp(x)
```

여기서 ```x```는 ```Variable``` 인스턴스임을 기억하자.
<br>

***
# 2. 함수 연결
만약 다음과 같은 연산을 수행한다면 어떤 식으로 코드를 짤까?

${y} = {({e}^{({x}^2)})}^2$

이렇게 코드를 짤 것이다.
```python
A = Square()
B = Exp()
C = Square()

x = Variable(np.array(0.5))
a = A(x)	# 1
b = B(a)	# 2
y = C(b)	# 3
print(y.data)
```
![](https://velog.velcdn.com/images/gsgh3016/post/30e86476-7bab-48f1-8d32-b4ad3452cd1b/image.png)

<br>

***
[Github](https://github.com/gsgh3016/Deep-Learning-from-Scratch3/blob/main/gamchan/part_1/step03.py)