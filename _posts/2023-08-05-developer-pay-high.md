---
title:  "개발자 연봉은 거품인가? (1)"
categories: opinion
tags:
  - developer
  - 개발자 
  - 연봉
  - 개발자연봉
comments: true
---

사실, 이 포스트를 쓸지 말지 약간은 갈등했다. 왜냐하면 이런 주제는 각자의 이해관계에
따라 주관적 견해를 견지하기 쉬우며 나 역시도 이에 자유롭지 못하기 때문이다.
그럼에도 불구하고 이렇게 포스팅 하는 이유는 아직도 구시대적인 사고방식에 갇혀서
개발자를 일반 사무직과 같이 취급하시는 고인물들이 많기 때문이다.  
  
하도 요새 개발자 연봉이 높아졌다고 하시는 분들이 많으니 그럼 이 분들은 왜 이런 생각을
하셨을까? 1996년 쯤 닷컴버블이 시작됐을 때 1997년 말 IMF가 오고 1998년 상반기까지 약
200만명의 실업자가 발생하였다. 여기서 정부가 아주 웃긴 정책을 내놓는데 IT업계의
인력난이 심화되고 있으니 실업자들을 교육시켜서 IT업계의 인력난을 해결하겠다는 것이다.
이에 발맞추어 코딩학원들이 우후죽순 생겨나게 되고 정부는 이들 학원들의
“6개월 단기 자바 개발자 양성 코스”와 같은 여러 커리큘럼들을 전액 국비
지원하게 된다. 이렇게 탄생한 비전공 국비지원 학원출신 개발자들이 2년차, 3년차,
대리 타이틀을 달고 SI 프로젝트들에 투입하게 되었고 이들은 실제로 1년차였으니
금액을 많이 받지 않아도 더 받는 것이니 점점 개발자 임금 비용은 낮아지게 된다.  
  
**Note**: 실질적으로 우스개 소리로 이런 개발자들을 국좀이라 부르기도 했다.
국비지원출신 좀비의 약자로 좀비 프로세스는 아무일도 하지 않으면서 살아있는
프로세스를 의미한다.
{: .notice--info}
이러다 잘 짜여진 프레임워크인 Spring이 등장하였고 이를 기반으로 정부가 만든
전자정부표준프레임워크를 공공기관, 공기업 또는
정부관련 기업들이 사용하도록 사실상 강제하였고 국비지원 학원 출신 개발자들은
이제 Spring의 내부원리 및 설계를 몰라도 정부프레임워크만 공부하면
그냥 Copy & Paste와 눈동냥으로 대부분의 일을 처리할 수 있었다.
이렇게 찍어낸 개발자들이 과공급되면서 공급과 수요의 원칙에 의해 개발자의 임금은
상승 곡선을 탈 수 없었고 아래와 같이 건설현장의 일용직 노동자를 구하는 것과
같은 밈(meme)이 유행하기도 했었다.  
  
![자바 2명요.](https://www.dropbox.com/s/6ih9mm6fh26v1kx/java.png?raw=1)
  
[image source: 김국현 개발자님]
  
그렇다면 무엇이 달라졌기에 개발자의 연봉이 높아졌다고 생각들을 하는 것일까? 일단
급변하고 있는 개발 생태계를 이유를 들 수 있겠다. 90년과 2000년 초반에 C++과
자바 진영으로 나뉘었던 개발 생태계가 새로운 언어들의 출현으로 인해
양 진영의 개발자들은 갈수록 입지가 줄어들고 있으며 그나마 C++은 Game Engine. Database,
기타 코어 개발에는 아직도 건재하고 있으나 주 용도가 웹 어플리케이션인 자바는
엔터프라이즈 환경을 제외하고는 갈수록 설 자리를 잃어가고 있다.
더욱이 Learning Curve가 Spring에 비해 적고 퍼포먼스가 Java에 뒤지지 않는 언어들이
속속히 등장하여 이제는 굳이 Java를 메인으로 쓸 메리트가 떨어지는 것이 사실이다.
뒤늦게 Spring Boot로 만회해보자 했지만 이미 공은 넘어간 것으로 보이며
게임 체인저가 되기는 어려워 보인다.  

##### Stack Overflow의 2023 인기 언어 순위

![2023_인기_순위](https://www.dropbox.com/s/o875pj5vu0ity39/favorite_languages.png?raw=1)
[image source: Stack Overflow]
  
**Note: 위에서만 보면 자바가 아직 건재한 것으로 보인다. 하지만 실상은 아래 표와 같이
자바를 계속 쓰고 싶다는 비율이 현저히 낮다.**
{: .notice--warning}
  
##### Stack Overflow의 Admired and Desired
  
![admired](https://www.dropbox.com/s/025co82l0wrcfs9/admire_desired.png?raw=1)
[image source: Stack Overflow]
  
**Note**: 위 그래프에서 파란점은 desired로 언어를 배우고 싶어하는 퍼센테이지를
나타내며 빨간점은 admired로 작년에 실제로 그 언어를 사용했는데 그 언어를 계속
사용하고자 하는 퍼센테이지를 나타낸다. **자바의 desired 즉 자바 언어를 배우고 싶은
퍼센테이지는 16.53%이며 자바 언어를 사용한 사람 중에 계속 쓰고 싶어하는 퍼센테이지는
불과 44.11%로 C, PHP, Powershell 등과 어깨를 나란히 하고 있다.** Rust의 경우는 desired
30.56%이며 admired가 무려 84.66%이다.
{: .notice--info}
  
자바의 글로벌 점유율은 아래표와 같이 대략 10.5%로 보여진다.
![occupation_java](https://www.dropbox.com/s/wg1ucv0yvwm7wpx/occupation.png?raw=1)
[image source: TIOBE]
  
그럼 한국의 실정은 어떨까? Jetbrain에 의하면 한국의 Java 점유율은
53%에 달한다. 이게 정확한 수치가 아니더라도 글로벌 점유율에 비해서 엄청 높은 것은
확실해 보인다. 왜 이런 현상이 일어난 것일까? 위에서 언급한 것과 같이
전자정부표준프레임워크 때문이다. 당연히 한국의 개발자 중 다수가 자바 개발자이며 이들
대부분은 크게 노력하지 않고 변화하지 않아도 전자정부표준프레임워크 일자리들은 계속
주어지고 (공공기관, 공기업 수주) 안주할 수 있기 때문에 변화하고 있는 현재 생태계에
적응하려는 노력조차 하지 않고 있다. 그러나 개발 생태계는 예전부터 꾸준히 변화하고
있었다. 대부분 환경도 비용절감 및 운영의 편이성을 위해 On-Premise에서 클라우드로
바뀌었고 이로 인해 기존에는 서버관리팀이 따로 운영하던 것과 달리 개발과 운영이 결합된
DevOps라는 새로운 형태의 영역이 등장하게 되었고 이들은 Python, Golang과 같은 언어를
선호한다. Bitcoin 붐과 함께 등장한 블록체인 영역에서도 Solidity, Rust같은 언어를
선호하고 있으며 요즘 ChatGPT로 핫한 LLM 및 ML 영역은 아직 Python과 R이 대세이다.
범용 언어로는 Golang 및 Python이 선호되고 있으며 시스템언어로는 기존 강자인 C/C++외
Rust가 따라잡고 있다. 심지어 웹어플리케이션도 Node.js가 더 나은 선택으로 보여진다.
그나마 C/C++을 제외하면 그나마 빠르다고 인식됐던 부분들도 한국에서 주로 쓰이는
Spring/Boot + JPA 같은 경우는 심지어 Python의 FastAPI보다 느리다.
  
![fastapi](https://www.dropbox.com/s/1189ewjrphn9uu1/fastapi.png?raw=1)
  
![spring](https://www.dropbox.com/s/1ibbufd3s704xp8/spring.png?raw=1)
[image source: TechEmpower]
  
위에서 언급한 바와 같이 각각의 사업 목적과 개발 프로젝트에 따라 필요한 개발자들이
다변화되었는데 한국의 대다수는 Java 개발자이니 공급과 수요의 원칙에 따라 몸값이
올라가는 건 어쩌면 당연한 수순이다. 맨 처음 신호탄을 쏜 것은 의외로 당시 스타트업 중
하나였던 직방이였다. 직방은 2021년 개발자 초봉 6천만원을 공표하였고 이에 따라 인재를
뺏길까 두려운 네카라쿠배 역시 울며겨자 먹기로 올릴 수 밖에 없었다. 이렇게 스탠다드가
올라가다 보니 스타트업 및 중소기업에서는 수준급 개발자를 채용하기 위해서는 당연히
오버페이는 필수이며 그 외 복지 및 혜택등으로 어필해야 하니 그들 입장에서는 개발자
연봉이 높아져 보이는 건 당연한 결과인 것이다.

다음 포스트에서는 과연 실제로 높은 것인가에 대해 애기해보도록 하겠다.

---

### Reference

+ [2023 Developer Survey](https://survey.stackoverflow.co/2023/)
+ [TIOBE Index for July 2023](https://www.tiobe.com/tiobe-index/)
+ [JET BRAIN Java](https://www.jetbrains.com/ko-kr/lp/devecosystem-2021/java/)
+ [TechEmpower Web Framework Benchmark](https://www.techempower.com/benchmarks/#section=data-r21)