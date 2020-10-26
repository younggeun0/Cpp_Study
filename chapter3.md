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





### 객체지향 프로그래밍의 이해

### 