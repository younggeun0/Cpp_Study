## Chapter1

### printf, scanf를 대신하는 입출력 방식

* C++ 파일 확장자는 `.cpp`
* 헤더파일 선언문에 표준 ㅎ더 파일의 선언에서는 확장자(`.h`) 생략
* 표준 입출력
  * `std::cout`과 `<<`, `std::cin`과 `>>`


```c++
#include <iostream>

int main(void)
{
	int val1;
	std::cout<<"number1 : ";
	std::cin>>val1;

	int val2;	
	std::cout<<"number2 : ";
	std::cin>>val2;

	int result=val1+val2;
	std::cout<<"added : "<<result<<std::endl;

	return 0;
}
```

* 변수 선언은 어디서든 가능
  * 컴파일러는 지역변수의 선언 위치에 제한을 두지 않음
* C++에서는 데이터의 입력도 데이터의 출력과 마찬가지로 별도의 포맷 지정이 필요 없음

```c++
// c
char str[100];
scanf("%s", str);
// c++
char str[100];
std::cin>>str;
```

### 함수 오버로딩(Function Overloading)

* **함수 오버로딩**
  * **매개변수의 선언 형태가 다르다면 동일한 이름의 함수 정의를 허용하는 것**
  * 매개변수의 자료형 또는 개수가 달라야 함


```c++
#include <iostream>

void MyFunc(void)
{
	std::cout<<"MyFunc(void) called"<<std::endl;
}

void MyFunc(char c)
{
	std::cout<<"MyFunc(char c) called"<<std::endl;
}

void MyFunc(int a, int b)
{
	std::cout<<"MyFunc(int a, int b) called"<<std::endl;
}

int main(void)
{
	MyFunc();
	MyFunc('A');
	MyFunc(12, 13);

	return 0;
}
```

### 매개변수의 디폴트 값(Default Value)

* 디폴트 값은 함수 선언 부분에만 표현
  * 반드시 오른쪽 매개변수의 디폴트 값부터 채우는 형태로 정의
  * 

```c++
#include<iostream>

int BoxVolume(int length, int width=1, int height=1);

int main()
{
	std::cout<<"[3, 3, 3] : "<<BoxVolume(3, 3, 3)<<std::endl;
	std::cout<<"[5, 5, D] : "<<BoxVolume(5, 5)<<std::endl;
	std::cout<<"[7, D, D] : "<<BoxVolume(7)<<std::endl;

	return 0;
}

int BoxVolume(int length, int width, int height)
{
	return length*width*height;
}

```

### 인라인(inline) 함수

* C 매크로 함수
  * 장점 - 일반적 함수에 비해 실행속도가 빠름
  * 단점 - 정의하기 어려움, 복잡한 매크로의 형태로 정의하는데 한계가 있음

```c++
#include <iostream>
#define SQUARE(x) ((x)*(x))  // C 매크로 함수

int main(void) 
{
    std::cout<< SQUARE(5) <<std::end;
    return 0;
}
```

```c++
#include <iostream>

inline int SQUARE(int x) // c++ inline function
{
	return x*x;
}

int main(void)
{
	std::cout<<SQUARE(5)<<std::endl;
	std::cout<<SQUARE(12)<<std::endl;
	return 0;
}
```

* C++ 템플릿이라는 것을 이용하지 않으면 자료형에 의존적임

```c++
#include <iostream>

template <typename T>
inline T SQUARE(T x) // c++ inline function
{
	return x*x;
}

int main(void)
{
	std::cout<<SQUARE(5)<<std::endl;
	std::cout<<SQUARE(12)<<std::endl;
	return 0;
}
```

### 이름공간(namespace)

* **동일한 이름의 함수명 등으로 발생하는 이름 충돌의 문제점을 해결하기 위함**
* `::`를 **범위지정 연산자(scope resolution operator)**
  * 이름 공간을 지정할 때 사용하는 연산자

```c++
#include <iostream>

namespace BestComImpl
{
	void SimpleFunc(void);
}

namespace BestComImpl
{
	void PrettyFunc(void);
}

namespace ProgComImpl
{
	void SimpleFunc(void);
}

int main(void)
{
	BestComImpl::SimpleFunc();

	system("pause");
	return 0;
}

void BestComImpl::SimpleFunc(void)
{
	std::cout<<"BestCom이 정의한 함수"<<std::endl;
	PrettyFunc();					// 동일 이름공간
	ProgComImpl::SimpleFunc();		// 다른 이름공간
}	

void BestComImpl::PrettyFunc(void)
{
	std::cout<<"So Pretty!!"<<std::endl;
}

void ProgComImpl::SimpleFunc(void)
{
	std::cout<<"ProgCom이 정의한 함수"<<std::endl;
}
```

* 이름공간은 다른 이름공간 안에 삽입 가능

```c++
namespace Parent 
{
    int num=2;
    namespace SubOne 
    {
        int num=3;
    }
    namespace SubTwo
    {
        int num=4;
    }
}

//...

std::cout<<Parent::num<<std::end;
std::cout<<Parent::SubOne::num<<std::end;
std::cout<<Parent::SubTwo::num<<std::end;
```

* `using`을 이용한 이름공간의 명시

```c++
#include <iostream>

namespace Hybrid
{
	void HybFunc(void)
	{
		std::cout<<"So simple function!"<<std::endl;
		std::cout<<"In namespace Hybrid!"<<std::endl;
	}
}

int main(void)
{
	using Hybrid::HybFunc;
	HybFunc();

	system("pause");
}

```

```c++
#include <iostream>
using namespace std;

int main(void)
{
	int num=20;
	cout<<"Hello World!"<<endl;
	cout<<"Hello "<<"World!"<<endl;
	cout<<num<<' '<<'A';
	cout<<' '<<3.14<<endl;

	return 0;
}
```

* 이름공간에 별칭(alias) 사용 가능

```c++
#include <iostream>
using namespace std;

namespace AAA
{
	namespace BBB
	{
		namespace CCC
		{
			int num1;
			int num2;
		}
	}
}

int main(void)
{
	AAA::BBB::CCC::num1=20;
	AAA::BBB::CCC::num2=30;

	namespace ABC=AAA::BBB::CCC;

	cout<<ABC::num1<<endl;
	cout<<ABC::num2<<endl;

	return 0;
}
```
