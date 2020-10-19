## Chapter2

### C 기본 복습

* **const**
  * 변수를 상수화
* **메모리 공간**
  * **데이터** - 전역변수가 저장되는 영역
  * **스택** - 지역변수 및 매개변수가 저장되는 영역
  * **힙** - malloc 함수호출에 의해 프로그램이 실행되는 과정에서 동적으로 할당이 이뤄지는 영역
  * **malloc & free** - malloc 함수호출에 의해 할당된 메모리 공간은 free 함수호출을 통해 소멸하지 않으면 해제되지 않음
* **Call-By-Value & Call-By-Reference**

```c++
void SwapByValue(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
} // Call-By-Value

void SwapByRef(int *c, int *d)
{
    int tmp = *c;
    *c = *d;
    *d = tmp;
} // Call-By-Reference

```

### 자료형 bool

* c++에서는 참 거짓 표현을 위해 `true`, `false` 키워드를 정의

```c++
// c에서는 참거짓 표현을 위해 매크로 상수를 정의함
#define TRUE 1
#define FALSE 0
```

* true, false는 각각 1과 0을 의미하지 않음
  * 정수 형태로 형변환을 했을 경우 1과 0으로 변환될 뿐
* 자료형 `bool`

```c++
#include <iostream>
using namespace std;

bool IsPositive(int num)
{
   if(num<0) 
       return false;
   else  
       return true;
}

int main(void)
{
   bool isPos;
   int num;
   cout<<"Input number: ";
   cin>>num;

   isPos=IsPositive(num);
   if(isPos)
       cout<<"Positive number"<<endl;
   else
       cout<<"Negative number"<<endl;

   return 0;
}
```

### 참조자(Reference)

* 변수는 할당된 메모리 공간에 붙여진 이름
  * 이름을 통해 해당 메모리 공간에 접근 가능
* **참조자를 통해 할당된 하나의 메모리 공간에 추가로 이름을 부여가능**
  * **참조자는 자신이 참조하는 변수를 대신할 수 있는 또 하나의 이름**

```c++
int num1 = 2020;
int &num2 = num1; // 참조자 num2
```

```c++
int *ptr = &num1; // 변수 num1의 주소 값을 반환해서 포인터 ptr에 저장
int &num2 = num1; // 변수 num1에 대한 참조자 num2를 선언
// num2는 num1과 동일한 메모리 공간을 가리킴
```

```c++
#include <iostream>
using namespace std;

int main(void)
{
	int num1=1020;
	int &num2=num1;

	num2=3047;
	cout<<"VAL: "<<num1<<endl; // 3047
	cout<<"REF: "<<num2<<endl; // 3047

	cout<<"VAL: "<<&num1<<endl; // 동일한 주소값
	cout<<"REF: "<<&num2<<endl; // 동일한 주소값

	system("pause");
	return 0;
}
```

* 참조자의 수에는 제한이 없으며 대상으로도 참조자를 선언가능

```c++
int num1 = 2759;
int &num2 = num1;
int &num3 = num2;
int &num4 = num3;
```

* 참조자의 선언 가능한 범위
  * 변수에 대해서만 선언이 가능, 선언됨과 동시에 참조해야 함
  * 참조자 선언 후 누군가를 참조하는 것은 불가능
  * null로 초기화도 불가능

```c++
// (x)
int &ref = 20;
int &ref;
int &ref = NULL;
```

* 포인터 변수도 참조자 선언이 가능

```c++
int num = 12;
int *ptr = &num;

int &ref = num;
int *(&pref) = ptr;
```

* **Call-By-Reference == 참조자(reference) 기반 함수 호출**
  * 주소 값을 전달 받아 함수 외부에 선언된 변수에 접근하는 형태의 함수호출
  * C++에서는 함수 외부에 선언된 변수의 접근방법으로 두 가지가 존재
    * 주소 값을 이용하는 방식
    * 참조자를 이용하는 방식

```c++
#include <iostream>
using namespace std;

void SwapByRef(int *ref1, int *ref2) // 주소 값을 이용하는 방식
{
	int temp=*ref1;
	*ref1=*ref2;
	*ref2=temp;
}

int main(void)
{
	int val1=10;
	int val2=20;
	
	SwapByRef(&val1, &val2);
	cout<<"val1: "<<val1<<endl;
	cout<<"val2: "<<val2<<endl;
	return 0;
}
```

```c++
#include <iostream>
using namespace std;

void SwapByRef2(int &ref1, int& ref2) // 참조자를 이용하는 방식
{
	int temp=ref1;
	ref1=ref2;
	ref2=temp;
}

int main(void)
{
	int val1=10;
	int val2=20;
	
	SwapByRef2(val1, val2);
	cout<<"val1: "<<val1<<endl;
	cout<<"val2: "<<val2<<endl;
	return 0;
}
```

* 반환형이 참조형인 경우

```c++
#include <iostream>
using namespace std;

int & RefRetFuncOne(int &ref)
{
	ref++;
	return ref;
}

int main(void)
{
	int num1=1;
	int &num2=RefRetFuncOne(num1);

	num1++;
	num2++;
	cout<<"num1: "<<num1<<endl; // 4
	cout<<"num2: "<<num2<<endl; // 4
	return 0;
}
```

```c++
#include <iostream>
using namespace std;

int & RefRetFuncOne(int &ref)
{
	ref++;
	return ref;
}

int main(void)
{
	int num1=1;
	int num2=RefRetFuncOne(num1);

	num1+=1;
	num2+=100;
	cout<<"num1: "<<num1<<endl; // 3
	cout<<"num2: "<<num2<<endl; // 102
	return 0;
}
```

* const 참조자로 상수를 참조가능

```c++
const int &ref = 20; // 20이란 literal constant도 참조가능
```

### **malloc & free를 대신하는 new & delete**

* **C 동적할당**
  * 할당할 대상의 정보를 무조건 바이트 크기단위로 전달해야 함
  * 반환형이 void형 포인터이기 때문에 적절한 형 변환을 거쳐야 함

```c++
#include <iostream>
#include <string.h>
#include <stdlib.h>
using namespace std;

char * MakeStrAdr(int len)
{
	char * str = (char*)malloc(sizeof(char)*len); // c 동적할당
	return str;
}

int main(void)
{
	char * str = MakeStrAdr(20);
	strcpy(str, "I am so happy~");
	cout<<str<<endl;
	free(str);
	return 0;
}
```

* C++에서는 `new`, `delete` 키워드 제공돼 C 동적할당 불편함이 사라짐

```c++
#include <iostream>
#include <string.h>
using namespace std;

char * MakeStrAdr(int len)
{
	// char * str=(char*)malloc(sizeof(char)*len);
	char * str = new char[len];
	return str;
}

int main(void)
{
	char * str = MakeStrAdr(20);
	strcpy(str, "I am so happy~");
	cout<<str<<endl;
	// free(str);
	delete []str;
	return 0;
}
```

### C++에서 C 표준함수 호출

* C언어 표준함수를 사용할 땐 `c`를 더하고 `.h`를 뺌
  * 하위 버전과의 호환성(backwards compatibility)를 제공
  * C++ 표준함수에는 함수들이 오버로딩되어 있어 가급적 C++ 표준헤더를 이용해 함수호출하는게 좋음

```c++
// C
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>

// C++
#include <cstdio>
#include <cstdlib>
#include <cmath>
#include <cstring>
```

