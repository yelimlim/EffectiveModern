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

> auto와 형식 1 - 컴파일러만 아는 
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

> auto와 std::function

C++11 표준 라이브러리 템플릿이다. 
함수 포인터 개념을 일반화한 것이다.
함수 포인터는 함수만 가리키지만 std::function은 호출만 가능하면 사용할 수 있다.
함수 포인터가 함수의 형식을 지정해야 하는 것처럼 std::function도 함수의 형식을 지정해야 한다.

* 간결함

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

* 메모리

auto로 선언된 클로저를 담는 변수는 클로저와 같은 형식이다.
따라서 클로저에서 요구하는 만큼 메모리를 사용한다.
하지만 std::function으로 사용할 경우 고정된 std::function 탬플릿 인스턴스 크기보다 클로저의 크기가 크면 힙 메모리를 할당해서 클로저를 저장한다.
대부분의 경우 auto를 사용하는 것보다 std::function을 사용할 때 더 많은 메모리를 차지하게 된다.

* 속도

std::function은 인라인화를 제한하고 간접 함수 호출을 산출하기 때문에 auto보다 호출 속도가 느리다.

> auto와 형식2 - 형식 불일치  예방

* case 1

``` cpp
std::vector<int> v;
...
unsigned sz = v.size();
```
위 코드에서 v.size()의 반환 값은 std::vector<int>::size_type이다.
32비트 Window에서는 unsigned도 32비트이고 std::vector<int>::size_type도 32비트이다.
하지만 64비트 Window에서는 unsigned는 32비트이지만 std::vector<int>::size_type은 64비트이다.
auto를 사용하면 이러한 문제를 피할 수 있다.
```cpp
auto sz = v.size();
```

* case 2

```cpp
std::unorderd_map<std::string,int> m;
...
for(const std::par<std::string, int>& p : m)
{...}
```
위 코드는 한 가지 문제점이 있다.
std::unorderd_map는 키로 선언한 형식을 내부적으로 const으로 바꾸어 정의한다.
따라서 해시테이블(std::unorderd_map)에 저장된 형식은 std::pair<const std::string, int>이다.
위 코드를 실행하면 루프를 돌 때마다 std::pair<const std::string, int>를 std::pair<std::string, int>로 변환하기 위해 추가 비용이 들 것이다.
auto를 사용하면 이러한 문제를 피할 수 있다.
```cpp
for(const auto& p : m)
{...}
```
이렇게 auto를 사용하면 m 안의 한 요소를 가리키는 포인터를 얻게 된다.
바꾸기 전 코드는 m 안의 한 요소가 아닌 만들어진 임시 객체를 가리키고 있다.

> auto와 refatoring
* 초기화 표현식의 형식이 변하면 auto의 형식은 자동으로 변한다.
* refactoring이 한결 수월해진다.

> auto의 문제점

* 형식 연역 관련
  * auto 변수의 형식은 변수 초기화에 쓰이는 표현식으로부터 연역된다.
  * 예상과 다른 형식으로 연역 될 수도 있다.

* 가독성에 관한 문제
  * 해결책
    * IDE 기능 활용
    * 이름 잘 짓기

## 질문들
> 클로저 형식이란?
* std::function 객체(?)

> std::function - 인라인?
* std::function은 호출 가능한 모든 것을 포함한다.
* 따라서 컴파일 타임에 std::function으로 무엇을 저장할지 알 수 없다.

> std::function - 간접 함수 호출?
* https://challenger100.wordpress.com/2016/03/18/stdfunction%EC%9D%80-%EC%95%84%EC%A3%BC-%EB%8A%90%EB%A6%AC%EB%8B%A4-%EB%9E%8C%EB%8B%A4%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90/
* https://probablydance.com/2013/01/13/a-faster-implementation-of-stdfunction/
* https://blog.demofox.org/2015/02/25/avoiding-the-performance-hazzards-of-stdfunction/
* 뭥가... 람다를 std::function으로 받으면 람다는 코드와 캡처를 포함하는 클래스를 생성하기 때문에
* std::function은 해당 객체의 타입을 알 수 없고 그래서 가상 함수를 호출하게 되는데 이 때 시간이 좀 걸린다.

> 형식 추론과 형식 연역
* http://occamsrazr.net/tt/311
* 프로그래밍 세계에서 다른 모든 언어는 형식 추론 이라는 말을 사용하는데 c++은 형식 연역이라는 말을 사용한다.
* 연역 - 일반적인 것에서 구체적인 것으로
* 추론(귀납) - 구체적인 것에서 일반적인 것으로
