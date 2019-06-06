---
layout: post
title: "[Algorithm, LeetCode] 861. Transpose Matrix"
tag: [Algorithm, LeetCode, Python]
date: 2019-06-06
description: "MxN Matrix가 배열로 주어졌을 때 이를 Transpose하는 LeetCode 문제를 풀어보고, 이러한 상황에서 유용한 python의 기법을 알아보자"
categories: [Algorithm, Python]

---

Acceptance | Difficulty | Like/Dislike
---|:---:|---:
`64.0` | `Easy` | `199/216`

<br>
Given a matrix `A`, return the transpose of `A`.  

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.  
<br>
`A` 행렬이 입력으로 들어오면 그의 전치행렬을 반환하자.  
여기서 전치행렬 (transpose of a matrix)이란 원래 행렬에서 행과 열의 원소가 서로 바뀐 행렬을 의미한다.  

**Example1**  
```bash
Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: [[1,4,7],[2,5,8],[3,6,9]]
```

**Example2**
```bash
Input: [[1,2,3],[4,5,6]]
Output: [[1,4],[2,5],[3,6]]
```
***Note***
 >  1 <= A.length <= 1000  
 >  1 <= A[0].length <= 1000

<br>
## 풀이  
---

  - MxN 행렬이 있을 때 각 열에서 0~N에 해당하는 각 행의 원소들을 모아서 새로운 행렬을 만들어주면 바로 전치행렬이 된다

  - 모든 행과 열에 해당하는 원소들에 다 접근해야 하므로 `O(MN)`이 걸리며 이 문제는 시간적으로 더 효율적인 알고리즘을 이용하는게 포인트가 아님!

  - `zip`은 python의 내장 함수로써 동일한 개수로 이루어진 자료형을 묶어주는 역할을 한다. 입력으로 들어온 값들을 각각 처음 값부터 끝 값까지 묶어서 tuple로 만들어준다.
  - `zip`함수의 반환 값은 object이며 list로 변환을 해서 표시한다

```python
>>> zip([1,2],[3,4])
<zip object at 0x03861490>
>>> temp = list(zip([1,2],[3,4]))
>>> temp
[(1, 3), (2, 4)]
>>> type(temp[0])
<class 'tuple'>
```

  - 하지만 문제에서 Input은 2차원 배열로 입력이 들어오기 때문에 그대로 zip 함수의 인자로 넣게될 경우 1개의 값만 들어온 것으로 인식되므로 작동되지 않는다

```python
>>> input_ = [[1,2,3],[4,5,6],[7,8,9]]
>>> list(zip(input_))
[([1, 2, 3],), ([4, 5, 6],), ([7, 8, 9],)]
```

  - 그래서 이 문제를 해결하기 위해서는 zip 함수의 인자로 [1,2,3], [4,5,6], [7,8,9] 배열이 각각 들어와야 하며 이를 해결해주는 것이 `Asterisk(*)` 이다

  - python에서 `Asterisk(*)`은 단순히 곱셈의 의미 이상의 여러 연산을 가능케 한다. 그 중에서 `컨테이너 타입의 데이터를 Unpacking`하는 용도로 이 문제에서 사용을 할 수 있다

```python
>>> a = [[1,2,3],[4,5,6],[7,8,9]]
>>> print(*a)
[1, 2, 3] [4, 5, 6] [7, 8, 9]
>>> print(a)
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
---------------------------------------------------------------------
>>> def countValue(t):
	print(len(t))

>>> countValue(a)
3
>>> countValue(*a)
Traceback (most recent call last):
  File "<pyshell#29>", line 1, in <module>
    countValue(*a)
TypeError: countValue() takes 1 positional argument but 3 were given
# *a 를 인자로 넣게 될 경우 unpacking을 해서 들어가기 때문에
# [1,2,3], [4,5,6], [7,8,9] 배열이 각각 들어가지는 것을 확인할 수 있다.
```

  - 따라서 `zip` 함수와 `Asterisk(*)` 연산을 이용하여 문제를 쉽게 해결할 수 있다!
<br>

## 해답 코드 (허무함 주의, 너무 간단함 주의...)
---

```python
class Solution:
    def transpose(self, A: List[List[int]]) -> List[List[int]]:
        return [list(b) for b in zip(*A)]
```
