## 7. 객체 생성 시 괄호와 중괄호를 구분하라

c++11에서는 객체 생성 구문이 다양해졌다.
```cpp

int x(0);

int y = 0;

int z{ 0 };

int z = { 0 };

```

> assignment

위 코드에서 중괄호를 이용한 생성에서 assignment(할당)이 일어난다고 오해하기도 하지만 assignment는 일어나지 않는다.
int 같은 내장 형식에서 초기화와 assignment 구분은 학술적인 차원에서 차이가 나지만 
사용자 정의 형식에서 초기화와 assignment가 각자 다른 함수를 호출하므로 구분해야 한다.
```cpp
Widget w1;      // 기본 생성자 호출
Widget w2 = w1; // assignment X. 복사 생성자 호출
w1 = w2;        // assignment O. 복사 assignment 연산자(operator=)을 호출
```

> uniform initialization



## 질문들
> 초기화와 할당의 차이
* https://katyscode.wordpress.com/2013/02/27/c-explained-object-initialization-and-assignment-lvalues-and-rvalues-copy-and-move-semantics-and-the-copy-and-swap-idiom/
* 초기화 - 객체를 선언하고 객체에 초기 값을 제공하는 표현식을 제공하는 것
* 할당 - 기존 오브젝트 값을 변경할 때
