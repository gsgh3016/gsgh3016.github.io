---
layout: single
title: (스터디 내용 정리) step01. 상자로서의 변수
categories: DeepLearning
---

이 책에서 구현하는 코드는 객체 지향으로 특히 캡슐화(encapsulation)와 다형성(Polymorphism)에 무게를 두고 있다.

그중 캡슐화에 첫 단계인 1고지는 ```Variable```과 ```Function``` 클래스 구현이 중심이다.
```Variable```은 ```Function```과 상호작용하며 서로를 호출하며 역전파 연산을 수행한다.

step01에서는 그중 ```Variable```을 기초적으로 구현한다.
<br>

***

# 1. 변수란
![](https://velog.velcdn.com/images/gsgh3016/post/ab7ba382-ca56-4be3-9579-4bc840fb874b/image.png)
수학 문제 풀 때 생각하는 변수는 대표성(Symbolic)을 띈다.
미지수를 표현할 때 ${x}$라고 쓰는데, ${x}$에는 어떤 데이터/값이 들어가 있다.
이를 실제 데이터로 표현하면 다른 상수와 혼돈이 생기기 때문에 미지수를 사용하는 것이다.

이를 정리하면
<ul>
  <li>변수와 데이터는 별개이다.(클래스와 메소드의 차이)
  <li>변수에는 데이터가 들어간다.
  <li>데이터는 변수를 통해 참조한다.
</ul>
<br>

***

# 2. ```Variable``` 클래스 구현
위 성질로 ```Variable```과 ```data```를 구현하면 다음과 같은 코드이다.

```python
class Variable:
    def __init__(self, data):
        self.data=data
```
```Variable```을 생성하면 데이터는 어떤 식으로도 들어간다.(```None```이라도)

```python
data = np.array(1.0)
x = Variable(data)
print(x.data)
```
```x```의 데이터는 ```x.data```로 참조한다.
```np.array```로 스칼라 데이터를 넣는 이유는 향후 벡터와 텐서 계산에도 활용하기 위함이다.

딥러닝 프레임워크는 벡터와 텐서 계산이 중점으로 이루어지기 때문이다.
<br>

***

[Github](https://github.com/gsgh3016/Deep-Learning-from-Scratch3/blob/main/gamchan/part_1/step01.py)