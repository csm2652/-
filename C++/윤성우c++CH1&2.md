## C++
+ ### using namespace시 아래와 같은 코드가 필요하면
```
    AAA::BBB::CCC::num1 = 30
-> 	namespace ABC = AAA::BBB::CCC
	ABC::num1 = 30;
```
 이렇게 namespace를 새로 별칭을 주어 접근가능하다.
 ___

+ ### `const` 관련 변수는 `const`의 위치에 따라 다르다
예시: 
```c
const int num = 10;   //변수 num 상수화
const int* ptr1 = &val1; // 변수명이 가리키는 주소의 '값'을 상수화 
//→ 포인터가 가리키는 값을 상수화/ *ptr1 = val3 불가, ptr1 = &val2같은 주소값 변경가능

int* const prt2 = &val2; 
/*
포인터 변수를 상수화한다. -> 포인터 변수(주소 저장)가 상수화/ 
ptr2가 가리키는 주소의 값은 변경할 수 있다./
*ptr2 = val3가능 ptr2 = &val2 불가
*/
int ViewCount() const { // 메소드의 상수화(같은 클래스 맴버 변수 변경 불가)
        // view++; // 컴파일 에러. 멤버 변수 값을 변경할 수 없다.
        return viewCounnt;
    }
```
___
+ ### TRUE / FALSE
기본적으로 C는 `true false`가 따로 없다. 그냥 0과 1을 쓴다. C99 이후 `true` `false`가 생겼지만 그냥 `#define TRUE 1 #define FALSE 0`을 주로 쓴다. 컴파일러가 C99 이상 버전이 아닐 수 도 있기에..
**참고로 C++에서 _`true`와 `false`는 정수형 1, 0과 다르다_**
C++에서 `true false`는 참 거짓을 표현하기 위한 1바이트 크기의 데이터일 뿐이며, 다만, `true false`가 정의되기 이전에는 참을 표현하기 위해서 숫자 1을, 거짓을 표현하기 위해서 숫자 0을 사용했기 때문에, 이 둘을 출력하거나 정수의 형태로 형 변환하는 경우에 각각 1과 0으로 변환되도록 정의되어있 을 뿐이다.
___
+ ### 변수
>"변수는 할당된 메모리 공간에 붙여진 이름이다. 그리고 그 이름을 통해서 해당 메모리 공간에 접근이 가능하다."
별거아닌 팁: 변수 명은 컴파일러에서 symbol table에 주소랑 타입까지해서 잘 저장해둔다.. 우리가 모든 주소위치(0xffffff같은거 외울 필요가 없는게 다 symbol table 덕분..

+ ### 포인터
>"할당받은 메모리 안에 대입되는 변수의 주소타입을 저장하기 위해 존재하는 변수타입"
 한글로 \*는 역참조 연산자, &는 참조 연산자라 한다.

+ ### 참조자(&)
>참조자는 자신이 참조하는 변수를 대신할 수 있는 또 하나의 이름
변수에 별명(별칭)을 하나 붙여주는 것
    
참조자는 변수에 대해서만 선언이 가능하고, 선언됨과 동시에 누군가를 참조해야만 한다. 즉, 다음 선언은 유효하지 않다.
```cpp
int &ref=20; (x)
int &ref; (x)
int &ref = NULL; (x)
```
또한, 참조자를 함수의 매개변수로 설정해두면, 함수 호출시 삽입되는 변수을 기반으로 초기화를 한다.
이때, 반환을 다시 참조형으로 하면 삽입된 변수와 완전히 같은 변수를 얻게된다.
```cpp
int num = 1;
int& reference(int &ref){
	ref++
    return ref;
}
int num2 = reference(num);
-----------------------------
위는 num2와 num은 같은 메모리 공간을 공유한다.
------------------------------
int num = 1;
int reference(int &ref){
	ref++
    return ref;
}
int num2 = reference(num);
----------------------------
위는 reference함수가 참조형 ref의 변수의 값을 반환하게 되어 num2와 num은 다른게 된다.
```
마지막으로 헷갈리기 쉬운 배열, 포인터 참조자 관련 예시이다.
```cpp
int num = 12;
int *ptr = &num;
int **dptr = &ptr;

int &ref = num;
int *(&pref) = ptr; // int* &pref -> int*의(정수형 포인터) 참조자, 즉 ptr의 주소를 담는것
int **(&dpref) = dptr; //int** &dpref -> int**(정수형 포인터의 포인터)의 참조자, 즉 dptr의 주소를 담는것

//사실 ref나 ptr이나 쓰임새는 똑같은 것이다... 주소값을 통한 변경등에 쓰이겠지.. 
//근데 참조자는 "말그대로 같은 공간"을 가리키고 있고, 
//포인터 변수는 할당된 포인터변수 매모리에 값으로서 할당받은 주소를 기록하는점이 다르다.
cout << ref << endl;
cout << *ref << endl;
cout << **ref << endl;
retur 0;
______
console:
12
12
12
```
+ Call-by-value / Call-by-reference
함수의 대표적인 호출 방식에는 call-by-value와 call-by-reference가 있다.
>Call-by-value: 값을 인자로 전달하는 함수의 호출방식
Call-by-reference: 주소 값을 인자로 전달하는 함수의 호출방식/ 더 정확히는 주소 값을 전달받아서, 함수 외부에 선언된 변수에 접근하는 형태의 함수 호출
___즉, 주소값을 전달 받았다는 사실 보단 주소 값이 참조의 도구로 사용되었다는 사실이 더 중요하다___


