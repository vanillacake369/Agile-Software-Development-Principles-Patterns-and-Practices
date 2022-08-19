# Bowling Game

# 볼링게임 규칙을 먼저 이해해보자

![Untitled](Bowling%20Game%2043113191dd22445baf440b9137dfddbd/Untitled.png)

1. (1+4) = 5
2. 5 + (4+5) = 14
3. 스페어
→ 다음 공을 굴릴 때까지 스코어 계산 X
4. *첫 번째 공으로 5개의 핀
→ 세 번째 프레임(스페어) 계산*
: "이전 프레임까지의 스코어(14)" + "10"+ "다음 1개의 공으로 쓰러뜨린 핀의 개수(5)” = 29
두 번째 공으로 스페어
→ 다음 공을 굴릴 때까지 스코어 계산 X
5. 스트라이크
→ *네 번째 프레임(스페어) 계산*
: "이전 프레임까지의 스코어(29)" + "10"+ "다음 1개의 공으로 쓰러뜨린 핀의 개수(10)” = 49
6. 0 / 1 
*→ 다섯 번째 프레임(스트라이크) 계산*
: "이전 프레임까지의 스코어(49)" + "10"+ "다음 2개의 공으로 쓰러뜨린 핀의 개수(1)” = 60
**

# 요구사항이 무엇인가

---

볼링 리그에 대한 기록을 관리하는 애플리케이션

- 모든 게임의 결과를 저장
- 팀의 순위를 결정
- 매주 시합의 승자와 패자를 결정
- 각 게임의 스코어를 기록

# 입력, 출력 결정

---

- 입력: 10개 프레임의 ‘투구’ (공)
- ‘투구’—쓰러뜨린 핀의 갯수
- 출력: 각 프레임의 스코어

# 인수테스트 함수부터 작성

---

- **테스트 데이터**

![Untitled](Bowling%20Game%2043113191dd22445baf440b9137dfddbd/Untitled.png)

- **인수테스트**

```java
throwBall(6); // ‘투구'를 더하기 위해 호출할 함수 throwBall()
throwBall(3);
assertEquals(9, getScore()); // 스코어를 얻는 함수 getScore()
```

# 시스템 설계

---

- **문제영역 개념 설계/분석 (UML)**

![Untitled](Bowling%20Game%2043113191dd22445baf440b9137dfddbd/Untitled%201.png)

> *OK, clearly a Game object consists of a sequence of ten frames. Each Frame object contains one, two, or three throws.

분명히 Game객체는 프레임 10개의 나열로 이루어져 있지. 각 Frame 객체는 한 번이나 두번, 또는 세 번의 ‘투구’를 포함하고 있지.*
> 
- 볼링게임: Game
- 프레임: Frame
- 투구: Throw

즉 문제영역을 분석/설계 해보니 각 객체 간 의존관계를 나타내고 있다. 

( 의존성 사슬. dependency chain )

- 이럴 때는 의존성 사슬의 끝부터 시작해 거꾸로 작업하는 것이 편하다
    
    > RSK : *Well, pick a class … any class. Shall we start at the end of the dependency chain and work backwards? That will make testing easier.*
    
    RCM : *Sure, why not. Let’s create a test case for the Throw class.*
    > 
    
    [의존관계에 관해 더 보고 싶다면 여기를 참고해보자.](https://maro-matta.tistory.com/entry/%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%EC%9D%98%EC%A1%B4%EC%84%B1%EC%9D%B4-%EB%AD%90%EC%9E%84)
    

~~라고 해놓고 나중에가서 *“Topdown방식으로 진행했어야 했어!! Game부터 시작할 걸…”* 이러고 있다.~~ 

의존성 사슬의 끝인 Throw객체부터 작업해보자.

# Throw 클래스의 테스트부터 작성

---

플레이어가 넘어뜨린 핀의 수를 저장하는 테스트 작성

`TestThrow1`

```java
// TestFrame.java-------------------------------------
import junit.framework.TestCase;

public class TestThrow extends TestCase{
	public TestThrow(String name){
		super(name);
	}
	// public void test???
}
```

Throw객체가 플레이어가 넘어뜨린 핀의 수를 저장, 즉 어떠한 행위를 하지 않음
-> Frame으로 넘어가 Throw를 마무리할 수 있게하는 테스트 클래스 작성

`TestFrame1`

```java
// TestFrame.java-------------------------------------
import junit.framework.TestCase;
public class TestFrame extends TestCase{
	public TestFrame(String name){
		super(name);
	}
	// public void test???
}
```

# Frame 작성

---

## Frame의 정의

---

Frame은 스코어를 제공할 수 있는 객체이다.

Frame은 다음과 같은 행동을 한다.

- 스코어를 제공
    - 각 투구에서 핀수를 합함
    - 스트라이크, 스페어 등을 고려

## Frame에 대한 TDD

---

## 1차 f.getScore() 테스트

---

`TestFrame2`

```java
// TestFrame.java-------------------------------------
import junit.framework.TestCase;

public class TestFrame extends TestCase{
	public TestFrame(String name){
		super(name);
	}
	public void testScoreNoThrows(){
		Frame f = new Frame(); // Frame 생성
		assertEquals(0,f.getScore()); // 프레임의 점수를 얻음을 확인
	}
}
```

`Frame1`

```java
// Frame.java-----------------------------------------
public class Frame {
	public int getScore() { // 프레임의 점수를 얻음
		return 0;
	}
}
```

투구를 더하게 된다면, `return 0;`로 인해 getScore( )에 대한 테스트는 실패하게 된다.

투구를 더하고( `add()` ), 이후 스코어를 확인하는 테스트를 작성하자.

## 2차 add() 이후 f.getScore() 테스트

---

`TestFrame3`

```java
// TestFrame.java-------------------------------------
import junit.framework.TestCase;

public class TestFrame extends TestCase{
	public TestFrame(String name){
		super(name);
	}
	public void testAddOneThrow(){
		Frame f = new Frame();
		f.add(5); // 투구에서 쓰러뜨린 핀수를 더함
		assertEquals(5, f.getScore()); // 프레임의 스코어가 5로 제대로 나오는 지를 테스트
	}
}
```

`Frame2`

```java
// Frame.java-----------------------------------------
public class Frame {
	public int getScore() {
		return 0;
	}
	public void add(Throw t){

	}
}
```

테스트는 정수를 넘겨주고 //`f.add(5);`

메소드는 Throw 객체를 기대하고 있다. // `add(Throw t) { .. }`

여기서는 투구를 객체로 보기 보다는 실제적인 핀수를 받기로 한다.

따라서 add()의 매개변수를 Throw에서 int로 바꿔주도록 한다.

## 3차 add()의 매개변수 타잎 변경

---

`TestFrame3`

```java
// TestFrame.java-------------------------------------
import junit.framework.TestCase;

public class TestFrame extends TestCase{
	public TestFrame(String name){
		super(name);
	}
	public void testAddOneThrow(){
		Frame f = new Frame();
		f.add(5);
		assertEquals(5,f.getScore());
	}
}
```

`Frame3`

```java
// Frame.java-----------------------------------------
public class Frame {
	public int getScore() {
		return 0;
	}
	public void add(int t){

	}
}
```

## 4차 Frame 작성

---

원하는 Frame의 모습을 갖추었다.

이제 제대로 동작되는(테스트 가능한) Frame을 작성해보자.

`Frame4`

```java
// Frame.java --------------------------------------------
public class Frame{
	public int getScore() {
		return itsScore;
	}
	public void add(int pins) {
		itsScore += pins;
	}
	private int itsScore = 0;
}
```

# 경계값, Strike, Spare 처리

---

다음 세 가지 문제에 대한 처리를 해주어야 한다.

1. Frame.add()를 11로 호출하게 된다면?
→ pins 변수에 대한 경계값을 0~10으로 두고, 예외처리에 대한 코드를 이후에 작성해준다.
2. `add(10);` 에 대하여 getScore()는?
3. `add(3);` `add(7);`에 대하여 getScore()는?

1번 문제는 경계값과 예외처리를 통해 해결해주었다.

2번,3번에 대해서는 다음과 같이 접근해야만 한다.

- 스트라이크/스페어 처리 위해서 다음 Frame 인스턴스에 대해 확인
- 점수를 구하여 경우에 따른 계산

Game에서 Frame을 만들고, 각각을 연결리스트 형태로 연결하게하면 된다.

따라서 이제부터는 Game에 대한 테스트 케이스를 작성해보도록 한다.

# TDD를 사용한 Game

---

## 1차 Game에 5개의 Frame 연결리스트 필요한지 확인

---

`TestGame1`

```java
// TestGame.java-------------------------------------
import junit.framework.TestCase;

public class TestGame extends TestCase{
	public TestGame(String name){
		super(name);
	}
	public void testOneThrow() {
		Game g = new Game(); // Game -> Frame
		g.add(5); // 5 LinkedList of Frame
		assertEquals(5,g.score());
	}
}
```

`Game1`

```java
// Game.java --------------------------------------------
public class Game {
	public int score() {
		return 0;
	}
	public void add(int pins) {
	}
}
```

위 Game1 코드를 테스트 통과하도록 만든다

`Game2`

```java
// Game.java --------------------------------------------
public class Game {
	public int score() {
		return itsScore;
	}
	public void add(int pins) {
		itsScore += pins;
	}
	private int itsScore = 0;
}
```

## 2차 Game에 대한 스페어 없는 투구 2개 테스트

---

`TestGame2`

```java
//TestGame.java------------------------------------------
public void testTwoThrowsNoMark()
{
	Game g = new Game();
	g.add(5);
	g.add(4);
	assertEquals(9, g.score());
}
```

## 3차 Game에 대한 스페어 없는 투구 2개 테스트

---

`TestGame3`

```java
//TestGame.java------------------------------------------
public void testFourThrowsNoMark()
{
	Game g = new Game();
	// frame1
	g.add(5);
	g.add(4);
	// frame2
	g.add(7);
	g.add(2);

	assertEquals(18, g.score());
	assertEquals(9, g.scoreForFrame(1));	// frame1 score : 9
	assertEquals(18, g.scoreForFrame(2)); 	// frame2 score : 18
}
```

# 프레임마다 스코어를 보여줄 ScoreForFrame()도입

---

위 3차를 보다싶이, 각 프레임 이후의 점수를 확인할 수 있는 메서드가 필요하다.

이를 위해 ScoreForFrame()를 작성해보자.

## TestGame3의 scoreForFrame()에 대해 컴파일 가능하게 만들기

---

Game에 scoreForFrame( )을 추가한다.

`Game3`

```java
//Game.java----------------------------------
public int scoreForFrame(int frame)
{
 return 0;
}
```

# 테스트 통과할 수 있게 Game 작성

---

## 속성

---

- `itsCurrentThrow`
    
    : 다음에 던질 투구를 가르키는 인덱스
    
- `itsScore`
: 총 점수
- `int itsThrows[21];`
: 투구 수 저장 배열
*// 21은 경기 가능 최대 투구수*

## 메소드

---

- `score()`
: 현재 스코어 반환
- `scoreForFrame(int frame)`
: 입력 frame 까지의 점수를 구함 (호출 시, 배열을 순회하면서 스코어 계산)
- `add()` 
: 호출마다 배열에 새 정수 추가

`Game4`

```java
//Game.java----------------------------------
public class Game
{
 public int score()
 {
	 return itsScore;
 }

 public void add(int pins) // 배열에 새 정수 추가
 { 
	 itsThrows[itsCurrentThrow++]=pins; 
		//itsCurrentThrow++ -> 다음 투구 핀수 를 저장할 위치를 유지(++)
	 itsScore += pins;
		// itsScore에 핀수를 누적
 }

 public int scoreForFrame(int frame) // 배열을 순회하면서 스코어 계산
 { 
	 int score = 0;
	 ***for (int ball=0; frame>0 && (ball<itsCurrentThrow); ball+=2,frame--) {***
		 score += itsThrows[ball] + itsThrows[ball+1];
			// 스트라이크와 스페어의 경우는 일단은 보류
			// 한 프레임의 점수는 두 번의 투구에서의 핀수의 합
	 }
	 return score;
 }

 private int itsScore = 0; // 핀수를 누적
 private int[] itsThrows = new int[21]; // 투구 핀수 pins를 저장
 private int itsCurrentThrow = 0;// 다음 투구 핀수를 저장할 위치를 유지(add()에서 ++ 사용)
}
```

## ~~SRP 위반~~

---

~~지금 Game은 SRP를 위반하고 있다.~~

~~Game~~

- ~~투구를 받아서 총점을 계산~~
- ~~각 프레임의 스코어를 기록~~

스코어를 기록하는 것이 제대로 동작하게끔 한 뒤에, 원칙적용해볼 것.

→ 우선은 scoreForFrame()의 루프를 단순화해보자.

## scoreForFrame() 단순화 리팩토링

---

`Game5`

```java
public int scoreForFrame(int theFrame)
{
	 int ball = 0;
	 int score=0;
	 ***for (int currentFrame = 0; currentFrame < theFrame; currentFrame++)***
	 {
		 score += itsThrows[ball++] + itsThrows[ball++];
	 }
	 return score;
}
```

- ***+= 표현식에는 부작용 존재***
→ 컴파일러에 따라 +=와 ++의 연산우선순서가 다르므로 오류 발생 가능 있음.

여기서 잠재적인 순서 의존성(둘 중 어느 것을 먼저 계산하게 될 지 모름)을 없애보자.

## +=,++ 표현의 순서의존성 제거

---

`Game6`

```java
public int scoreForFrame(int theFrame)
{
	 int ball = 0;
	 int score=0;
	 for (int currentFrame = 0; currentFrame < theFrame; currentFrame++)
	 {
		 int firstThrow = itsThrows[ball++]; // 첫 번째 투구 계산
		 int secondThrow = itsThrows[ball++]; // 두 번째 투구 계산
		 score += firstThrow + secondThrow; // 프레임의 스코어 계산
	 }
	 return score;
}
```

## Game객체 생성을 반복 하는 문제 해결

---

`TestGame4`

```java
public void testSimpleSpare()
{
	Game g = new Game();
}
```

- 각 테스트 함수에서 Game객체 생성을 반복 하는 문제가 발생한다. 이를 다음을 통해 해결해본다.
    - Game 객체를 속성으로…
    - setUp(): 게임 객체를 생성하고 저장

`TestGame5`

```java
// Game g = new Game(); 
private Game g; // Game 객체를 속성으로
	
public void setUp()
{
	g = new Game();
}
```

## Spare 테스트 케이스 작성

---

일반적인 케이스 처리와 각 프레임에 대한 스코어는 마무리되었다.

이제 스페어 테스트 케이스 작성해보자.

`TestGame6`

```java
public void testSimpleSpare()
	{
		 g.add(3);
		 g.add(7); // 스페어
		 g.add(3); // 1차 -> 이전스코어(0) + 10 + 1차(3) = 13
		 assertEquals(13, g.scoreForFrame(1));
	}
```

그렇다면 이를 통과하게 만들어보자.

`Game7`

```java
public int scoreForFrame(int theFrame)
{
	int ball = 0;
	int score=0;

	for (int currentFrame = 0; currentFrame < theFrame;	currentFrame++)
	{
	 int firstThrow = itsThrows[ball++]; // 첫 번째 투구 계산
	 int secondThrow = itsThrows[ball++]; // 두 번째 투구 계산
	 int frameScore = firstThrow + secondThrow; // 두 투구를 더한 값

	 // 스페어인 경우, 다음 프레임의 첫 번째 투구가 필요하다.
	 if ( frameScore == 10 )
		 score += frameScore + itsThrows[ball++]; // 다음의 투구를 더한 값
	 else
		 score += frameScore; // 스페어가 아닌 경우
	 } // end for-loop
   
	 return score; // 프레임 점수 반환!
}
```

- 한 프레임의 점수가 10이면, 다음 투구의 점수를 더해서 프레임의 점수로 한다.
- 스페어 경우 :
    - frameScore + itsThrows[ball++]
    - 반환값 (score): 입력 프레임까지의 게임 점수

## ball++로 인한 테스트 케이스 실패

---

하지만 아래의 테스트 케이스에서 실패가 발생한다.

`TestGame7`

```java
public void testSimpleFrameAfterSpare()
{
	g.add(3); // 1-1
	g.add(7); // 1-2
	g.add(3); // 2-1
	g.add(2); // 2-2
	assertEquals(13, g.scoreForFrame(1)); // 통과
	assertEquals(18, g.score()); // 실패
}
```

이를 위해 scoreForFrame() 수행 시, ball의 후위증가(++)에 대해 리팩토링을 수행한다.

`Game7`에서의 scoreForFrame()에서는 firstThrow와 secondThrow에서 두 차례의 ball++를 통해 투구에 대한 인덱스(ball)값 이미 증가되었다.

- `Game7`
    
    ```java
    public int scoreForFrame(int theFrame)
    {
    	int ball = 0;
    	int score=0;
    
    	for (int currentFrame = 0; currentFrame < theFrame;	currentFrame++)
    	{
    	 int firstThrow = itsThrows[ball++]; // 첫 번째 투구 계산
    	 int secondThrow = itsThrows[ball++]; // 두 번째 투구 계산
    	 int frameScore = firstThrow + secondThrow; // 두 투구를 더한 값
    
    	 // 스페어인 경우, 다음 프레임의 첫 번째 투구가 필요하다.
    	 if ( frameScore == 10 )
    		 score += frameScore + itsThrows[**ball++**]; // 다음의 투구를 더한 값
    	 else
    		 score += frameScore; // 스페어가 아닌 경우
    	 } // end for-loop
       
    	 return score; // 프레임 점수 반환!
    }
    ```
    

즉, spare 처리 시의 itsThrow[ball++] 문장으로 인해 스페어 처리 이후 ball의 증가로 다음 프레임 처리 시, firstThrow에 다음 프레임의 첫 투구 점수를 저장하는 것이 아닌, 다음 프레임의 두 번째 투구 점수를 저장하게 된다.
이를 위해 spare 처리 시, itsThrow[ball++]가 아닌 itsThrow[ball]로 처리를 해주자고 한다.

- 왜 오류가 생기게 되는지 이해가 잘 안 된다면 여기를 보자.
    
    `TestGame7`
    
    ```java
    g.add(3); // 1-1
    g.add(7); // 1-2
    g.add(3); // 2-1
    g.add(2); // 2-2
    ```
    
    `Game7`
    
    ```java
    public int scoreForFrame(int theFrame)
    {
    	int ball = 0;
    	int score=0;
    
    	for (int currentFrame = 0; currentFrame < theFrame;	currentFrame++)
    	{
    	 int firstThrow = itsThrows[ball++]; // 첫 번째 투구 계산
    	 int secondThrow = itsThrows[ball++]; // 두 번째 투구 계산
    	 int frameScore = firstThrow + secondThrow; // 두 투구를 더한 값
    
    	 // 스페어인 경우, 다음 프레임의 첫 번째 투구가 필요하다.
    	 if ( frameScore == 10 )
    		 score += frameScore + itsThrows[ball++]; // 다음의 투구를 더한 값
    	 else
    		 score += frameScore; // 스페어가 아닌 경우
    	 } // end for-loop
       
    	 return score; // 프레임 점수 반환!
    }
    ```
    
    - 프레임 1 : 스페어
        - `TestGame7`에서 1-1 : 3점
            - `Game`에서의  `int firstThrow = itsThrows[ball++];`
                - ⇒ ball(0) “1-1”
                - ⇒ firstThrow=itsThrows[0]=3
                - ⇒ ball++
                - ⇒ ball(1) “1-2”
        - `TestGame7`에서 1-2 : 7점
            - `Game`에서의 `int secondThrow = itsThrows[ball++];`
                - ⇒ ball(1) “1-2”
                - ⇒ secondThrow=itsThrows[1]=7
                - ⇒ ball++
                - ⇒ ball(2) “2-1”
            - `frameScore = firstThrow + secondThrow == 10` ,즉 스페어이므로 다음 코드 실행
                - `Game`에서의`score += frameScore + itsThrows[ball++];`실행 
                ( **ball++쓴 것으로 오류 발생! )**
                    - ⇒ ball(2) “2-1”
                    - ⇒ itsThrows[2]=3
                    - ⇒ ball++ 
                    **여기서 후위증가를 해버리므로 ball이 2-1이 아닌 2-2를 가르키게 됨 (한 칸씩 밀려버림)**
                    - ⇒ball(3) “2-2”
    - 프레임 2 : 일반점수
        - `TestGame7`에서 **2-1** : 3점
            - `Game`에서의  `int firstThrow = itsThrows[ball++];`
            - ⇒ ball(3) **“2-2”**
            - ⇒ itsThrows[3]=2=firstThrow
            - ⇒ ball++
            - ⇒ ball(4) “3-1”
        - `TestGame7`에서 **2-2** : 2점
            - `Game`에서의  `int secondThrow = itsThrows[ball++];`
            - ⇒ ball(4) “3-1”
            - ⇒ itsThrows[4]=??
            - ⇒ ball++
            - ⇒ ball(4) “3-2”

`Game8`

```java
public int scoreForFrame(int theFrame)
{
	int ball = 0;
	int score=0;

	for (int currentFrame = 0; currentFrame < theFrame;	currentFrame++)
	{
	 int firstThrow = itsThrows[ball++]; // 첫 번째 투구 계산
	 int secondThrow = itsThrows[ball++]; // 두 번째 투구 계산
	 int frameScore = firstThrow + secondThrow; // 두 투구를 더한 값

	 // 스페어인 경우, 다음 프레임의 첫 번째 투구가 필요하다.
	 if ( frameScore == 10 )
		 score += frameScore + itsThrows[**ball**]; // 다음의 투구를 더한 값
	 else
		 score += frameScore; // 스페어가 아닌 경우
	 } // end for-loop
   
	 return score; // 프레임 점수 반환!
}
```

후위증가 부분을 수정해주었음에도 `TestGame7`에 대해서 실패한다.

반대로 `TestGame7`의 총 점수를 계산하는 것 대신, `TestGame8` 2번째 프레임 점수를 계산하는 테스트는 통과하게 된다.

`TestGame8`

```java
public void testSimpleFrameAfterSpare()
{
	g.add(3); // 1-1
	g.add(7); // 1-2
	g.add(3); // 2-1
	g.add(2); // 2-2
	assertEquals(13, g.scoreForFrame(1)); // 통과
	assertEquals(18, g.scoreForFrame(2)); // 통과
}
```

이에 따라 Game클래스의 score( )메소드가 잘못되었는지 확인해보도록 한다.

`Game`

```java
//Game.java----------------------------------
public class Game
{
	public int score()
	{
	 return itsScore;
	}

	public void add(int pins) // 배열에 새 정수 추가
	{ 
	 itsThrows[itsCurrentThrow++]=pins; 
	 // itsCurrentThrow++ -> 다음 투구 핀수 를 저장할 위치를 유지(++)
	 itsScore += pins;
	 // itsScore에 핀수를 누적
	}

	public int scoreForFrame(int theFrame)
	{
		int ball = 0;
		int score=0;

		for (int currentFrame = 0; currentFrame < theFrame;	currentFrame++)
		{
		 int firstThrow = itsThrows[ball++]; // 첫 번째 투구 계산
		 int secondThrow = itsThrows[ball++]; // 두 번째 투구 계산
		 int frameScore = firstThrow + secondThrow; // 두 투구를 더한 값

		 // 스페어인 경우, 다음 프레임의 첫 번째 투구가 필요하다.
		 if ( frameScore == 10 )
			 score += frameScore + itsThrows[ball]; // 다음의 투구를 더한 값
		 else
			 score += frameScore; // 스페어가 아닌 경우
		 } // end for-loop
	   
		 return score; // 프레임 점수 반환!
	}

	private int itsScore = 0; // 핀수를 누적
	private int[] itsThrows = new int[21]; // 투구 핀수를 저장
	private int itsCurrentThrow = 0;// 다음 투구 핀수를 저장할 위치를 유지(add()에서 ++ 사용)
}
```

현재까지 작성한 Game클래스의 `score()`는 실제 스코어를 반환해야하지만, add()를 통해 결국 넘어뜨린 핀 수의 합을 반환할 뿐이다.

- add(): 쓰러뜨린 핀수를 저장, 합산
    - *itsThrows[]: 투구의 핀수들을 저장*
    - *itsScore: 게임의 점수*

우리가 원하는 **Game에서의 score()의 역할**은 게임의 점수를 반환하는 것이다.

즉 **현재 프레임으로 scoreForFrame()을 호출하는 일**이다.

따라서 score()이 현재 프레임으로 getCurrentFrame()을 호출하도록 변경하도록 한다.

하지만 현재 프레임 번호가 무엇인지 확인하는 방법이 없다.

이를 위해 현재 프레임 번호가 무엇인지 확인하는 `getCurrentFrame()`을 TDD를 통해 설계해보자.

# TDD를 사용한 getCurrentFrame() 설계

---

`getCurrentFrame()`

- 현재 투구를 할 프레임을 확인
- 반환값은 현재 프레임의 번호

## 현재 투구가 1인지 테스트 작성

---

`TestGame9`

```java
//TestGame.java------------------------------------------
 public void testOneThrow()
 {
	 g.add(5); // 1-1
	 assertEquals(5, g.score()); // 1-1의 점수가 5인지 체크
	 assertEquals(1, g.getCurrentFrame()); // 현재 프레임이 1인지 체크
 }
```

`Game9`

```java
//Game.java----------------------------------
 public int getCurrentFrame()
 {
	 return 1; // 현재 프레임 : 1
 }
```

## 현재 투구가 2인지 테스트 작성

---

`TestGame10`

```java
//TestGame.java------------------------------------------
public void testTwoThrowsNoMark()
{
	 g.add(5); // 1-1
	 g.add(4); // 1-2
	 assertEquals(9, g.score()); // 1-1 + 1-2 의 점수가 9인지 체크
	 assertEquals(1, g.getCurrentFrame()); // 현재 프레임이 1인지 체크
}
```

## 현재 투구가 4인지 테스트 작성

---

`TestGame11`

```java
//TestGame.java------------------------------------------
public void testFourThrowsNoMark()
{
	 g.add(5); // 1-1
	 g.add(4); // 1-2
	 g.add(7); // 2-1
	 g.add(2); // 2-2

	 assertEquals(18, g.score()); // 1-1 + 1-2 + 2-1 + 2-2 의 점수가 18점인지 체크
	 assertEquals(9, g.scoreForFrame(1)); // 프레임1의 점수가 9인지 체크
	 assertEquals(18, g.scoreForFrame(2)); // 프레임1의 점수가 9인지 체크
	 assertEquals(2, g.getCurrentFrame()); // 현재 프레임(2)이 2인지 체크
}
```

`TestGame11`에선 2개의 프레임을 추가했으나, Game의 `getCurrentFrame()`에서 1만을 반환하게 설계되어있기에 실패하게 된다.

여기서 제대로 동작할 때까지 1을 더했다 뺐다를 반복하며 getCurrentFrame()을 설계한다.

이를 ‘우연에 의한 프로그래밍,programming by coincidence’ 이라고 부른다.

> 저자는 다음과 같이 표현하였다.

*(flails around adding and subtracting 1 until it works)
(제대로 동작할 때까지 1을 더했다 뺐다를 반복한다)*
> 

`Game10`

```java
public int getCurrentFrame()
{
	return 1 + (itsCurrentThrow-1)/2;
}
```

- 프레임 당 두번의 ‘투구’
    - ‘투구’의 횟수/2…
- `itsCurrentThrow` : 현재의 투구 횟수
    - `1+ (itsCurrnetThrow-1)/2`
    - 반한 할 때마다 **현재 프레임 값**을 계산한다
- 이해가 안 된다면 아래 예시를 참조해보자.
    
    **/2 ??**
    
    - `투구1` / 2 = 0
    - `투구2` / 2 = 1
    - `투구3` / 2 = 1
    - `투구4` / 2 = 2
    - ….
    
    **(현재 투구-1) / 2 ??**
    
    - (`투구1`-`1`) / 2 = 0 / 2 = 0
    - (`투구2`-`1`) / 2 = 1 / 2 = 0
    - (`투구3`-`1`) / 2 = 2 / 2 = 1
    - (`투구4`-`1`) / 2 = 3 / 2 = 1
    - (`투구5`-`1`) / 2 = 4 / 2 = 2
    - (`투구6`-`1`) / 2 = 5 / 2 = 2
    - ….
    
    **1 + (현재 투구-1) / 2 ??**
    
    - (`투구1`-`1`) / 2  + 1 = 0 / 2 + 1  = 0 + 1 = 1
    - (`투구2`-`1`) / 2  + 1 = 1 / 2 + 1  = 0 + 1 = 1
    - (`투구3`-`1`) / 2  + 1 = 2 / 2 + 1  = 1 + 1 = 2
    - (`투구4`-`1`) / 2  + 1 = 3 / 2 + 1  = 1 + 1 = 2
    - (`투구5`-`1`) / 2  + 1 = 4 / 2 + 1  = 2 + 1 = 3
    - (`투구6`-`1`) / 2  + 1 = 5 / 2 + 1  = 2 + 1 = 3
    - ….

# 각 투구, 즉 add() 후 currentFrame 조정하게끔 설계

---

*이를 각 ‘투구’ 후 멤버 변수인 currentFrame 조정하는 것으로 변경해보도록 하자.* 

*add() 함수에서 itsCurrentFrame값을 조정해보록 하자.*

## 각 프레임의 첫 번째 투구에서 프레임 넘버를 조정하는 경우

---

- itsCurrentFrame : 현재 프레임 번호 
→ 초기값 0
- getCurrentFrame()
    
    → itsCurrentFrame을 반환
    
- firstThrow
→ 각 프레임에서 첫번째 투구 여부 (T/F toggling)

`Game11`

```java
//Game.java----------------------------------
public int getCurrentFrame()
{
	return itsCurrentFrame;
}

public void add(int pins)
{
	itsThrows[itsCurrentThrow++]=pins;
	itsScore += pins;
	if (firstThrow == true)	{
		firstThrow = false;
		itsCurrentFrame++;
	}
	else {
		firstThrow=true;;
	}
}

private int itsCurrentFrame = 0;
private boolean firstThrow = true;
```

## 각 프레임의 두 번째 투구에서 프레임 넘버를 조정하는 경우

---

- itsCurrentFrame : 현재 프레임 번호 
→ 초기값 1
- getCurrentFrame()
    
    → itsCurrentFrame을 반환
    
- firstThrow
→ 각 프레임에서 첫번째 투구 여부 (T/F toggling)

`Game12`

```java
//Game.java----------------------------------
public int getCurrentFrame()
{
	return itsCurrentFrame;
}

public void add(int pins)
{
	itsThrows[itsCurrentThrow++]=pins;
	itsScore += pins;
	if (firstThrow == true)	{
		firstThrow = false;
	}
	else	{
		firstThrow=true;
		itsCurrentFrame++;
	}
}

private int itsCurrentFrame = 0;
private boolean firstThrow = true;
```

**1. 각 프레임의 첫 번째 투구에서 프레임 넘버를 조정하는 경우** 에서는 현재 프레임(itsCurrentFrame)은 마지막 공을 던진 프레임, 즉 이전에 공을 던진 프레임을 가르키고 있다.
(애초에 초기값이 0이므로…!!)

하지만 현재 프레임(itsCurrentFrame)은 내가 던지고 있는 프레임의 번호를 가르켜야 한다.

따라서 **2. 각 프레임의 두 번째 투구에서 프레임 넘버를 조정하는 경우** 를 사용해야한다.

( 즉, 내가 던지고 있는 프레임 번호를 지정하기 위해서는 각 프레임의 마지막 투구 직후에 증가되어야한다. ***이렇게 되면 itsCurrentFrame은 다음에 투구할 프레임을 가르키게 된다*** )

또한 프레임 넘버 조정은 매 프레임마다 반복되므로 이를 add()에서 꺼내어 전용멤버함수로 만들 필요가 있다.

전용멤버함수 adjustCurrentFrame()을 만들면 아래와 같다.

# 프레임 넘버 조정하는 adjustCurrentFrame() 설계

---

`TestGame12`

```java
public void testTwoThrowsNoMark()
{
	g.add(5); // 1-1
	g.add(4); // 1-2
	assertEquals(9, g.score()); // 1-1 + 1-2 의 점수가 9인지 체크
	assertEquals(2, g.getCurrentFrame()); // itsCurrentFrame이 2인지 체크
}	

public void testFourThrowsNoMark()
{
	g.add(5); // 1-1
	g.add(4); // 1-2
	g.add(7); // 2-1
	g.add(2); // 2-2

	assertEquals(18, g.score()); // 1-1 + 1-2 + 2-1 + 2-2 의 점수가 18점인지 체크
	assertEquals(9, g.scoreForFrame(1)); // 프레임1의 점수가 9인지 체크
	assertEquals(18, g.scoreForFrame(2)); // 프레임1의 점수가 9인지 체크
	assertEquals(3, g.getCurrentFrame()); // itsCurrentFrame이 3인지 체크
}
```

`Game12`

```java
//Game.java------------------------------------------
public void add(int pins)
{
	itsThrows[itsCurrentThrow++]=pins;
	itsScore += pins;
	adjustCurrentFrame();
}

private void adjustCurrentFrame() {
	 if (firstThrow == true) {
		 firstThrow = false;
	 }
	 else {
		 firstThrow=true;
		 itsCurrentFrame++;
	 }
}

private int itsCurrentFrame = 1;
```

# 스페어 케이스 처리

---

## 스페어가 나오는 2개의 테스트 시험

---

`TestGame13`

```java
public void testSimpleSpare()
{
	g.add(3);
	g.add(7);
	g.add(3);
	assertEquals(13, g.scoreForFrame(1));
}
public void testSimpleFrameAfterSpare()	{
	g.add(3);
	g.add(7);
	g.add(3);
	g.add(2);

	assertEquals(13, g.scoreForFrame(1));
	assertEquals(18, g.scoreForFrame(2));
	assertEquals(3, g.getCurrentFrame());
}
```

위 테스트는 제대로 동작한다.

## 스페어를 위해 현재 프레임의 이전 프레임 반환하는 score() 작성

---

지금까지의 add()를 통해 itsScore에 pins를 누적합산해왔고,score()함수를 통해 itsScore를 반환하였다.

```jsx
// Game12
public void add(int pins)
{
	itsThrows[itsCurrentThrow++]=pins;
	itsScore += pins;
	adjustCurrentFrame();
}
// Game4
public int score()
{
	return itsScore;
}
```

그러나 이런 과정에는 스페어인 경우의 점수 계산이 반영되어 있지 않다.

스페어인 경우를 계산해주기 위해 현재 프레임(itsCurrentFrame)의 이전 프레임( itsCurrentFrame -1 )에 대한 점수와 현재 프레임의 점수를 알아야한다.

***따라서 score()는이전 프레임( itsCurrentFrame -1 )에 대해 계산해야한다.***

이를 위해 `scoreForFrame(getCurrentFrame( ) -1)`을 호출한 `score()`를 작성해보자. 

`Game13`

```jsx
public int score() {
	return scoreForFrame(getCurrentFrame()-1);
}
```

- `getCurrentFrame( )` : “현재 프레임”
- `getCurrentFrame( ) -1` : “이전 프레임”. 점수 계산은 (현재 프레임-1)에서 가능
- `scoreForFrame(getCurrentFrame( ) -1)` : “(스페어를 친)이전 프레임” 의 “점수 계산”

`TestGame14`

```java
public void testSimpleFrameAfterSpare() {
	g.add(3);
	g.add(7);
	g.add(3);
	g.add(2);

	assertEquals(13, g.scoreForFrame(1));
	assertEquals(18, g.scoreForFrame(2));
	assertEquals(18, g.score());
	assertEquals(3, g.getCurrentFrame());
}
```

~~다만 이렇게 score()를 작성하게 되면 부작용이 발생한다.~~ 

~~score()에서 무조건 이전 프레임을 도출해내면, 투구 한 번만 한 경우, 프레임0번째에 대해 계산을 수행하게 된다.~~ 

~~이를  TestOneThrow 테스트 케이스를 통해 확인할 수 있다.~~

`TestGame15`

```java
public void testOneThrow()
{
	g.add(5);
	assertEquals(5, g.score());
	assertEquals(1, g.getCurrentFrame());
}
```

~~하지만 목적에 부합하는 테스트가 아님에 따라 테스트 케이스를 삭제한다~~

# 스트라이크 처리

---

`TestGame16`

```java
public void testSimpleStrike() {
	g.add(10); // 프레임 #1(스트라이크) : 10 + 3 + 6 = 19 점
	g.add(3);
	g.add(6); // Frame2 : 9
	assertEquals(19, g.scoreForFrame(1)); // 프레임 #1(스트라이크) : 10 + 3 + 6 = 19 점
	assertEquals(28, g.score()); // 프레임 #2 : 3 + 9 = 9점
	assertEquals(3, g.getCurrentFrame());
}
```

- 프레임 #1(스트라이크) : 10 + 3 + 6 = 19 점
- 프레임 #2 : 3 + 9 = 9점
- 게임 점수 : 19 + 9 = 28점
- 현재 투구 프레임 : #3

## 위 스트라이크 테스트 케이스를 통과하도록 Game클래스를 설계해보자.

---

## 1. 핀수를 입력받도록 변경: add()

---

`Game14`

```java
public void add(int pins) {
		itsThrows[itsCurrentThrow++]=pins;
		itsScore += pins;
		adjustCurrentFrame(pins); // 핀수를 입력받도록 변경: add()
}
```

## 2. 스트라이크인 경우의 프레임 넘버 조정 : adjustCurrentFrame()

---

- 첫번째 투구이면
    - 스트라이크이면, 프레임 넘버 증가
    - 스트라이크 아니면, 첫번째 투구 처리됨 (false)
- 두번째 투구이면
    - 프레임 넘버 증가

⛔ *itsCurrentFrame : 다음에 투구할 프레임 넘버* ⛔ 

`Game14`

```java
private void adjustCurrentFrame(int pins) {
	 if (firstThrow == true) {  // 첫번째 투구에서
			 if( pins == 10 )
				 itsCurrentFrame++; // 스트라이크이면, 프레임 넘버 증가
			 else
				 firstThrow = false; // 스트라이크 아니면, 첫번째 투구 처리됨 (false)
	 }
	 else { // 두번째 투구이면
			 firstThrow=true; // 첫번째 투구 토글
			 itsCurrentFrame++; // 프레임 넘버 증가
	 }
}
```

## 3. 스트라이크인 경우의 점수 계산 : scoreForFrame()

---

- 스트라이크 점수 계산을 적용
    - 10 + 다음의 두 투구의 핀수
        
        → `10 + itsThrows[ball] + itsThrows[ball+1]`
        

`Game14`

```java
public int scoreForFrame(int theFrame)
{
	int ball = 0;
	int score=0;
	for (int currentFrame = 0;currentFrame < theFrame;currentFrame++) {
		int firstThrow = itsThrows[ball++];
		if (firstThrow == 10) {
			score += 10 + itsThrows[ball] + itsThrows[ball+1];
			// 10 + 다음의 두 투구의 핀수 ( itsThrows[ball] + itsThrows[ball+1] ) 
		}
		else {
			int secondThrow = itsThrows[ball++];
			int frameScore = firstThrow + secondThrow; // 두 투구를 더한 값
			// spare needs next frames first throw
			if ( frameScore == 10 )
				score += frameScore + itsThrows[ball]; // next frames first throw를 더한 값
			else
				score += frameScore; // 스페어가 아닌 경우
		}
	}
	return score; // 프레임 점수 반환!
}
```

- Game의 전체 소스코드는 아래와 같다.
    
    `Game.java`
    
    ```java
    //Game.java----------------------------------
    public class Game
    {
    	public int score() {
    		return scoreForFrame(getCurrentFrame()-1);
    	}
    
    	public int scoreForFrame(int theFrame)
    	{
    		int ball = 0;
    		int score=0;
    		for (int currentFrame = 0;currentFrame < theFrame;currentFrame++)
    		{
    			int firstThrow = itsThrows[ball++];
    			// 스트라이크인 경우
    			if (firstThrow == 10) 
    			{
    				score += 10 + itsThrows[ball] + itsThrows[ball+1]; //10+첫투구+두번째투구
    			}
    			else
    			{
    				int secondThrow = itsThrows[ball++];
    				int frameScore = firstThrow + secondThrow;
    				
    				// 스페어인 경우, 다음 프레임의 첫 번째 투구가 필요하다.
    				if ( frameScore == 10 )
    					score += frameScore + itsThrows[ball]; // 다음의 투구를 더한 값
    				
    				// 스페어가 아닌 경우
    				else
    					score += frameScore; // 스페어가 아닌 경우
    			}
    		}
    		return score;
    	} // end scoreForFrame()
    	
    	public int getCurrentFrame()
    	{
    		return itsCurrentFrame;
    		// 현재 프레임 번호인 itsCurrentFrame을 반환
    	}
    	
    	public void add(int pins)
    	{
    		itsThrows[itsCurrentThrow++]=pins;
    		itsScore += pins;
    		adjustCurrentFrame(pins);
    	}
    
    	private void adjustCurrentFrame(int pins)
    	{
    		if (firstThrow == true)
    		{
    			if( pins == 10 ) // strike
    				itsCurrentFrame++;
    			else
    				firstThrow = false;
    		}
    		else
    		{
    			firstThrow=true;
    			itsCurrentFrame++;
    		}
    	} // end adjustCurrentFrame()
    
    	private int itsCurrentFrame = 1; // 현재 프레임 번호 
    	private boolean firstThrow = true; // 각 프레임에서 첫번째 투구 여부 (T/F toggling)
    	private int itsScore = 0; // 핀수를 누적
    	private int[] itsThrows = new int[21]; // 투구 핀수를 저장
    	private int itsCurrentThrow = 0;// 다음 투구 핀수를 저장할 위치를 유지(add()에서 ++ 사용)
    } // end Game class
    ```
    

# Frame의 경계값 설정

---

## 퍼펙트게임 테스트

---

아래 퍼펙트 게임 테스트 케이스 수행 시, 실패하게 된다.

`TestGame17`

```java
public void testPerfectGame()
{
	for (int i=0; i<12; i++)
	{
		g.add(10);
	}
	assertEquals(300, g.score()); 
	// 기대값 : 10프레임 * 30 = 300
	// 실제값 : 330
	assertEquals(10, g.getCurrentFrame());
}
```

add()를 11번 부르니 현재프레임, 즉 itsCurrentFrame이 12까지 계속 증가하게 된다.

이로 인해 30점(퍼펙트게임 시의 프레임점수)이 11번 곱혀져 330점이 나오게되어 테스트에 실패하게 된다.

### 해결법 1 : 현재 프레임을 10으로 제한

---

`Game15`

```java
private void adjustCurrentFrame(int pins) {
	if (firstThrow == true) {  // 첫번째 투구에서
		if( pins == 10 ) 
			itsCurrentFrame++; // 스트라이크이면, 프레임 넘버 증가
		else
			firstThrow = false; // 스트라이크 아니면, 첫번째 투구 처리됨 (false)
	}
	else { // 두번째 투구에서
		firstThrow=true; // 첫 투구 토글
		itsCurrentFrame++; // 프레임 넘버 증가
	}
	itsCurrentFrame = Math.min(10, itsCurrentFrame); 
	//현재 프레임을 10번 프레임으로 제한
}
```

```java
public int score() {
	return scoreForFrame(getCurrentFrame()-1);
}
```

Math.min()을 통해 itsCurrentFrame을 10을 제한하였다.

**문제는 score() 에서 `return scoreForFrame(getCurrentFrame()-1);`  에서 1을 뺀다는 것이다.**

**이로 인해 10번째가 아닌 9번째 프레임의 스코어를 계산하게 된다.**

### 해결법 2 : 현재 프레임을 11로 제한, 11을 가르키면 게임을 끝내게끔 규칙을 수정

---

score()에서 프레임 넘버 -1해서 점수를 계산하므로 +1 한 값인 11로 최소값을 설정하도록 수정해보자. 

다만, 이렇게 되면 itsCurrentFrame이, 즉 현재 프레임이 11을 가르키게 된다는 부작용이 발생한다.

따라서 게임의 규칙으로서 getCurrentFrame이 11을 가르키면 게임이 끝나게끔 하게 하자.

```java
private void adjustCurrentFrame(int pins) {
	if (firstThrow == true) {  // 첫번째 투구에서
		if( pins == 10 ) 
			itsCurrentFrame++; // 스트라이크이면, 프레임 넘버 증가
		else
			firstThrow = false; // 스트라이크 아니면, 첫번째 투구 처리됨 (false)
	}
	else { // 두번째 투구에서
		firstThrow=true; // 첫 투구 토글
		itsCurrentFrame++; // 프레임 넘버 증가
	}
	**itsCurrentFrame = Math.min(11, itsCurrentFrame);** 
	//현재 프레임을 10번 프레임으로 제한
}
```

`TestGame18`

```java
public void testPerfectGame()
{
	for (int i=0; i<12; i++)
	{
		g.add(10);
	}
	assertEquals(300, g.score()); // 통과
	**assertEquals(11, g.getCurrentFrame()); // 통과**
}
```

우리는 게임 종료 시, 현재 프레임이 11을 가르키게 하였다.

만약 **10번째에서 스페어 이후 스페어 처리 위해 11번째 던진 공이 스트라이크라면, 스트라이크로 처리되어 12번째,13번째 프레임까지 늘어나 스트라이크로 처리되는지 테스트** 해보자.

`TestGame19`

```java
public void testEndOfArray()
{
	for (int i=0; i<9; i++) // 프레임 9개 모두 0점
	{
		g.add(0);
		g.add(0);
	}
	// 10번째 프레임 스페어
	g.add(2); // 10-1
	g.add(8); // 10-2

	// 스페어 처리 위한 공이 스트라이크
	g.add(10); // 11-1
	assertEquals(20, g.score());
}
```

이 테스트는 통과한다. 

score는 절대 10보다 큰 수로 scoreForFrame을 호출하지 않기 때문에 마지막 스트라이크(11번째)에 대해서는 scoreForFrame을 호출하지 않는다.

즉 마지막 스트라이크는 마지막 스페어를 완결짓는 10이란 점수로 계산될 뿐이다.

이제 원래 만들었던 스코어 카드를 이 프로그램에 집어넣어 보자.

`TestGame20`

```java
public void testSampleGame()
{
	// 1
	g.add(1);
	g.add(4);
	// 2
	g.add(4);
	g.add(5);
	// 3
	g.add(6);
	g.add(4);
	// 4
	g.add(5);
	g.add(5);
	// 5
	g.add(10);
	// 6
	g.add(0);
	g.add(1);
	// 7
	g.add(7);
	g.add(3);
	// 8
	g.add(6);
	g.add(4);
	// 9
	g.add(10);
	g.add(2);
	// 10
	g.add(8);
	g.add(6);

	assertEquals(133, g.score());
} // 모두 pass
```

**10개의 스트라이크 이후 마지막으로 9점을 내는 테스트**

`TestGame21`

```java
public void testHeartBreak()
{
	// 10개의 스트라이크
	for (int i=0; i<11; i++)
		g.add(10);
	
	// 추가 투구
	g.add(9);
	assertEquals(299, g.score());
} // all pass
```

**10번째 프레임이 스페어인 경우 스페어 처리를 위한 추가투구 1개를 처리하는지 테스트**

`TestGame22`

```java
public void testTenthFrameSpare()
{
	// 9개의 스트라이크
	for (int i=0; i<9; i++)
		g.add(10);
	
	// 10번 쨰 투구가 스페어
	g.add(9);
	g.add(1);
	// 추가 투구
	g.add(1);
	
	assertEquals(270, g.score());
} // all pass
```

# 이제 scoreForFrame을 리팩토링해보자.

---

*기존 scoreForFrame*

`Game14`

```java
public int scoreForFrame(int theFrame)
{
 int ball = 0;
 int score=0;

 for (int currentFrame = 0; currentFrame < theFrame;
	 currentFrame++) {
	 int firstThrow = itsThrows[ball++];

	 **// strike case**
	 if (firstThrow == 10) { 
		 score += 10 + itsThrows[ball] + itsThrows[ball+1];
	 } 

	 **// spare case & else**
	 else { 
		 int secondThrow = itsThrows[ball++];
		 int frameScore = firstThrow + secondThrow;
		 // spare needs next frames first throw
		 if ( frameScore == 10 )
			 score += frameScore + itsThrows[ball]; // 다음 프레임의 첫 투구를 더한 값
		 else
			**// neither strike, spare case**
			 score += frameScore; 
		 }
	 }
 return score; // 프레임 점수 반환!
}
```

## 1차 수정 : Strike, Spare, Else 구분

---

- **strike case / spare case & else 구분**
    - spare case & else를 handleSecondThrow()로 추출
- 이를 위해 지역변수 **ball, firstThrow,secondThrow를 Game 클래스의 멤버변수**로 변경

`Game16`

```java
public int scoreForFrame(int theFrame)
{
	ball = 0;
	int score=0;
	for (int currentFrame = 0; currentFrame < theFrame; currentFrame++) {
		firstThrow = itsThrows[ball++];
		**// strike 
		if (firstThrow == 10)
		{
			score += 10 + itsThrows[ball] + itsThrows[ball+1];
		}
		// spare & else 
		else {
			score += handleSecondThrow();**
		}
	}
	return score;
}

**private int handleSecondThrow()**
{
	int score = 0;
	secondThrow = itsThrows[ball++];

	int frameScore = firstThrow + secondThrow;
	// spare
	if ( frameScore == 10 )
		score += frameScore + itsThrows[ball];
	// else
	else
		score += frameScore;
	return score;
}

**private int ball;
private int firstThrow;
private int secondThrow;**
private int itsScore = 0;
private int[] itsThrows = new int[21];
private int itsCurrentThrow = 0;
private int itsCurrentFrame = 1;
private boolean firstThrowInFrame = true;
```

- adjustCurrentFrame()와 scoreForFrame() 간 firstThrow변수에 대한 충돌이 일어나기에 adjustCurrentFrame()를 위한 멤버변수 **firstThrowInFrame 를 따로 선언**

`Game17`

```java
private void adjustCurrentFrame(int pins)
{
	if (firstThrowInFrame == true) {
	if( pins == 10 ) // strike
		itsCurrentFrame++;
	else
		firstThrowInFrame = false;
	}
	else {
		 firstThrowInFrame=true;
		 itsCurrentFrame++;
	}
	itsCurrentFrame = Math.min(11, itsCurrentFrame);
}
```

## 2차 수정 : scoreForFrame 구조화 설계

---

가독성을 위하여 scoreForFrame의 strike,spare,else 점수 계산에 대하여 다음과 같은 구조로 변경하도록 해본다.

```java
if strike
	score += 10 + nextTwoBalls();
else if spare
	score += 10 + nextBall();
else
	score += twoBallsInFrame()
```

→ 이를 위해서는 ball 변수가 증가되는 방식을 변경하여 **세 경우가 독립적으로 ball 변수를 조작하게하여야한다.**

- **기존 ball 변수 증가방식**
    
    `Game16`
    
    ```java
    public int scoreForFrame(int theFrame)
    {
    	ball = 0;
    	int score=0;
    	for (int currentFrame = 0; currentFrame < theFrame; currentFrame++) {
    		***firstThrow = itsThrows[ball++];***
    		// strike 
    		if (firstThrow == 10)
    		{
    			score += 10 + itsThrows[ball] + itsThrows[ball+1];
    		}
    		// spare & else 
    		else {
    			score += handleSecondThrow();
    		}
    	}
    	return score;
    }
    ```
    

### Strike

---

`Game18`

```java
public int scoreForFrame(int theFrame)
{
 ball = 0;
 int score=0;
 for (int currentFrame = 0; currentFrame < theFrame; currentFrame++) {
	 ***firstThrow = itsThrows[ball];*** **//ball 변수는 항상 프레임의 첫투구의 위치를 가지게 함!!

	 *// if strike***
	 if (firstThrow == 10) {
		 *****ball++;* **// 프레임에서의 ball 위치 이동은 표현식 으로 처리**
		 score += 10 + itsThrows[ball] + itsThrows[ball+1];
	 }
	 else {
		 score += handleSecondThrow();
	 }
 }
 return score;
}
```

- `firstThrow = itsThrows[ball++];`  →  `firstThrow = itsThrows[ball];`
*****:* ball 변수는 항상 프레임의 첫 투구의 위치를 가지게 한다. 
이를 통해 strike인 경우, spare인 경우, else인 경우 각각에 따른 ball 증감 조작이 가능해졌다.
- strike인 경우, `ball++;`을 통해 증감 수행

### Spare & Else

---

`Game19`

```java
private int handleSecondThrow() {
	 int score = 0;
	 secondThrow = itsThrows[**ball+1**];
	 int frameScore = firstThrow + secondThrow;

	 ***// spare***
	 if ( frameScore == 10 ) {
		 **ball+=2; // 다음 프레임의 첫투구 위치**
		 score += frameScore + itsThrows[ball];
	 }

	 ***// else*** 
	 else	{
		 **ball+=2; // 다음 프레임의 첫투구 위치**
		 score += frameScore;
	 }
	 return score;
}
```

- 두번째 투구: 스페어 경우과 그 외의 경우 처리
- 두번째 투구: itsThrows[ball+1]
- 스페어 계산을 위한 다음 프레임의 첫 투구: ball += 2

## 3차 수정 : Strike,Spare,Else 에 대한 firstThrow,secondThrow 모듈/컴포넌트화

---

가독성을 위하여 firstThrow와 secondThrow를 없애고 적절한 함수로 대체해본다.

### Strike()

---

- **strike(), nextTwoBalls() 도입**

`Game20`

```java
public int scoreForFrame(int theFrame) {
	 ball = 0;
	 int score=0;
	 for (int currentFrame = 0; currentFrame < theFrame; currentFrame++) {
		 firstThrow = itsThrows[ball];
		 if (**strike()**) {
			 ball++;
			 score += 10 + nextTwoBalls();
		 }
		 else {
			 score += **handleSecondThrow()**;
		 }
	 }
	 return score;
}
**private boolean strike() {
	 return itsThrows[ball] == 10;
}
private int nextTwoBalls() {
	 return itsThrows[ball] + itsThrows[ball+1];
}**
```

### Spare() & Else 처리

---

- spare(), nextBall() 신설

`Game21`

```java
private int handleSecondThrow()
{
	 int score = 0;
	 secondThrow = itsThrows[ball+1];
	 int frameScore = firstThrow + secondThrow;
	 // spare needs next frames first throw
	 if ( **spare()** )
	 {
		 ball+=2;
		 score += 10 + **nextBall()**;
	 }
	 else
	 {
		 ball+=2;
		 score += frameScore;
	 }
	 return score;
}
**private boolean spare()
{
	 return (itsThrows[ball] + itsThrows[ball+1]) == 10;
}
private int nextBall()
{
	 return itsThrows[ball];
}**
```

### frameScore (= firstThrow + secondThrow) 모듈화

---

firstThrow ,secondThrow 모두 제거하고, 이를 모듈화해주었으므로 점수 총합을 계산해주는 변수 **frameScore (= firstThrow + secondThrow) 또한 처리**해준다.

- frameScore 변수 제거
- twoBallsInFrame() 도입

`Game22`

```java
private int handleSecondThrow()
{
	int score = 0;
	secondThrow = itsThrows[ball+1];

	// spare needs next frames first throw
	if ( spare() )
	{
		ball+=2;
		score += 10 + nextBall();
	}
	else
	{
		**score += twoBallsInFrame();**
		ball+=2;
	}
	return score;
}
**private int twoBallsInFrame()
{
	return itsThrows[ball] + itsThrows[ball+1];
}**
```

![Untitled](Bowling%20Game%2043113191dd22445baf440b9137dfddbd/Untitled%202.png)

scoreForFrame( )의 if(strike)…문과 handleSecondThrow( )의 spare()…문,else…문 모두에서 결합된 유일한 ball 변수가 각 경우에 따라 독립적으로 처리된다.

## 4차 수정 : handleSecondThrow()을 scoreForFrame()에 합쳐보자.

---

scoreForFrame() ← handleSecondThrow()

- Strike, Spare, Else 경우를 모두 통합
- pseudo 코드와 가장 유사한 형태로 통합
- 점수 계산 후, ball 변수의 증가 하는 것으로 변경
- Strike
    - nextTwoballs() → nextTwoBallsForStrike()로 변경
        
        ```java
        if (strike())
        {
         score += 10 + nextTwoBallsForStrike();
         ball++;
        }
        ...
        **private int nextTwoBallsForStrike()
        {
        	 return itsThrows[ball+1] + itsThrows[ball+2];
        }**
        ```
        
- Spare
    - nextBall() → nextBallForSpare()로 변경
        
        ```java
        else if ( spare() )
        {
         score += 10 + nextBallForSpare();
         ball+=2;
        }
        ...
        **private int nextBallForSpare()
        {
        	 return itsThrows[ball+2];
        }**
        ```
        
- Else
    - 이전과 같이 twoBallsInFrame() 사용
    
    ```java
    else
    {
     score += twoBallsInFrame();
     ball+=2;
    }
    ...
    **private int twoBallsInFrame()
    {
    	return itsThrows[ball] + itsThrows[ball+1];
    }**
    ```
    

`Game23`

```java
public int scoreForFrame(int theFrame)
{
	 ball = 0;
	 int score=0;
	 for (int currentFrame = 0; currentFrame < theFrame;currentFrame++)
	 {
		 if (strike())
		 {
			 score += 10 + nextTwoBallsForStrike();
			 ball++;
		 }
		 else if ( spare() )
		 {
			 score += 10 + nextBallForSpare();
			 ball+=2;
		 }
		 else
		 {
			 score += twoBallsInFrame();
			 ball+=2;
		 }
	 }
	 return score;
}
**private int nextTwoBallsForStrike()
{
	 return itsThrows[ball+1] + itsThrows[ball+2];
}
private int nextBallForSpare()
{
	 return itsThrows[ball+2];
}**
```

# Bottom-Up이 아닌 Top-Down

---

초기엔 의존성 사슬의 끝단인 Throw의 설계에 과도하게 집중하다 보니, Frame과 Game에 대한 설계를 생각치 못 했다. 

하지만 Throw, Frame, Game 순으로 TDD를 하다보니, 의존의 끝단인 Throw가 아닌 전체적인 윤곽을 볼 수 있는 Game을 먼저 설계하는 방향으로 진행하였다.

이 말인 즉슨, 

가장 낮은 수준에서 위를 바라보기 보다, ***가장 높은 수준에서 시작해서 아래로 내려오면서 설계하는 것을 지향해야한다***는 뜻이다.

이제 Game클래스부터 시작하여 순서대로 Frame, Throw를 설계해보자.

# Scorer클래스 추가 (feat.SRP)

---

SRP를 기반한 설계를 진행해보자.

**(기존)Game**

- Score관리
- Frame관리

⬇️

**Game** 

- 프레임을 쭉 따라가기
    - score()
    - getCurrentFrame()
    - add()
    - adjustCurrentFrame()
    - scoreForFrame()

**Scorer**

- 스코어만 계산
    - addThrow()
    - scoreForFrame()
    - strike(), spare()
    - nextTwoBallsForStrike()
    - nextBallForSpare()
    - twoBallsInFrame()
    - int[] itsThrows
    - ball
    - itsCurrentThrow

`Game24`

```java
//Game.java----------------------------------
public class Game {
	public int score()
	{
	 return scoreForFrame(getCurrentFrame()-1);
	}
	
	public int getCurrentFrame()
	{
		return itsCurrentFrame;
		// 현재 프레임 번호인 itsCurrentFrame을 반환
	}
	
	public void add(int pins)
	{
		itsScorer.addThrow(pins);
		itsScore += pins;
		adjustCurrentFrame(pins);
	}
	
	private void adjustCurrentFrame(int pins)
	{
		if (firstThrowInFrame == true) {
		if( pins == 10 ) // strike
			itsCurrentFrame++;
		else
			firstThrowInFrame = false;
		}
		else {
			 firstThrowInFrame=true;
			 itsCurrentFrame++;
		}
		itsCurrentFrame = Math.min(11, itsCurrentFrame);
	} // end adjustCurrentFrame()
	
	public int scoreForFrame(int theFrame) {
	{
		return itsScorer.scoreForFrame(theFrame);
		// 점수 계산하는 score클래스의 scoreForFrame() 호출
	}

	private int itsScore = 0; // 핀수를 누적
	private int itsCurrentFrame = 1; // 현재 프레임 번호 
	private boolean firstThrowInFrame = true; // 각 프레임에서 첫번째 투구 여부 (T/F toggling)
	private Scorer itsScorer = new Scorer();
}
```

`Scorer1`

```java
//Scorer.java-----------------------------------
public class Scorer
{
	 public void addThrow(int pins)
	 {
		 itsThrows[itsCurrentThrow++] = pins;
	 }

	 public int scoreForFrame(int theFrame)
	 {
		 ball = 0;
		 int score=0;
		 for (int currentFrame = 0; currentFrame < theFrame; currentFrame++)
		 {
			 if (strike())
			 {
				 score += 10 + nextTwoBallsForStrike();
				 ball++;
			 }
			 else if ( spare() )
			 {
				 score += 10 + nextBallForSpare();
				 ball+=2;
			 }
			 else
			 {
				 score += twoBallsInFrame();
				 ball+=2;
			 }
		 }
		 return score;
	 }

	 private boolean strike()
	 {
		 return itsThrows[ball] == 10;
	 }

	 private boolean spare()
	 {
		 return (itsThrows[ball] + itsThrows[ball+1]) == 10;
	 }

	 private int nextTwoBallsForStrike()
	 {
		 return itsThrows[ball+1] + itsThrows[ball+2];
	 }

	 private int nextBallForSpare()
	 {
		 return itsThrows[ball+2];
	 }

	 private int twoBallsInFrame()
	 {
		 return itsThrows[ball] + itsThrows[ball+1];
	 }

	 private int ball; // 투구 수 index
	 private int[] itsThrows = new int[21]; // 투구 핀수를 저장
	 private int itsCurrentThrow = 0; // 다음 투구 핀수를 저장할 위치를 유지
}
```

Game과 Scorer는 각자의 책임을 각각 수행하고 있다. 즉, SRP를 지키고 있다.

# Game, Scorer 리팩토링

---

## 1. Game에서의 add()에서 itsScore가 더 이상 사용되지 않는다.

---

`Game24`

```java
public void add(int pins)
 {
	 itsScorer.addThrow(pins);
	 itsScore += pins;
	 adjustCurrentFrame(pins);
 }
```

이를 아래와 같이 바꾼다.

`Game25`

```java
public void add(int pins)
{
	itsScorer.addThrow(pins);
	adjustCurrentFrame(pins);
}
```

## 2. `itsCurrentFrame++;`를 모듈화 및 리팩토링한다.

---

- `private void advanceFrame()`
    
    ```java
    private void advanceFrame()
    {
    	itsCurrentFrame = Math.min(11, itsCurrentFrame + 1);
    }
    ```
    
    - itsCurrentFrame++; 를 모듈화
        
        → 디버깅하기 힘든 ++ 문법 청소
        
    - itsCurrentFrame을 11로 제한하는 함수에 *itsCurrentFrame증가부분( +1 )*을 넣게한다.

`Game26`

```java
private void adjustCurrentFrame(int pins)
{
	if (firstThrowInFrame == true)
	{
		if( pins == 10 ) // strike
			**advanceFrame();**
		else
			firstThrowInFrame = false;
	}
	else
	{
		firstThrowInFrame=true;
		**advanceFrame();**
	}
}

**private void advanceFrame()
{
	itsCurrentFrame = Math.min(11, itsCurrentFrame + 1);
}**
```

## 3. 스트라이크인 경우를 자체 함수로 분리해보자.

---

`Game27`

```java
private void adjustCurrentFrame(int pins)
{
 if (firstThrowInFrame == true)
 {
	 if (**adjustFrameForStrike(pins)** == false)
		 firstThrowInFrame = false;
 }
 else
 {
	 firstThrowInFrame=true;
	 advanceFrame();
 }
}

**private boolean adjustFrameForStrike(int pins)
{
 if (pins == 10)
 {
	 advanceFrame();
	 return true;
 }
 return false;
}**

**private void advanceFrame()
{
	 itsCurrentFrame = Math.min(11, itsCurrentFrame + 1);
}**
```

## 4. itsCurrentFrame이 *마지막으로 던졌던 공의 프레임*을 표현하게끔 만들기

---

기존 itsCurrentFrame은 아래와 같다.

`Game`

```java
public int score()
{
 return scoreForFrame(**getCurrentFrame()-1**);
}

public int getCurrentFrame()
{
	return itsCurrentFrame;
	// 현재 프레임 번호인 itsCurrentFrame을 반환
}
private void advanceFrame()
{
	**itsCurrentFrame = Math.min(11, itsCurrentFrame + 1);**
}
```

위 코드에서는 itsCurrentFrame이 *다음 던지려는 프레임의 수를 가르키고 있으므로* score()에서 getCurrentFrame()-1 에 대한 계산을 수행한다.

itsCurrentFrame은 *다음 던지려는 프레임의 수*가 아닌 ***마지막으로 던졌던 공의 프레임***을 표현해야한다.

***제한값을 11이 아닌 10으로 바꾸고, -1을 없애버리면 해결된다!!***

즉 11을 도달하면 끝이었던 규칙에서 10까지 도달하게 되면 게임 종료하는 규칙으로 변경하면 된다는 것이다.

위 코드를 아래와 같이 수정한다.

`Game28`

```java
//Game.java----------------------------------
 public int score()
 {
	 return scoreForFrame(**itsCurrentFrame**);
 }
 private void advanceFrame()
 {
	 **itsCurrentFrame = Math.min(10, itsCurrentFrame + 1);**
 }

```

## 5. adjustCurrentFrame() ← adjustFrameForStrike() 합병

> I hate the ***side effect*** in adjustFrameForStrike(). I want to get rid of it. What do you think of
this?
> 

What kind of *side effect*??

`Game27`

```java
*// adjustFrameForStrike( )에 의한 스트라이크 판별 이후 스트라이크 처리*
private void adjustCurrentFrame(int pins)
{
	if (firstThrowInFrame == true)
	{
		if (adjustFrameForStrike(pins) == false)
			firstThrowInFrame = false;
	}
	...
}
```

- `adjustCurrentFrame( )`이 `adjustFrameForStrike( )`에 의존적임. 
결합도 측면에서 안 좋음
    - 제어 결합도(Control Coupling)
        
        : 한 모듈이 다른 모듈의 상세한 처리 절차를 알고 있어 이를 통제하는 경우나 처리 기능이 두 모듈에 분리되어 설계된 경우에 발생한다.
        
- Frame을 조정하는 측면에서 봤을 때, adjustCurrentFrame()이 adjustFrameForStrike()을 포함하는, 포괄적인 모듈임
    - adjustCurrentFrame() ← adjustFrameForStrike() 합병 가능!

adjustCurrentFrame() ← adjustFrameForStrike() 합병하여 아래와 같이 수정한다.

`Game29`

```java
private void adjustCurrentFrame(int pins)
{
	// 첫 투구이고 스트라이크인 경우 : Frame 증가
	// 두 번째 투구인 경우 : Frame 증가
	if ((**firstThrowInFrame && pins == 10) || (!firstThrowInFrame)**)
		advanceFrame();
	// 첫 투구인데 일반인 경우 : 다음 두 번째 투구를 위해 첫 투구 확인변수 토글
	else
		firstThrowInFrame = false;
}
```

## 6. adjustCurrentFrame() if 문 간소화 :: strike() 모듈화

---

`Game30`

```java
private void adjustCurrentFrame(int pins)
{
	**// 첫 투구이고 스트라이크인 경우 : Frame 증가
	// 두 번째 투구인 경우 : Frame 증가**
	if (strike(pins) || !firstThrowInFrame)
		advanceFrame();
	// 첫 투구인데 일반인 경우 : 다음 두 번째 투구를 위해 첫 투구 확인변수 토글
	else
		firstThrowInFrame = false;
}

private boolean strike(int pins)
{
	return (firstThrowInFrame && pins == 10);
}
```

## 7. adjustCurrentFrame() if 문 간소화 :: < strike() && 두번째투구 경우> 모듈화

---

`Game31`

```java
private void adjustCurrentFrame(int pins)
{
	if (lastBallInFrame(pins))
		advanceFrame();
	else
		firstThrowInFrame = false;
}

private boolean lastBallInFrame(int pins)
{
	return strike(pins) || !firstThrowInFrame;
}
```