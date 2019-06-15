## Move Constructer (이동 생성자)

```cpp
class_name(class_name &&)             // 기본
class_name(class_name &&) = default;  // 강제로 컴파일러가 이동 생성자를 만들도록
class_name(class_name &&) = delete;   // 암시적 이동생성자 생성을 막는다
```

이동 생성자는 rvalue에 의해 초기화 될때 불립니다.




## 질문
* 컨테이너를 가지고 있지 않은 클래스의 이동 생성자는 복사 생성자와 차이가 없는가?

## 링크
> 이동 생성자
* https://modoocode.com/227
