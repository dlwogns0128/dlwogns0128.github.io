---
layout: post
title: "[Algorithm, LeetCode] 991. Broken Calculator"
tag: [Algorithm, LeetCode]
date: 2019-02-16
description: "곱하기 2와 빼기 1 연산만 사용할 수 있는 고장난 계산기를 이용하여 X에서 Y로 갈 수 있는지 계산을 해보자"
categories: Algorithm

---

Acceptance | Difficulty | Like/Dislike
---|:---:|---:
`40.1` | `Medium` | `128/51`

<br>
On a broken calculator that has a number showing on its display, we can perform two operations:

-   **Double**: Multiply the number on the display by 2, or;
-   **Decrement**: Subtract 1 from the number on the display.

Initially, the calculator is displaying the number  `X`.  
Return the minimum number of operations needed to display the number  `Y`.

`X`에서 `Y`로 갈 수 있는 최소의 연산 수를 계산하자  
연산은 곱하기 2(**Double**)와 빼기 1(**Decrement**)만 존재한다

**Example1**
```
Input: X = 2, Y = 3
Output: 2
Explanation: Use double operation and then decrement operation {2 -> 4 -> 3}.
```
**Example2**
```
Input: X = 3, Y = 10
Output: 3
Explanation: Use double, decrement and double {3 -> 6 -> 5 -> 10}.
```
**Example3**
```
Input: X = 1024, Y = 1
Output: 1023
Explanation: Use decrement operations 1023 times.
```

***Note***
 >  1<=X<=10^9   
 >  1<=Y<=10^9


## 풀이
---
 - 처음에는 DP(Dynamic Programming)을 생각해 보았다. 하지만 무한 탐색에 빠지는 오류가 나는 것을 확인하였다.

 - 그러다 `Y`에서 `X`로 반대로 가는 것을 생각해봄

 - 그러면 연산 방법은 `나누기 2`(**Half**)와 `더하기 1`(**Increment**)로 바뀌게 되며 나누기의 경우 1로 수렴되어 탐색의 범위가 만들어지게 된다!

 - 하지만 `Y`에서 나누기와 더하기를 어떻게 조합하는지 생각하다가 못품..

## 해설
---
 우선 이 문제에서 핵심은 `X`를 이용해서 Double 연산과 Decrement 연산을 이용하는 것이 아니라 그 반대인 `Y`를 이용하여 Half 연산과 Increment 연산을 이용하는 것이다  
그렇게 될 경우 `log(n)`으로 탐색을 수행할 수 있게된다. 여기서 `Y`에 연산을 적용시킬 수 있는 경우의 수를 생각해보자

- `Y`가 `X`보다 작은 경우 `[Y<=X]`

  - 최소한의 경우를 생각해 볼 때 **Increment** 연산밖에 사용할 수 없다

- `Y`가 `X`보다 큰 경우 `[Y>X]`

  1. `Y`가 홀수일 때
    - 사용할 수 있는 연산은 **Increment** 연산 뿐이다.
  2. `Y`가 짝수일 때
    - 사용할 수 있는 연산은 **Half**, **Increment** 연산 두 가지다
    - 여기서 Increment 연산을 하게되면 `Y+1`이 되고 이는 홀수이므로 또 Increment 연산을 사용할 수 밖에 없어 2번의 연산 과정을 거친 다음 `Y+2`가 되어야 한다
    - `Y+2`에서 Half 연산을 사용하게 되면 `Y/2+1`가 되고 여기까지 총 **3**번의 연산을 수행하였다
    - **하지만** `Y`에서 Half 연산을 하게 되면 `Y/2`가 되고 추가적으로 1번의 Increment 연산을 하여 `Y/2+1`를 **2** 번의 연산 횟수로 도달할 수 있다
    - 즉, `Y/2+1`도 **2번의** 연산(Half + Increment)으로 가능하므로
    - <u>`Y`가 짝수일 때는 **Half** 연산을 이용하는 것이  합리적이다 </u>


``` c++
class Solution {
public:

    int brokenCalc(int X, int Y) {
        int res = 0;
        if(Y<=X)
            return res += X-Y;
        while(Y>X)
        {
            if(Y%2==1)
                Y +=1;
            else
                Y /=2;
            res +=1;
        }
        return res += X-Y;
    }
};
```


## 참고
---
>LeetCode's Solution
