## 클래스
- 일종의 구조체지만 기본적으로 해당 멤버에 밖에서 접근 불가능하다.
- 하지만 C++의 클래스는 기본적으로 밖에서 접근이 가능하다.
- C++에서는 구조체 보다 클래스를 사용한다.
- 데이터 은닉, 캡슐화
```C++
#include <string>
    class Person
    {
        std::string d_name;         // 이름
        std::string d_address;      // 주소
        std::string d_phone;        // 전화번호
        size_t      d_mass;         // kg 단위 몸무게.

        public:                     // 멤버 함수
            void setName(std::string const &name);
            void setAddress(std::string const &address);
            void setPhone(std::string const &phone);
            void setMass(size_t mass);

            std::string const &name()    const;
            std::string const &address() const;
            std::string const &phone()   const;
            size_t mass()                const;
    };
// Person.h
```
1. 생성자
- 생성자의 이름은 클래스 이름과 동일
- 반환값 조차 없다 `void` 도 아님
- 클래스의 변수를 정의하는 즉시, 생성자가 호줄되도록 보장한다.
- 생성자를 정의하지 않으면 컴파일러가 기본 생성자를 대신 정의.
- 생성자는 객체가 생성되고 나면 데이터 멤버가 합리적인 정의가 잘되어있는지 확인해야 한다.
- 전역 변수로 선언한 실체도 생성자를 호출한다.
  ``` c++
  #include <iostream>
    using namespace std;

    class Demo
    {
        public:
            Demo();
    };

    Demo::Demo()
    {
        cout << "Demo constructor called\n";
    }

    Demo d;       // 정의하는 즉시 실행된다.

    int main()
    {}

    /*
        출력:
        Demo constructor called
    */
  ```
- 만약 전역, 지역에 실체를 선언했으면 생성자는 전역 -> 지역 순으로 생성된다.
```c++
#include <iostream>
    #include <string>
    using namespace std;

    class Test
    {
        public:
            Test(string const &name);   // 인자가 주어진 생성자
    };

    Test::Test(string const &name)
    {
        cout << "Test object " << name << " created" << '\n';
    }

    Test globaltest("global");

    void func()
    {
        Test functest("func");
    }

    int main()
    {
        Test first("main first");
        func();
        Test second("main second");
    }
/*
    출력:
Test object global created
Test object main first created
Test object func created
Test object main second created
*/
```
- 매개변수가 있는 생성자 정의시 기본생성자(매개변수가 없는)는 필요시 직접 정의해야한다.
2. 생성자 위임하기
```C++
  class Stat
    {
        public:
            Stat()
            :
                State("", "")   // 파일이름/검색경로 없음
            {}
            Stat(std::string const &fileName)
            :
                Stat(fileName, "")  // 파일이름만 제공
            {}
            Stat(std::string const &fileName, std::string const &searchPath)
            :
                d_filename(fileName),
                d_searchPath(searchPath)
            {
                // 나머지 조치는 생성자가 수행한다.
            }
    };
```

3. `default` 키워드
- 기본 복사 생성자 및 기본 대입 연산자를 사용하겠다는 의미
- 깊은 복사를 하지 않아도 되므로 컴파일러가 구현해주는 디폴트 함수 및 연산자를 사용한다.
```C++
 class Strings
    {
        public:
            Strings() = default;
            Strings(std::string const *sp, size_t size);

            Strings(Strings const &other) = delete;
    };
```
4. `inline` 함수
- 컴파일러에게 함수를 호출하는 장소마다 함수의 코드를 삽입하라고 요청
- 실행시간이 빨라질 수 있다. 함수를 호출할 필요가 없기 때문
- 함수의 몸체가 아주 작을때만 사용하는 것이 좋다.
- c++ 17에서는 `inline` 변수도 사용가능

5. `sizeof`
- 클래스의 데이터 멤버에 `sizeof` 적용할 수 있다.
```c++
 class Data
    {
        std::string d_name;
        ...
    };

sizeof(Data::d_name);
```
  
