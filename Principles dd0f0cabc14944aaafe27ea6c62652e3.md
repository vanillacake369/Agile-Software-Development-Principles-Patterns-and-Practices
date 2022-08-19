# Principles

# 설계 악취

---

- 경직성 : 단순한 방법으로도 소프트웨어를 변경하기 어려운 경향
- 취약성 : 한 군데를 변경했을 때 프로그램의 많은 부분이 잘못되는 경향
- 부동성 : 다른 시스템에서 유용하게 쓸 수 있는 부분을 포함하고 있지만, 그런 부분을 원래 시스템에서 분리하는 수고와 위험성이 지나치게 클 때 설계는 움직이게 할 수 없음
- 점착성 : 소프트웨어의 설계를 유지하기 어려울 때
    - 설계의 점착성 : 설계 유지 방법이 엉터리 방법보다 사용하기 어려울 때
    → 잘못된 동작을 하기는 쉽지만, 옳은 동작을 하기는 어렵다.
    - 환경의 점착성 : 개발환경이 느리고 비효율적일 때  발생
- 불필요한 복잡성 : 현재 시점에서 유용하지 않은 요소가 설계에 포함되어 있을 때 발생
    
    → 개발자가 요구사항에 대한 변경을 미리 예상하고, 잠재적인 변경을 처리하기 위해 소프트웨어에 기능을 집어넣을 때 발생함.
    
- 불필요한 반복 : ‘복사하기’와 ‘붙여넣기’를 통해 소프트웨어 시스템이 반복된 코드 요소로 구성될 때
→ 시스템에 반복되는 코드가 존재할 때, 시스템을 변경하는 일은 고될 수 있음.
- 불투명성 : 모듈을 이해하기 어려운 경향
→ 개발자는 읽는 사람의입장에서 생각하고 코드를 명료하고 표현적으로 유지하려는 지속적인 노력이 필요하다.

# Copy Program

---

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled.png)

## ver1

---

```cpp
void Copy()
{
 int c;
 while ((c=RdKbd()) != EOF)
 WrtPrt(c);
}
```

## ver2

---

```cpp
bool ptFlag = false;
// remember to reset this flag
void Copy()
{
 int c;
 while ((c=(ptflag ? RdPt() : RdKbd())) != EOF)
 WrtPrt(c);
}
```

## ver3

---

```cpp
bool ptFlag = false;
bool punchFlag = false;
// remember to reset these flags
void Copy()
{
 int c;
 while ((c=(ptflag ? RdPt() : RdKbd())) != EOF)
 punchFlag ? WrtPunch(c) : WrtPrt(c);
}
```

## ver4

---

```cpp
class Reader
{
 public:
 virtual int read() = 0;
};
class KeyboardReader : public Reader
{
 public:
 virtual int read() {return RdKbd();}
};

KeyboardReader GdefaultReader;

void Copy(Reader& reader = GdefaultReader)
{
 int c;
 while ((c=reader.read()) != EOF)
 WrtPrt(c);
}
```

- 요구사항은 언제나 바뀐다. 요구사항 변경으로 설계가 퇴화된다면, 애자일 방식대로 하고 있지 않은 것이다.
- Agile 팀은 OCP에 따라 모듈을 수정하지 않고도 설계를 확장할 수 있게 설계
→ 객체지향 설계의 기본적인 주의(tenet)
    - 기초 설계는 KeyboardReader,PrintWriter에 직접 의존하게 되는, 의존성의 방향으로 인해 유연하지 못 했음
        - 거꾸로 뒤집어 더 이상 입력 장치에 의존하지 않도록 설계
        - 스트래터지 패턴을 사용해 원하는 역전 작업을 처리
1. 애자일 실천 방법으로 문제 찾아냄
2. 설계 원칙 적용하여 문제 진단
3. 적절한 디자인 패턴 적용 문제 해결

# SRP (Separating Coupled Responsibilities)

---

> 한 클래스는 단 한 가지의 변경 이유만을 가져야 한다.
> 

## Geometry GUI

---

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%201.png)

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%202.png)

## Modem Interface

---

```cpp
interface Modem
{
 public void dial(String pno);
 public void hangup();
 public void send(char c);
 public char recv();
}
```

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%203.png)

- 서로 다른 시간에 두 가지 책임의 변경을 유발하는 방식으로 바뀌지 않는 다면, 이들을 분리할 필요는 없다. 이런 경우 분리하면, 오히려 불필요한 복잡성이란 악취를 풍기게 할 것임!
→ 동시에 변경할 필요성이 있는 경우 분리!

## Persistence

---

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%204.png)

- 업무 규칙(Business Rules)인 CalculatePay()
- 영속성 제어**(Persistence)인 Store()
    
    ** *같은 상태가 오래 계속되는 성질*
    

업무 규칙은 자주 바뀌는 경향이 있고 영속성은 자주 바뀌지 않는 경향이 있고, 바뀌는 이유도 완전히 다름. 따라서 이 둘을 묶는 것은 문제를 부르는 것이나 다름 없음!

# OCP (Open-Closed Pattern)

---

> 소프트웨어 개체(클래스,모듈,함수 등)는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀있어야 한다.
> 

## Strategy Pattern

---

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%205.png)

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%206.png)

- 모듈은 추상화를 조작할 수 있다. 이런 모듈은 고정된 추상화에 의존하기 때문에 수정에 대해 닫혀있을 수 있다. 그 모듈의 행위는 추상화의 새 파생 클래스들을 만듦으로써 확장이 가능하다.
- Client클래스는 추상화를 사용하지만, 파생 Server클래스의 객체를 사용할 것이다. Client 객체가 다른 서버 클래스를 사용하게 하려면, ClientInterface클래스의 세 파생 클래스를 생성하면 된다.

## Template Method Pattern

---

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%207.png)

- 추상 인터페이스가 Policy 클래스 자체의 한 부분
- Policy 내부에 명시된 행위는 Policy 클래스의 새로운 파생 클래스를 생성함으로써 확장되거나 수정될 수 있다.

## Shape Class & DrawAllShapes()

---

### 9-1

---

```cpp
--shape.h---------------------------------------
enum ShapeType {circle, square};
struct Shape
{
	ShapeType itsType;
};
--circle.h---------------------------------------
struct Circle
{
	ShapeType itsType;
	double itsRadius;
	Point itsCenter;
};
void DrawCircle(struct Circle*);
--square.h---------------------------------------
struct Square
{
	ShapeType itsType;
	double itsSide;
	Point itsTopLeft;
};
void DrawSquare(struct Square*);
--drawAllShapes.cc-------------------------------
typedef struct Shape *ShapePointer;
void DrawAllShapes(ShapePointer list[], int n)
{
	int i;
	for (i=0; i<n; i++)
	{
		struct Shape* s = list[i];
		switch (s->itsType)
		{
			case square:
				DrawSquare((struct Square*)s);
				break;
			case circle:
				DrawCircle((struct Circle*)s);
				break;
		}
	}
}
```

- 새로운 도형을 추가하려면…
    - enum ShapeType에 추가
    - 새로운 도형 클래스 정의
    - DrawAllShapes( ) 수정
- OCP 위반
    - 경직성
    - 불투명성
    - 취약성
    - 부동성

### 9-2

---

```cpp
class Shape
{
	public:
	virtual void Draw() const = 0;
};
class Square : public Shape
{
	public:
	virtual void Draw() const;
};
class Circle : public Shape
{
	public:
	virtual void Draw() const;
};
void DrawAllShapes(vector<Shape*>& list)
{
	vector<Shape*>::iterator i;
	for (i=list.begin(); i != list.end(); i++)
		(*i)->Draw();
}
```

- 변경에 대해 보호할 수 있는 추상화를 작성해야 한다.
- OCP를 따르자면 비용이 많이 든다.
- 추상화 시기를 적절히 선택해야 한다.
- 테스트를 먼저 작성한다.
- 아주 짧은 주기로 개발한다.
- 기반구조보다 기능요소를 먼저 개발한다.
- 가장 중요한 요소를 먼저 개발한다.
- 소프트웨어를 빠른 시간 내에 자주 릴리즈한다.

**그러나 Circle과 Square 간 draw() 순서 변경에 대해서는 닫혀 있지 않다.**

### 9-3,4,5

---

```cpp
class Shape
{
	public:
		virtual void Draw() const = 0;
		virtual bool Precedes(const Shape&) const = 0;
		bool operator<(const Shape& s) {return Precedes(s);}
};
```

```cpp
template <typename P>
class Lessp // utility for sorting containers of pointers.
{
	public:
	bool operator()(const P p, const P q) {return (*p) < (*q);}
};
void DrawAllShapes(vector<Shape*>& list)
{
	vector<Shape*> orderedList = list;
	sort(orderedList.begin(),	orderedList.end(),	Lessp<Shape*>());
	vector<Shape*>::const_iterator i;
	for (i=orderedList.begin(); i != orderedList.end(); i++)
		(*i)->Draw();
}
```

```cpp
bool Circle::Precedes(const Shape& s) const
{
	if (dynamic_cast<Square*>(s))
		return true;
	else
		return false;
}
```

- 추상화를 통해 DrawAllShapes를 순서에 대해 닫으려 함.
- Shape의 새로운 파생 클래스 생성될 때마다, 모든 Precedes() 함수가 변경되어야 함
→ OCP 위반!

### 9-6

---

```cpp
#include <typeinfo>
#include <string>
#include <iostream>

using namespace std;

class Shape
{
	public:
		virtual void Draw() const = 0;
		bool Precedes(const Shape&) const;
		bool operator<(const Shape& s) const
		{return Precedes(s);}
	private:
		static const char* typeOrderTable[];
};

const char* Shape::typeOrderTable[] =
{
	typeid(Circle).name(),
	typeid(Square).name(),
	0
};

// This function searches a table for the class names.
// The table defines the order in which the
// shapes are to be drawn. Shapes that are not
// found always precede shapes that are found.
//
bool Shape::Precedes(const Shape& s) const
{
	const char* thisType = typeid(*this).name();
	const char* argType = typeid(s).name();
	bool done = false;
	int thisOrd = -1;
	int argOrd = -1;
	for (int i=0; !done; i++)
	{
		const char* tableEntry = typeOrderTable[i];
		if (tableEntry != 0)
		{
			if (strcmp(tableEntry, thisType) == 0)
				thisOrd = i;
			if (strcmp(tableEntry, argType) == 0)
				argOrd = i;
			if ((argOrd >= 0) && (thisOrd >= 0))
				done = true;
		}
		else // table entry == 0
			done = true;
	}
	return thisOrd < argOrd;
}
```

- 테이블 주도적 접근방식(Table-Driven Approach)
    - 테이블은 그려질 도형의 순서를 정의
    - 테이블에서 클래스 이름을 찾는다.
        - tableEntry = typeOrderTable[ ]
- 이를 통해 순서정책 변경 시, 테이블에 대해서만 수정을 가해주면 됨.
- Precedes()는 변경에 대해 닫혀있게 된다.

# LSP (Liskov Substitution Principle)

---

> 서브타입(subtype)은 그것의 기반 타입(base type)으로 치환 가능해야한다.
> 

> 타입 S의 각 객체 o1과 타입 T의 각 객체 o2 가 있을 때, T로 프로그램 P를 정의했음에도 불구하고 o2를 o1으로 치환하여도 P의 행위가 변하지 않으면, S는 T의 서브타입이다
> 

## LSP위반은 곧 OCP위반이다

---

### 10-1

---

```cpp
struct Point {double x,y;};
struct Shape {
	enum ShapeType {square, circle} itsType;
	Shape(ShapeType t) : itsType(t) {}
};
struct Circle : public Shape
{
	Circle() : Shape(circle) {};
	void Draw() const;
	Point itsCenter;
	double itsRadius;
};
struct Square : public Shape
{
	Square() : Shape(square) {};
	void Draw() const;
	Point itsTopLeft;
	double itsSide;
};
void DrawShape(const Shape& s)
{
	if (s.itsType == Shape::square)
		static_cast<const Square&>(s).Draw();
	else if (s.itsType == Shape::circle)
		static_cast<const Circle&>(s).Draw();
}
```

- 상속은 사용하지만, 다형성은 사용하지 않음((오버라이딩 하지 않음)
- Circle,Square가 Shape를 대체할 수 없음. DrawShape는 인자로 받는 Shape를 검사 이후, 조건에 따른 Draw함수 호출(if-else,switch-case)
→ OCP 위반

**LSP 위반은 곧 잠재적인 OCP위반이다.**

## Rect Class & Square Class

---

![Untitled](Principles%20dd0f0cabc14944aaafe27ea6c62652e3/Untitled%208.png)

### 10-2 & f()

---

```cpp
class Rectangle
{
	public:
		void SetWidth(double w) {itsWidth=w;}
		void SetHeight(double h) {itsHeight=w;}
		double GetHeight() const {return itsHeight;}
		double GetWidth() const {return itsWidth;}
	private:
		Point itsTopLeft;
		double itsWidth;
		double itsHeight;
};

void Square::SetWidth(double w)
{
	Rectangle::SetWidth(w);
	Rectangle::SetHeight(w);
}
void Square::SetHeight(double h)
{
	Rectangle::SetHeight(h);
	Rectangle::SetWidth(h);
}

Square s;
s.SetWidth(1); // Fortunately sets the height to 1 too.
s.SetHeight(2); // sets width and height to 2. Good thing.
```

```cpp
void f(Rectangle& r)
{
	r.SetWidth(32); // calls Rectangle::SetWidth
}
```

- 만갹 어떤 Square 객체에 대한 참조값을 이 함수에 넘겨준다면, 그 Square 객체는 세로 값이 가로 값에 맞춰 바뀌지 않기 때문에 문제가 생길 것이다.
→ LSP 위반

### 10-3 & g()

---

```cpp
class Rectangle
{
	public:
		virtual void SetWidth(double w) {itsWidth=w;}
		virtual void SetHeight(double h) {itsHeight=h;}
		double GetHeight() const {return itsHeight;}
		double GetWidth() const {return itsWidth;}
		private:
		Point itsTopLeft
		double itsHeight;
		double itsWidth;
};
class Square : public Rectangle
{
	public:
		virtual void SetWidth(double w);
		virtual void SetHeight(double h);
};
void Square::SetWidth(double w)
{
	Rectangle::SetWidth(w);
	Rectangle::SetHeight(w);
}
void Square::SetHeight(double h)
{
	Rectangle::SetHeight(h);
	Rectangle::SetWidth(h);
}
```

```cpp
void g(Rectangle& r)
{
	r.SetWidth(5);
	r.SetHeight(4);
	assert(r.Area() == 20);
}
```

- virtual, 즉 추상화를 통해 상속관계에서의 SetWidth(),SetHeight()를 닫아버림
→ 자체 모순이 없는 설계
**→ But, 사용자에 대해 모순 존재**
- 스퀘어 객체가 매개변수로 넘어오면 오동작: (4 X 4)
- 스퀘어가 렉탱글로 치환되지 않음
- LSP 위반

**모델만 별개로 보고, 그 모델의 유효성을 충분히 검증할 수 없다. 어떤 모델의 유효성(validity)은 오직 그 고객의 관점에서만 표현될 수 있다.**

### IS-A 관계와 행위에 대하여

---

**‘IS-A’는 행위에 대한 것이다**

- 행위적 관점에서는 스퀘어 객체는 렉탱글 객체가 아니다.
- 스퀘어와 렉탱글은 is-a관계가 아니다.
- 객체 모델링의 관점에서는 스퀘어는 렉탱글과 상속관계에 있지 않다

## DBC(Design By Contract)

---

### DBC를 사용하면…

- 사용자는 클래스의 계약 사항을 명시적으로 설정.
- 이 클래스를 사용하는 작성자가 신뢰할 수 있는 행위에 대해서 알려줌.
- 각 메소드의 사전조건과 사후조건을 선언하는 것으로 계약을 구체화됨.
- 메소드를 실행하기 위해서는 사전조건이 참이 되어야 함.
- 메소드는 완료되면 사후조건이 참이 됨을 보장함.

### 파생 크래스 사전조건,사후조건 규칙

> “파생 클래스에서 루틴 재선언은 오직 원래의 사전조건과 같거나 더 약한 수준에서 그것을 대체
할 수 있고, 원래의 사후조건과 같거나 더 강한수준에서 그것을 대체할 수 있다
> 
- 파생된 객체는 기반 클래스가 받아들일 수 있는것은 모두 받아들여야 한다. (파생클래스의 사전
조건은 기반클래스의 사전조건보다 약하다.)
- 파생 클래스는 기반 클래스의 모든 사후조건을 따라야 한다. (파생클래스의 사후조건은 기반 클
래스의 사후조건보다 강하다.)

### DBC에 의하면 Rectangle, Square는…

**Rectangle::SetWidth(double w)**

```cpp
assert((itsWidth == w) && (itsHeight == old.itsHeight));
```

**Square::SetWidth(double w)**

```cpp
assert(itsWidth == w);

```

- (itsHeight == old.itsHeight) 조건을 강제하지 않음.
- 사후조건이 더 약함 → 계약 위반
- (itsWidth == itsHeight) → (itsHeight == old.itsHeight)과 충돌