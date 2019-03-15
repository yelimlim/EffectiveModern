## 5. 명시적 형식 선언보다는 auto를 선호하라

> case 1
```cpp
int x;
```
위와 같이 초기화 없이 선언하면 x 값은 정해지지 않는다.

> case 2
```cpp
template<typename It>
void dwin(It b, it e)
{
  for(; bI= e; ++b)
  {
    tyname std::iterator_traits<It>::value_type currValue = *b;
    ...
  }
}
```
위 코드는 역참조를 통해 초기화 되는 지역 변수이다.
"tyname std::iterator_traits<It>::value_type"는 매우 길다.
  
> case 3

클로저 형식의 지역 변수는 선언할 수 없다.
컴파일러만 클로저의 형식을 알기 때문에 명시적으로 지정하는 것은 불가능하다.

> auto와 초기화
``` cpp
int x1;       // 에러 없이 컴파일 된다.
auto x2;      // 컴파일 에러: 초기치가 꼭 필요하다.
auto x3 = 0;    
```
auto 변수의 형식은 초기화하는 값으로 부터 연역 되므로 반드시 초기화를 해야 한다.
이를 통해 실수를 방지 할 수 있다.

> auto와 형식
```cpp
template<typename It>
void dwim(It b, It e)
{
  for(; b != e; ++b)
  {
    auto currValue = *b;
    ...
  }
}
```
위와 같이 컴파일러만 아는 형식도 auto를 통해 지정할 수 있다.

> auto와 람다
```cpp
auto derefUPLess =
  [](const std::unique_ptr<Widget>& p1,
  const std::unique_ptr<Widget>& p2)
  { return *p1 < *p2; };
```
위와 같이 람다로 변수를 초기화 할 수도 있다.

```cpp
auto derefLess =
  [](const auto& p1,
  const auto& p2)
  { return *p1 < *pe; };
```
C++14에는 위와 같이 람다 매개변수에도 auto를 사용할 수 있다.

> std::function
C++11 표준 라이브러리 템플릿이다. 
함수 포인터 개념을 일반화한 것이다.
함수 포인터는 함수만 가리키지만 std::function은 호출만 가능하면 사용할 수 있다.
함수 포인터가 함수의 형식을 지정해야 하는 것처럼 std::function도 함수의 형식을 지정해야 한다.

```cpp
// 함수의 서명 = 함수의 형식
bool(const std::unique_ptr<Widget>&,
  const std::unique_ptr<Widget>&)
```
위 코드 함수 형식에 해당하는 std::function은 아래와 같다.
```cpp
std::function<bool(const std::unique_ptr<Widget>&,
  const std::unique_ptr<Widget>&)> func;
```
auto를 사용할 때가 더 간결한 것을 볼 수 있다.

> auto와 메모리
auto로 선언된 클로저를 담는 변수는 클로저와 같은 형식이다.
따라서 클로저에서 요구하는 만큼 메모리를 사용한다.
하지만 std::function으로 사용할 경우 





































## 질문들
> 클로저 형식이란?
