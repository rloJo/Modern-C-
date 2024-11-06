## 눈에 띄는 C와의 차이
---
1. `const` 키워드의 사용
- `const` 키워드는 인자나 변수값이 불변임을 알리는 수식어.
```C++
 int main()
    {
        int const ival = 3;     // 상수 int
                                // 3으로 초기화

        ival = 4;               // 할당하면
                                // 에러 메시지 출력
    }
```
- `const`로 선언된 변수는 C와 다르게 배열 크기로 지정 가능하다.
```c++_
  int const size = 20;
  char buf[size];
```
- 포인터선언
```c++
  char const *buf; // buf는 한 무더기 char을 가리키는 포인터 변수
                   // buf가 가리키는 값을 수정 불가능 하다.
  *buf = 'a' // 불가능
  buf++;  // 가능

  char *const buf;
  *buf = 'a' // 가능
  buf++;  // 불가능

  char const *const buf; // 이것도 가능하다
  *buf = 'a' // 불가능
  buf++;  // 불가능
```
- `const` 키워드의 위치에 대하여 키워드 오른쪽에 나타나는 것은 무엇이든 바꿀수 없다.

2. 영역 지정 연산자::
- `::`는 지역변수가 이미 있는데 같은 이름으로 전역변수가 존재한느 상황에 사용한다.
```c++
  #include <stdio.h>

    double counter = 50;                // 전역 변수

    int main()
    {
        for (int counter = 1;           // 이 변수는 
             counter != 10;             // 지역 변수를 가리킨다.
             ++counter)
        {
            printf("%d\n",
                    ::counter           // 전역 변수
                    /                   // 나누기
                    counter);           // 지역 변수
        }
    }
```
### 구조체에 포함되는 함수
```c++
    struct Person
    {
        char name[80];
        char address[80];

        void print();
    };

    void Person::print()
    {
        cout << "Name:      " << name << "\n"
                "Address:   " << address << '\n';
    }
```
1. 데이터 은닉: public 과 private 그리고 class
- `public` : 구조체의 모든 필드에 모든 코드가 접근할 수 있다. 
- `private` : 구조체 자체에서만 잇따르는 필드에 접근 가능.
``` c++
   struct Person
    {
        private: // 구조체에 정의된 멤버 함수만 접근 가능
            char d_name[80];
            char d_address[80];
        public: 
            void setName(char const *n);
            void setAddress(char const *a);
            void print();
            char const *name();
            char const *address();
    };
```
2. C 구조체와 C++ 구조체
- C++는 구조체에 매개변수를 사용하지 않는다.
- 기존 C는 구조체안에 함수를 넣지 못하기 때문에 매개변수로 해당 변수를 입력받음
```c++
 /* 구조체 PERSON의 정의    C로 구현됨   */
    typedef struct
    {
        char name[80];
        char address[80];
    } PERSON;

    /* PERSON 구조체를 조작하기 위한 함수들 */

    /* 이름과 주소로 필드를 초기화한다    */
    void initialize(PERSON *p, char const *nm,
                       char const *adr);

    /* 정보를 인쇄한다    */
    void print(PERSON const *p);

    /* etc..    */
 /* C++로 구현 */
  class Person
    {
        char d_name[80];
        char d_address[80];

        public:
            void initialize(char const *nm, char const *adr);
            void print();
            // etc..
    };
```


### C문법에 추가된 것들
1. 참조
- 별명과 비슷하다.
``` C++
  int int_value;
  int &ref = int_value;
// ref가 int값을 참조 한다고 알려준다.
```
```c++
   void increase(int *valp)    // int를 가리키는
    {                           // 포인터를 기대함
        *valp += 5;
    }

    int main()
    {
        int x;

        increase(&x);           // x의 주소를 건넴
    }
```
```C++
    void increase(int &valr)    // int를 참조함
    {                           
        valr += 5;
    }

    int main()
    {
        int x;

        increase(x);            // 참조를 건넴
    }
```
- 다음 위 코드는 동일한 효과를 얻는다.
2. Rvalue 참조
