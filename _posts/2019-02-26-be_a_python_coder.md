---
layout: post
title: "[파이썬답게 코딩하기] 1장. 파이썬 개념"
tag: Python, 파이썬, Scpoe, Variable
description: "파이썬 개념 다지기"
categories: [Python Programming]

---

# 1장 개념 다지기
---

## [변수]

파이썬에서는 `=` 연산자를 통해 변수를 선언하고 동시에 값을 할당한다. 즉, 파이썬에서는 명시적으로 새로운 변수를 선언하기 위한 기능이 없다. 이 때문에 변수의 유효 범위를 결정하기 위한 파이썬만의 기능이 만들어졌다(`global`, `nonlocal`).

### 유효 범위 (Scope)

파이썬에서 변수의 유효 범위를 계산할 때 네임스페이스(**namespace**)를 기반으로 계산한다. 즉, 코드상에 사용된 변수가 네임스페이스 상에 있는지 확인하며 없을 경우 'NameError Exception'이 발생한다.
파이썬의 네임스페이스는 `built-in`, `global`, `enclosed`, `local`로 구성되어 있다.

 - *built-in* : 파이썬에 내장된 네임스페이스로 파이썬으로 작성된 코드 어디서나 사용이 가능
 - *global* : 파일 단위의 모듈 안에서 유효. 특정 모듈을 import 하게되면 해당 모듈의 global 변수도 사용이 가능
 - *enclosed* : 함수 안에 함수가 있는 구조. 즉, 내부 함수가 외부 함수의 변수에 접근할 수 있는 것을 의미.
 - *local* : 클래스나 함수의 내부로 한정.

변수를 확인할 때 **LEGB** 규칙에 따라 네임스페이스를 계산한다. 제일 먼저 Local 네임스페이스를 확인하고, 다음으로 Enclosed, Global, Built-in 네임스페이스를 순차적으로 검색.



```python
msg = "Hello"

def read_work():
    print(msg)
    print("World")

def read_exception():
    print(msg)
    msg = "World"
    print(msg)

def main():
    print("=== first read ===")
    read_work()

    print("=== second read ===")
    read_exception()

if __name__ == "__main__":
    main()
```

    === first read ===
    Hello
    World
    === second read ===

    ---------------------------------------------------------------------

    UnboundLocalError              Traceback (most recent call last)

    <ipython-input-2-59410e66baa9> in <module>
         18
         19 if __name__ == "__main__":
    ---> 20     main()


    <ipython-input-2-59410e66baa9> in main()
         15
         16     print("=== second read ===")
    ---> 17     read_exception()
         18
         19 if __name__ == "__main__":


    <ipython-input-2-59410e66baa9> in read_exception()
          6
          7 def read_exception():
    ----> 8     print(msg)
          9     msg = "World"
         10     print(msg)


    UnboundLocalError: local variable 'msg' referenced before assignment

---

오류가 난 원인은 msg가 선언 전에 사용되었기 때문이며 이는 한마디로 msg를 전역변수가 아닌 지역변수로 판단한 것이다.<br>
그 이유는 변수의 유효 범위를 계산하는 LEGB 방식에 있으며 local 네임스페이스부터 확인하기 때문에 'read_exception'함수에서 **local 네임스페이스를 확인할 때, msg라는 변수가 선언되어 있기 때문에 전역변수가 아닌 지역변수로 생각한 것이다.**

---


```python
msg = "Hello"

def write():
    msg = "World"
    print(msg)

def main():
    print("=== print msg ===")
    print(msg)

    print("=== write function ===")
    write()

    print("=== print msg ===")
    print (msg)

if __name__ == "__main__":
    main()
```

    === print msg ===
    Hello
    === write function ===
    World
    === print msg ===
    Hello

---
위 코드에서 우리는 msg라는 전역변수 값을 수정하려고 했으나 실제 결과는 전역변수의 내용이 변경되지 않았다.<br>
write 함수에서 msg 변수에 값을 할당하는 것은 앞에서처럼 새롭게 지역변수 msg를 선언하고 값을 할당하는 것으로 인식하기 때문에 global 변수의 값이 변경되지 않는다.

`따라서 global 변수 값을 변경하기 위해서는(유효 범위를 다루기 위해서는) 전역변수를 사용하기 위한 선언을 해야한다.`

---


### global


```python
msg = "Hello"

def write():
    global msg
    msg += " World"
    print(msg)

def main():
    print("=== print msg ===")
    print(msg)

    print("=== write function ===")
    write()

    print("=== print msg ===")
    print (msg)

if __name__ == "__main__":
    main()
```

    === print msg ===
    Hello
    === write function ===
    Hello World
    === print msg ===
    Hello World



```python

```
