---
layout: single
title: (스터디 내용 정리) step08. 재귀에서 반복문으로
categories: DeepLearning
---

# 1. 반복문을 이용한 구현
Variable.backward 메소드를 DFS를 기반으로 수정한다.
- 수정 전
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
- 수정 후
```python
class Variable:
    ...
    def backward(self):
        funcs = [self.creator]
        while funcs:                    # 4. 반복한다.(funcs가 빈 리스트가 될 때까지)
            f = funcs.pop()             # 1. 함수를 가져온다.
            x, y = f.input, f.output    # 2. 함수의 입출력을 가져온다.
            x.grad = f.backward(y.grad) # 3. backward 메소드를 호출한다.
            if x.creator is not None:
                funcs.append(x.creator) # x가 입력 변수가 아니면 x의 생성자를 추가한다.ㄴ
```
<br>

***
# 2. 재귀 vs 반복문 vs 꼬리 재귀
<table border="1">
    <thead>
        <tr>
            <th></th>
            <th>Recursion</th>
            <th>Looping</th>
            <th>Tail Recursion</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Definition</td>
            <td>A method where the solution to a problem depends on solutions to smaller instances of the same problem.</td>
            <td>A programming construct that repeatedly executes a block of code as long as a certain condition is met.</td>
            <td>A special kind of recursion where the last operation of the function is the recursive call.</td>
        </tr>
        <tr>
            <td>Space Complexity</td>
            <td>Can be high, because each recursive call requires extra space on the call stack.</td>
            <td>Usually lower than recursion, because it only requires a constant amount of space.</td>
            <td>Similar to loops, can be optimized by compilers or interpreters to use a constant amount of space.</td>
        </tr>
        <tr>
            <td>Use Case</td>
            <td>Effective for problems that can naturally be divided into smaller similar problems (e.g., tree and graph traversals).</td>
            <td>Good for simple repetitive tasks that don't require changing states (e.g., simple iterations over arrays).</td>
            <td>Effective in functional programming languages and problems that involve state that needs to be carried forward.</td>
        </tr>
    </tbody>
</table>

일반 재귀와 꼬리 재귀를 코드 상 비교하면
일반 재귀:
```python
def factorial_recursive(n):
    # Base case: factorial of 0 is 1
    if n == 0:
        return 1
    else:
        # Recursive case: n! = n * (n-1)!
        return n * factorial_recursive(n-1)

print(factorial_recursive(5))  # Output: 120
```

꼬리 재귀
```python
def factorial_tail_recursive(n, accumulator=1):
    # Base case: factorial of 0 is 1
    if n == 0:
        return accumulator
    else:
        # Tail Recursive case: n! = n * (n-1)!
        return factorial_tail_recursive(n-1, n * accumulator)

print(factorial_tail_recursive(5))  # Output: 120
```

꼬리 재귀와 재귀 함수의 차이점은 재귀 함수는 연산 이후 return을 주는 반면, 꼬리 재귀는 연산 없이 바로 return이 이루어진다. 따라서 꼬리 재귀는 스텍 메모리 사용이 단순하다.
<br>

***
# 3. 동작 확인
이후 계산 그래프는 계산 순서를 확인할 때 연산에 따라 달라진다. 연산의 우선순위에 따라 먼저 행해져야 하는 연산이 다르기 때문이다. 위 책의 DFS 방식은 계산 그래프에서 재귀 방식보다 편리하다.
<br>

***