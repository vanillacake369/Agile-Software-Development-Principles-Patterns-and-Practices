# Story Points vs Task Points

# Contents

# 들어가기 앞서...

Agile Process 에 대해 ( 탄탄한 이론설명 + 실무적인 이야기 ) 를 보고 싶다면 [여기를 참고](https://brunch.co.kr/@insuk/13)해라.

여기만큼 잘 정리된 곳이 없다.

# About Agile Process

![Untitled](Story%20Points%20vs%20Task%20Points%2026b551542d034466bfa6c19e9124c187/Untitled.png)

### **Product Owner(PO)**

제품 책임자, 제품에 대한 요구사항 즉 백로그(Backlog)를 작성하는 주체다.

팀이 개발할 소프트웨어에 대한 모든 요구사항에 대한 오너쉽을 가진다. 또한 백로그가 준비가 되었으면, 어떤 백로그부터 개발을 해야 할지 우선순위를 정하는 유일한 사람이다.

팀 밖에 있는 여러  이해관계 당사자들(Stakeholders, 고객, 사용자 등)과의 대화를 하는 유일한 사람이 되어야 하며, 팀에 들어오는 외압(?)을  컨트롤할 수 있는 유일한 사람이 되어야 한다.

### Scrum Master(SM)

스크럼 마스터, 스크럼 팀의 스크럼이 잘 수행될 수 있도록 도와주는 역할이다.

스크럼 마스터는 최대한 객관적인 시각에서 스크럼에 정해진 원칙들이 팀에 잘  적용될 수 있도록 도와주고, 문제가 생겼을 때 해결하는 역할을 한다. 

여기서 말하는 문제란, 팀 구성원 간의 오해나 이해의 부족으로 인해 생기는 여러 가지 분쟁이나, 하는 일에 대한 우선순위 선정, 일이 끝났을 때 정말로 일이 잘 끝났는 지 내려진 정의를  확인해 보고 투명하게 의사결정을 할 수 있게 가이드하는 역할을 한다.

### **Team Member**

팀 구성원, 위 두 역할을 제외한 모든 팀원들을 말한다. 일반적으로 스크럼 팀은 **Cross-functional 한 롤**을 가진 사람들로 이루어져야 한다. 여기서 말하는 Cross-functional이라 함은 제품이 만들어지기 위해서 필요한 모든 인력, 개발자뿐만이 아니라 디자이너와 테스터, 제품을 만들기 위해 참여하는 모든 사람들을 뜻한다.

# What is Story Point?

<aside>
💡 **Story Point 정의**

Story points in Agile are **abstract measurements that developers use instead of hours.** 
Points are relative values, so a story with a value of four is twice as hard as a story with a value of two.  
The actual numbers don't matter — you could assign values between 1,000,000 and 5,000,000 if you want.
Instead, you want to give the team an idea of the story’s relative difficulty. Story points tell you how much effort a given story will take to resolve.

</aside>

스토리 포인트는 제품 백로그 항목 또는 기타 작업을 완전히 구현하는 데 필요한 전반적인 노력의 추정치를 표현하기 위한 측정 단위이다.

그렇다면 이것은 왜 이렇게 불리는 걸까?

## How come does it called “point”?

로버트 C 마틴 옹이 쓰신 Agile Software Development, Principles, Patterns, and Practices 에서 챕터 3의 Initial Exploration 부분을 따와봤다.

> The developers work together to estimate the stories. The estimates are relative, not absolute. We write a number of “points” on a story card to represent the relative cost of the story. We may not be sure just how much time a story point represents, but we do know that a story with eight points will take twice as long as a story with four points.
> 

위 내용을 보니 왜 “point”라는 단어를 쓰기 시작했는지 알 것 같다.

내 짐작이지만 포인트란 영어로 하나의 점이다. 

즉 점을 찍는 행위 자체를 의미한다.

*“We write a number of “points” on a story card to represent the relative cost of the story.”*의 의미는 사실 스토리 카드에서 시간과 노력이 major하게 들어가는 영역에 점을 찍는 행위 아닌가 싶다.

이렇게 하면 시간과 노력이 major하게 들어가는 point 수에 따라 4개냐 8개냐 결정된다.

즉, point의 수로 모든 작업물의 전체 시간,노력 추정치를 얻을 수 있는 것 아닌가 싶다.

# What is Task Point?

![Untitled](Story%20Points%20vs%20Task%20Points%2026b551542d034466bfa6c19e9124c187/Untitled%201.png)

*준원,성철 왈 : 스토리 세분화를 하여 점수를 통한 우선순위를 매긴 목록들*

> **Task Planning**

At the start of a new iteration, the developers and customers get together to plan. 
The developers break the stories down into development tasks. 
 A task is something that one developer can implement in 4–16 hours. 
The stories are analyzed, with the customers’ help, and the tasks are enumerated as completely as possible.

A list of the tasks is created on a flip chart, whiteboard, or some other convenient medium. Then, one by one,
the developers sign up for the tasks they want to implement. As each developer signs up for a task, he or she estimates that task in arbitrary task points. (Many developers find it helpful to use “perfect programming hours” as their task points.)

Developers may sign up for any kind of task. Database guys are not constrained to sign up for database
tasks. GUI guys can sign up for database tasks if they like. This may seem inefficient, but as you’ll see, there is a
mechanism that manages this. The benefit is obvious. The more the developers know about the whole project, the
healthier and more informed the project team is. We want knowledge of the project to spread through the team irrespective of specialty.

Each developer knows how many task points he or she managed to implement in the last iteration. This number is their personal budget. No one signs up for more points than they have in their budget.
Task selection continues until either all tasks are assigned or all developers have used their budgets. If there
are tasks remaining, then the developers negotiate with each other, trading tasks based on their various skills. If
this doesn’t make enough room to get all the tasks assigned, then the developers ask the customers to remove tasks
or stories from the iteration. If all the tasks are signed up and the developers still have room in their budgets for
more work, they ask the customers for more stories.
> 

위 내용은 마틴옹의 **Agile Software Development, Principles, Patterns, and Practices 책 Chapter 3 Planning 에 있는 Task Planning 단락을 따온 것이다.** 

위 내용 중 Task에 정의에 관한 내용을 추리자면 다음과 같다.

> The stories are analyzed, with the customers’ help, and the tasks are enumerated as completely as possible.

A list of the tasks is created on a flip chart, whiteboard, or some other convenient medium. Then, one by one,
the developers sign up for the tasks they want to implement. As each developer signs up for a task, he or she estimates that task in arbitrary task points.(Many developers find it helpful to use “perfect programming hours” as their task points.)
> 

즉, Task란 사용자와 개발자가 함께 스토리를 분할 및 세분화를 통해 주어지는 작업목록(Task)이다. 이러한 task를 화이트보드나 어떤 게시판에 작성한 뒤, 각각의 개발자들이 참여하여 가져간다. 이렇게 가져간 task는 arbitrary task point를 통해 추정한다.

** arbitrary : based on random choice or personal whim, rather than any reason or system. (번역판에서는 임의의라고 해석하였다)

여기서 중요한 점이 다수의 개발자들이 가져간 task를 추정하는 데 쓰이는 task point를 “perfect programming hours” 로 한다는 것이다. (추정컨대 여기서의 point란 점을 찍어내듯이 주요 기준치를 찍어내는 개념으로 다가가면 좋을 것 같다)

즉, task point란 “perfect programming hours”가 될 수도 있는 것이고, 얼만큼의 노력과 기술이 들어가는지도 될 수 있는 것이다.

---

# Reference

- 애자일이 무엇인가요?

[https://brunch.co.kr/@insuk/13](https://brunch.co.kr/@insuk/13)

[https://brunch.co.kr/@insuk/14](https://brunch.co.kr/@insuk/14)

- Story Point

[https://www.simplilearn.com/story-points-in-agile-article#:~:text=Story points in Agile are,and 5%2C000%2C000 if you want](https://www.simplilearn.com/story-points-in-agile-article#:~:text=Story%20points%20in%20Agile%20are,and%205%2C000%2C000%20if%20you%20want).

- Agile Software Development, Principles, Patterns, and Practices pdf

[https://dokumen.pub/agile-software-development-principles-patterns-and-practices-pearson-new-international-edition-1292025948-9781292025940.html](https://dokumen.pub/agile-software-development-principles-patterns-and-practices-pearson-new-international-edition-1292025948-9781292025940.html)