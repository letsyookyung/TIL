### 변수의 범위(scope)
global과 nonlocal 키워드에 대해서 이해하려면 먼저 변수의 범위(scope)에 대한 개념을 간단히 짚고 넘어가야할 것 같습니다.

비단 파이썬 뿐만 아니라 대부분의 프로그래밍 언어에서 변수의 범위라는 것은 해당 변수를 어디에서 선언하느냐에 따라서 결정이 됩니다. 아주 단순하게 두 구역으로 나누면 함수 외부를 전역(global/module) 범위라고 하고, 함수 내부를 지역(local/function) 범위라고 합니다. 또한 함수를 중첩했을 때 외부 함수와 내부 함수의 사이에서 생겨나는 비지역(nonlocal/enclosing) 범위라는 것도 있습니다.

### global / nonlocal / local
이 세 구역의 범위를 각 함수의 입장에서 간단하게 주석으로 나타내보면 다음과 같습니다.

- outer(), inner() 함수 입장에서 전역(global) 범위
    def outer():
        # outer() 함수 입장에서 지역(local) 범위
        # inner() 함수 입장에서 비지역(nonlocal) 범위
        def inner():
            # inner 함수 입장에서 지역(local) 범위

---         
    global_var = "전역 변수"

    def outer():
        nonlocal_var = "비전역 변수"
        print(global_var) # 가능
        print(nonlocal_var) # 가능

        def inner():
            local_var = "지역 변수"
            print(global_var) # 가능
            print(nonlocal_var) # 가능
            print(local_var) # 가능

        print(local_var) # 불가능 (NameError: name 'local_var' is not defined)

    print(nonlocal_var) # 불가능 (NameError: name 'nonlocal_var' is not defined)
    print(local_var) # 불가능 (NameError: name 'local_var' is not defined)


---- 

    def outer():
        num = 0 

        def inner():
            print(num)

        inner()

        print(num)

    출력:
    0
    0

---- 

    num = 0

    def outer():
        global num
        num = 100
        print(num)

    outer()

    print(num)

    출력:
    100
    100

------ 

    def outer():
        num = 0 

        def inner():
            print(num)

        inner()

        print(num)

    출력:
    0
    0

--------

    def outer():
        num = 0

        def inner():
            num = 100
            print(num)

        inner()

        print(num)

    출력:
    100
    0

---------

    def outer():
        num = 0

        def inner():
            nonlocal num
            num = 100
            print(num)

        inner()

        print(num)

    출력:
    100
    100

----------

    이건??
    num = 0
    def outer():
        global num

        def inner():
            nonlocal num
            num = 100
            print(num)

        inner()

        print(num)

    




- Reference : https://www.daleseo.com/python-global-nonlocal/
