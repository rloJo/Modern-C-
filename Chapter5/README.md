## 정적 데이터와 함수
- **C** 에서는 여러 함수가 같은 변수에 접근할 필요가 있는 상황에 해당 변수를 `static`으로 정의한다.
- **C++** 에서는 정적 멤버를 정의한다.
- 클래스의 모든 실체가 데이터와 함수에 접근할 수 있지만(private) 밖에서는 접근 불가능하다. 

1.0 정적 데이터
-
- 클래스의 데이터 멤버라면 `static`으로 선언이 가능하다.
- 정적 데이터 멤버는 한 번만 생성되고 초기화 한다.
- 비-정적 데이터는 클래스의 실체마다 따로 생성된다.
- 일반적으로 `s_`를 변수명 앞에 붙여 사용한다.
```C++
    class Test
    {
        static int s_private_int;

        public:
            static int s_public_int;
    };

    int main()
    {
        Test::s_public_int = 145;   // OK
        Test::s_private_int = 12;   // 불가
                                    // 비공개 영역은 건드리지 못함
    }
```
1.1 비공개 정적 데이터
-
``` c++
    class Directory
    {
        static char s_path[];

        public:
            // 생성자, 소멸자, 등등.
    };
```
- `s_path`는 비공개 정적 데이터 멤버이다. 프로그램이 실행될 동안 `Directory::s_path[]` 하나만 존재한다.
- 하지만 `Directory` 클래스의 다른 멤버 함수 또는 생성자 소멸자가 들여다 보거나 변경할 수 없다.
- 생성자가 호출되기 전에 이미 정적 데이터 멤버가 존재하기 때문이다.
```C++
    class Graphics
    {
        static int s_nobjects;              // 실체의 갯수를 센다.

        public:
            Graphics();
            ~Graphics();                    // 다른 멤버는 보여주지 않음.
        private:
            void setgraphicsmode();         // 그래픽 모드로 전환
            void settextmode();             // 텍스트 모드로 전환
    }
```
1.2 공개 정적 데이터
-
- 데이터 멤버는 클래스의 공개 구역에 선언해도 된다.
- 하지만 데이터 은닉원칙에 위배 지양한다.

1.3 일반화된 상수 표현식(constexpr)
-
- 객체나 함수의 리턴값을 컴파일 타입에 값을 알 수 있다라는 의미를 갖는다.
- 컴파일 시간에 상수를 만드는 키워드 (컴파일 시간(실행시간 전)에 결정되는 상수 값으로만 초기화 가능)
- `constexpr`로 정의된 변수는 (변경불능) 상수 값을 갖는다.
- `constexpr`은 항상 `const`이지만 `const` 는 항상 `constexpr`은 아니다.
