## 이름 공간
- 식별자가 정의되어 있는 코드 코드 영역을 만드는 것.
- 이름 공간에 정의된 식별자는 다른 곳에 정의된 이름과 충돌하지 않는다.
- 부가기능
  1. `using namespace`를 사용해 이름 공간을 가져온다.
  2. `namespace p = FS` 별칭을 이용할 수 있다.
  3. 익명의 이름공간에 함수나 변수를 감출 수 있다.

- 이름 공간을 정의하는 법
```c++
   namespace identifier
    {
        // 선언된 또는 정의된 개체
        // (선언 구역)
    }
```
- 개체 참조하기 
```c++
int main()
    {
        using CppAnnotations::cos;
        ...
        cout << cos(60)         // CppAnnotations::cos()를 호출
            << ::cos(1.5)       // 표준 cos() 함수를 호출
            << '\n';
    }
```
```c++
 int main()
    {
        using CppAnnotations::value;
        ...
        cout << value << '\n';  // CppAnnotations::value 사용
        int value;              // 에러: 이미 선언된 값.
    }

```
## 쾨닉 검색(Koenig lookup)
- 이름공간을 지정하지 않고 함수를 호출하면 인자 유형의 이름공간을 따라 함수의 이름 공간을 결정
- 인자 유형이 정의된 이름공간에 그런 함수가 있다면 그 함수가 사용된다.
```c++
  #include <iostream>

    namespace FBB
    {
        enum Value        // FBB::Value 정의
        {
            FIRST
        };

        void fun(Value x)
        {
            std::cout << "fun called for " << x << '\n';
        }
    }

    int main()
    {
        fun(FBB::FIRST);    // 쾨닉 검색: 이름공간이
                            // fun()에 지정되어 있지 않으므로
    }
    /*
        출력:
    fun called for 0
    */
```
```c++
#include <iostream>

    namespace FBB
    {
        enum Value        // FBB::Value를 정의한다.
        {
            FIRST
        };

        void fun(Value x)
        {
            std::cout << "FBB::fun() called for " << x << '\n';
        }
    }

    namespace ES
    {
        void fun(FBB::Value x)
        {
            std::cout << "ES::fun() called for " << x << '\n';
        }
    }

    int main()
    {
        fun(FBB::FIRST);    // 애매 모호함 없음: 인자가
                            // 이름공간을 결정한다.
    }
    /*
        출력:
    FBB::fun() called for 0
    */

```
