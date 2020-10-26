## Chapter3

### C++에서의 구조체

* **구조체는 연관 있는 데이터를 묶을 수 있는 문법적 장치**

```c++
struct Car
{
	char gamerID[ID_LEN];	// 소유자ID
	int fuelGauge;		// 연료량
	int curSpeed;		// 현재속도
}
```

```c++
// 구조체 변수 선언
struct Car basicCar;
```

* **구조체 안에 있는 함수삽입**
  * 구조체 밖에 함수들이 있으면 전역함수형태를 띔
  * 구조체와 관련된 종속적인 함수임을 알 수 없음

```c++
struct Car
{
	char gamerID[CAR_CONST::ID_LEN];	
	int fuelGauge;		
	int curSpeed;		

	void ShowCarState()
    {
        cout<<"소유자 ID: "<<gamerID<<endl;
        cout<<"연료량: "<<fuelGauge<<"%"<<endl;
        cout<<"현재속도: "<<curSpeed<<"km/s"<<endl<<endl;
    }
    ...
};

int main(void)
{
	Car run99={"run99", 100, 0};
	run99.ShowCarState();
	return 0;
}
```

* 함수의 원형선언을 구조체 안에 두고 함수의 정의를 구조체 밖으로 뺄 수 있음
  * **구조체 안에 함수가 정의돼 있으면 함수를 인라인으로 처리하라는 의미**

```c++
struct Car
{
	char gamerID[CAR_CONST::ID_LEN];	
	int fuelGauge;		
	int curSpeed;		

	void ShowCarState();
};

void Car::ShowCarState()
{
    cout<<"소유자 ID: "<<gamerID<<endl;
    cout<<"연료량: "<<fuelGauge<<"%"<<endl;
    cout<<"현재속도: "<<curSpeed<<"km/s"<<endl<<endl;
}
```

### 클래스(Class)와 객체(Object)

* `struct`를 `class`로 바꾸면 구조체가 아닌 클래스가 됨
  * 구조체처럼 변수 선언 불가

```c++
Car run99 = {"run99", 100, 0}; // X
Car run99; // O
```

* 클래스 내에 선언된 변수는 기본적으로 클래스 내에 선언된 함수에서만 접근 가능
* 구조체와 클래스의 가장 큰 차이 - `접근제한자`
  * **클래스를 정의하는 과정에서 각각의 변수 및 함수의 접근 허용범위를 별도로 선언**
    * `public` - 어디서든 접근허용
    * `protected` - 상속관계에 놓여 있을 때, 유도 클래스에서 접근허용
    * `private` - 클래스 내(클래스 내에 정의된 함수)에서만 접근허용
      * 접근제한자를 사용하여 `정보 은닉`

```c++
class Car
{
private:
	char gamerID[CAR_CONST::ID_LEN];	
	int fuelGauge;		
	int curSpeed;		
public:
	void InitMembers(char * ID, int fuel);
	void ShowCarState();
};

void Car::InitMembers(char * ID, int fuel)
{
	strcpy(gamerID, ID);
	fuelGauge=fuel;
	curSpeed=0;
};
void Car::ShowCarState()
{
    cout<<"소유자 ID: "<<gamerID<<endl;
    cout<<"연료량: "<<fuelGauge<<"%"<<endl;
    cout<<"현재속도: "<<curSpeed<<"km/s"<<endl<<endl;
}
```

* **객체(Object), 멤버변수, 멤버함수**
  * 구조체와 클래스는 변수의 성격만 지니는 것이 아니기 때문에 변수라는 표현 대신 `객체(Object)`란 표현을 사용
  * 클래스를 구성하는(클래스 내에 선언된) 변수를 가리켜 `멤버변수`라 함
  * 클래스를 구성하는(클래스 내에 정의된) 함수를 가리켜 `멤버함수`라 함

* **C++에서 파일 분할**
  * `.h`은 클래스의 선언을 담는다
    * 컴파일러가 클래스와 관련된 문장의 오류를 잡아내는데 필요한 최소한의 정보
    * 클래스가 구성하는 외형적 틀을 보여줌
    * 클래스 선언(Declaration)
    * 인라인 함수는 헤더 파일에 함께 넣는다
      * 컴파일 과정에서 함수의 호출 문이 있는 곳에 함수의 몸체 부분이 삽입되어야 하기 때문
  * `.cpp`는 클래스의 정의(멤버함수의 정의)를 담는다
    * 클래스 정의(Definition)
    * 함수의 정의는 다른 문장의 컴파일에 필요한 정보를 가지고 있지 않음
      * 컴파일 된 이후, 링커에 의해 하나의 실행파일로 묶이기만 함

### 객체지향 프로그래밍(OOP)의 이해

* **객체지향 프로그래밍은 현실에 존재하는 사물과 사물과 대상, 그리고 그에 따른 행동을 있는 그대로 실체화 시키는 형태의 프로그래밍**
  * 객체를 이루는 것은 `데이터`와 `기능`
    * 객체는 하나 이상의 상태 정보(데이터)와 하나 이상의 행동(기능)으로 구성
  * 클래스는 객체 생성을 위한 틀
