## 31.기본 Capture mode를 피하라

> default capture mode 종류
* by refrerence
* by value

> reference capture mode 문제점
* dangling의 위험성이 있다.
  * dangling 예시
```cpp
using FilterContainer = std::vector<std::function<bool(int)>>;
FilterContainer filters;

void func()
{
  // 5의 배수를 선별하는 필터 추가
  filter.emplace_back(
    [](int value) { return value % 5 == 0; } // ok!
  )
}

// 특정 수의 배수를 선별하는 필터 추가하는 함수
void addDivisorFilter()
{
  auto calc1 = computeSomeValue1();
  auto calc2 = computeSomeValue2();
  
  auto divisor = computeDivisor(calc1, calc2);
  
  filter.emplace_back(
    [&](int value) { return vlaue % divisor == 0; } // error! - divisor 변수가 이 함수를 벗어나면 유지 되지 않는다. 
  );
  
  filter.emplace_back(
    [&divisor](int value) { return vlaue % divisor == 0; } // error! - 이렇게라도 명시적으로 작성하면 divisor의 수명을 조심할 수는 있다.
  );
}
```

> value capture mode 문제점
* 
