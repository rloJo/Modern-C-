## C++의 장점
---
- 코드 재사용에 따른 개발시간 단축
- 새 데이터 유형을 만들어 사용하는 것이 <b>C</b>보다 쉽다.
- 메모리 관리가 더 쉽고 투명하다.
- 버그가 덜 발생한다. (구문과 유형을 더 엄격하게 정검)
- '데이터 은닉(Data hiding)'을 더 쉽게 구현할 수 있다.

## 객체 지향 프로그래밍이란?
---
- C 프로그래밍 문제는 '절차적 접근법'으로 해결. 문제는 작은 문제로 분해되고, 이 과정을 반복
- 객체 지향 접근법은 Keywords를 식별하고. 이 keywords는 다이어그램에 묘사되어 내부적인 계통을 묘사
- keywords은 구현에서 객체가 되고 객체사이의 관계가 정의된다.

 ### C와 C++ 의 차이
 ---
1. `main` 함수
C++ 에는 `main`함수가 두가지 형태만 있다.
`int main()` 과 `int main(int argc, char **argv)이다.
- 반환 유형은 `int`이다. `void`가 아님
- `main`함수는 중복 정의가 불가능
- `return`서술문을 사용하지 않아도 생략시 `0`을 return
- `argvc[argc]`의 값은 0이다.
2. 함수 중복 정의 (함수 오버로딩)
- 이름은 동일하지만 다르게 작동하는 함수를 정의하는 것이 가능하다  
- 단, 매개변수 리스트가 달라하한다(`const` 속성에 따라서도 달라짐)  
- 반환 값만 다른것은 오버로딩 불가능하다.  
※컴파일러(링커)는 함수이름을 완전히 다른 이름으로 사용하기 때문이다.  
Ex) `void show(int)`는 내부적으로 VshowI, `void show (char *val)` 은 VshowCP
```C++
 #include <cstdio>

    void show(int val)
    {
        printf("Integer: %d\n", val);
    }

    void show(double val)
    {
        printf("Double: %lf\n", val);
    }

    void show(char const *val)
    {
        printf("String: %s\n", val);
    }

    int main()
    {
        show(12);
        show(3.1415);
        show("Hello World!\n");
    }
```
3. 기본 함수 인자
- 함수를 정의할 때, 기본 인자를 줄 수 있다.
- 생략을 할때는 반드시 오른쪽 부터 생략한다.
``` C++
#include <stdio.h>
    void showstring(char *str = "Hello World!\n");

    int main()
    {
        showstring("Here's an explicit argument.\n");
        showstring();           // 실제로 다음과 같다.
                                // showstring("Hello World!\n");
    }
```

``` C++
    void two_ints(int a = 1, int b = 4);

    int main()
    {
        two_ints();            // 인자:  1, 4
        two_ints(20);          // 인자: 20, 4
        two_ints(20, 5);       // 인자: 20, 5
        //two_ints(,6);  불가능하다 오른쪽 부터 생략해야한다.
    }
```

4. NULL-포인터 vs 0-포인터 nullptr
- C++ 에서 0 값은 모두 (int)0으로 코딩된다.
- C++ 에서 NULL은 피하는게 좋다.
- 아래 상황에서 show(0) 호출시 show(int)가 호출된다. show(char const*) 호출 x
- show(NULL) 도 마찬가지로 show(int) 호출. ((void*)0) 으로 정의하지 않기 때문
- show(nullptr)로 선언해야 show(char const*)이 호출된다.
``` C++
 #include <stdio.h>

    void show(int val)
    {
        printf("Integer: %d\n", val);
    }

    void show(double val)
    {
        printf("Double: %lf\n", val);
    }

    void show(char const *val)
    {
        printf("String: %s\n", val);
    }

    int main()
    {
        show(12);
        show(3.1415);
        show("Hello World!\n");

        int *ptr = nullptr; // OK
        int val = nullptr;// 오류: 값이 포인터가 아님
    }
```
5. 구조체 안에 포함된 메소드
- 메소드를 구조체의 멤버로 정의할 수 있다.
- `strcut Point`의 크기는 `int` 두개의 크기와 같다. 구조체 안에 선언된 함수는 크기에 영향을 주지 않음
```c++
    struct Point            // 화면 위의 점 하나 정의
    {
        int x;              // 좌표
        int y;              // x/y
        void draw();        // 그리기 함수
    };

    Point a;                // 화면 위의
    Point b;                // 두 개의 점

    a.x = 0;                // 첫 번째 점을 정의하고
    a.y = 10;               // 그 점을 그린다.
    a.draw();

    b = a;                  // a를 b에 복사
    b.y = 20;               // y-좌표를 재정의하고
    b.draw();               // 그 점을 그린다.
```


