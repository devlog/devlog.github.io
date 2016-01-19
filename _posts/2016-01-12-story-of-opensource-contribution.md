---
layout: post
title:  "쩌리 개발자 OpenSource(Gitlab CI) 컨트리뷰터 되기"
author: "koreamic"
date:   2016-01-12
categories: story
---

## 0. Gitlab Contribution 그 뜻밖의 여정의 시작....(2015-10-15)

### [0-1 하나의 질문 하나의 대답 그리고 한명의 용자](https://github.com/gitlabhq/gitlabhq/find/master)

  ![여정시작](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/story_start.png)

### 0-2 File Finder 제가 한번 먹어보겠습니다.

  ![제가먹겠습니다.](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/jegamug.png)

## 1. 쩌리 개발자

### Github? Ruby? Rails?

  ![여정시작](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/mungmy4.png)

## 2. 쩌리 개발자의 스펙과 전략

### 2-1 Spec

- Java외 다른 language 경험 무
- Web Project 경험(유)
- OCJP/SQLD/정보처리기사
- Github 계정 생성(2015-09-01)

### 2-2 Stretagy

#### 최소한으로만 알고 가쟈!!!
- Ruby - 기본문법(다른 소스를 읽을 수 있을 정도..)
- Rails - Request <-> Response(소스를 보고 다음 소스를 찾을 수 있을 정도..)

#### 내가 만들고자 하는 페이지와 가장 유사한 페이지를 찾아라!!
> - 왼손은 언제나 ctrl-c에 올라가 있다.
> - 내 손은 무엇이든 카피할 준비가 되어 있다.
> - 마우스는 그저 거들뿐...    
> - 손은 눈보다 빠르다. 누구보다 빠르고 정확하게 붙여넣을 수 있다. 마치 내 소스인것처럼...
> - 이미 난 수천번이나 원하는 것을 적재적소에 붙여넣어 왔다.

#### 붙여 넣고, 필요없는 것은 빼고, 필요한 것은 넣고, 모하는 놈인지 모르는 것은 남겨둔다.

#### 최소한의 변경으로 기능을 구현하자.(소극적)
> - 이건 너의 것, 나의 것이 아니다.
> - 많은 변경은 그 만큼 많은 버그를 만들 확률이 높다.

## 3. 개발 완료(?) + Issue 생성

### 3-1 [Issue](https://github.com/gitlabhq/gitlabhq/issues/9854)

> Github에 file finder 기능 있는데 너네는 없넹?
>
너네 피드백 싸이트에도 이런 내용 있던데...
>
그래서 내가 만들었는데 함 볼래?
>> 그래 그래

### 3-2 기능 비교

| feature | github | 쩌리 |
|---------|--------|------|
| search logic | fuzzy search | regular expression |
| ajax | O | X |
| branch choice | X | O |
| shortcuts | O | X |
| highlight text | O | O |

## 4. [첫 PR(Pull Request)](https://github.com/gitlabhq/gitlabhq/pull/9855)

### 4-1 [참 잘햇어요.](https://github.com/gitlabhq/gitlabhq/pull/9855#issuecomment-159935277)

![참잘햇어요](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/chamjalhessuyo.jpg)

### [4-2 잘햇어 근데 너 좀 맞자 (**First Code Review**)](https://github.com/gitlabhq/gitlabhq/pull/9855#discussion-diff-45987287)

![너좀맞자](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/nujommaja.jpg)
  > - **주석 작성 스타일이 안맞아**
  > - **우린 Grit lib를 이제 쓰지 않아**
  > - **css 작성 스타일이 안맞아**
  > - 버튼 이름을 바꾸는게 좋겠어
  > - **단축키가 있었으면 좋겠어**
  > - 함수명을 바꾸는게 좋겠어
  > - 코드 스타일이 안맞아
  > - 정규표현식의 예약문자 쓰면 오류나
  > - url도 바꾸는게 좋겠어
  > - **log 찍는거 치워버려**
  > - 이 화면에서 다른 화면의 css class 쓰지마
  > - style이 필요하면 css class로 빼
  > - icon 함수가 있어 직접 넣지 말고 함수써 그리고 아이콘은 다른게 더 어울리는 것 같아
  > - 왜.. 같은 코드가 중복되어 있어?
  > - **data를 ajax로 받아오는게 어때?**
  > - **왜 사용하지 않는 지역변수를 선언하니?**
  > - page title을 변경했으면 좋겠어
  > - **format.js는 왜쓰는 거야?**
  > - semicolon 지워줘
  > - **regular expression 말고 더 강력한 fuzzy 알고리즘을 사용하는게 어때?**
  > - padding이 잘못들어간거 같아

![너좀맞자](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/guji.jpg)

### 4-3 만약 회사에서 위 기능을 개발 했다면?

--------------

### 4.4 [도움의 손길](https://github.com/gitlabhq/gitlabhq/pull/9855#issuecomment-161548943)

![첫판부터장난질이냐](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/helping-hand.jpg)

> 이거 짱짱 흥미롭다.
>
> 도움이 필요하면 알려줘.
>
> 우리가 도와 줄께
>> 아 졸라 감동적이야 고마워.
>>
>> 몇가지 문제가 있었는데 많이 해결했어
>>
>> 그런데 너네가 사용하는 lib를 사용해서 내가 원하는 데이터를 얻어오는 방법을 모르겠어
>>
>> 종은 생각 있음 알려줭.
>>> 우리가 쓰는 lib에서 니가 원하는데로 데이터 얻기 힘들면
>>>
>>> 걍 lib안에 니가 원하는데로 함수 만들어

### 4.5 Review를 받고 도움을 받으니...

![일이커지네](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/growup1.png)

## 5. [ID(koreamic)가 부끄러워지는 순간(**Sencond Code Review**)](https://github.com/gitlabhq/gitlabhq/pull/9889)

### 5.1 이정도 되면 영혼이 탈출을 시도한다.

![멘붕](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/menbung1.jpg)

> - **이거 오타아냐?**
> - **Indentation 맞춰줘**
> - **for 문의 break하는 index가 잘못됐어.**
> - **camelCase 말고 snake_case가 루비 스타일이야.**

### 5.2 왜냐고 물어보신다면...

![멘붕](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/ch_hyang_respect.jpg)

> - 콘트롤어에 체크로직을 추가해야 할꺼 같아
> - javascript test case가 필요할 꺼 같애(spinach)
> - **왜 단축키를 전역으로 설정했어?**
> - fuzzy javascript lib 파일을 다른 곳으로 옮기는게 좋겠어.
> - replace 함수 안쓰고 구현하는게 낳을꺼 같애
> - html을 string으로 만들지 말고 jquery를 사용해서 DOM으로 만들어줘(XSS)
> - event key code 체크 할때 상수로 선언하는게 낳을 꺼 같애
> - event unbind 할때 문제가 생길 수 있을 꺼 같애
> - **왜 이렇게 많은 Keycode를 체크하는거야?**
> - return 구문은 필요 없을 꺼 같애
> - css class 이름이  좀더 명확할 필요가 있어.
> - **왜 `$.proxy`가 필요하지?**
> - **왜 변수 초기화가 필요하지?**
> - highlight text 하는 로직 Atom에서 사용하는거 참고해봐
> - 이거는 controller말고 view에서 하는거 좋을 것 같애
> - 굳이 대문자로 안해도돼. 자동으로 해주거덩.
> - 이 버튼 class는 필요 없는거 같은데.
> - 굳이 체크 로직 없어도 될꺼 같애.

## 6. [Last Code Review](https://github.com/gitlabhq/gitlabhq/pull/9889#issuecomment-168679023)

> #### 개발자가 개발을 했으면 코드리뷰를 받는게 오픈소스의 이치라더라라
> #### 내가 오늘 그 코드 리뷰를 해줄테니 달게 받아라.

![벌을받는게이치라더라](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/bulbadara.png)

>> - check logic바꿔야 할꺼 같애
>> - 변수를 축약해서 쓰지 않는게 좋을꺼 같애애
>> - coffescript에서는 `=>`로 바꿀수 있어
>> - element에서부터 찾아가는게 좋을 꺼 같애
>> - id 말고 css class로 바꿀 수 있을까?
>> - `i == 20`이렇게 하는게 더 클리어할 꺼 같애
>> - coffee script에서는 return 없어도 돼
>> - XSS때문에 파라미터를 받아서 ....
>> - if문이 뒤로 가는게 더 coffee script 스러운코드야~
>> - **왜 이 함수를 오버라이드 했어?**
>>
>>> **내가 너무 비판적으로 보일지도 모르겠어.**
>>>
>>> **하지만 나 로컬에서 돌려 보고 잘돌아가는 거를 확인 했어. 그뢰잇잡이야야.**
>>>
>>> **우린 다음 릴리즈까지 기다릴 수 없어. 난 우리가 이걸 받을 준비가 다 됐다고 생각해**
>
>![리뷰후](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/hebaragi_bul.png)

## 7. [드디어.....](https://github.com/gitlabhq/gitlabhq/pull/9889#issuecomment-169640056)

![리뷰후](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/merged.jpeg)
> 아 대박 !

> 아 존나 대다나다!

> 하트

> 구레잇 웤

> 짱짱

## 8. 아직도 *쩌리* 그래도 **개발자** 앞으로는 ***쩌는 개발자***

![부끄러운줄아시오](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/buggruunjul.gif)

> **그깟 프록시의 명분이 뭐요?**
>
> **도대체 뭐길래 2만의 개발자들을 사지로 내몰라는 것이오?**
>
> **개발자라면, 고객이 저기요라 부르는 개발자라면,**
>
> **오타나고, 오류나고 빌어먹을지언정... 내 코드들을 살려야 하겠소.**
>
> **그대들이 죽고 못사는 사대의 보안보다 내 소스코드리뷰가 열갑절 백갑절은 더 소중하오.**

![그꿈내가이뤄드리리다](https://raw.githubusercontent.com/devlog/devlog.github.io/master/_images/gggumnega.jpg)

> **협업를 하늘처럼 섬기는 개발자!**
>
> **진정그것이 그대가 꿈꾸는 개발자라면,**
>
> **그 꿈.. 깃든샘이 이루어 드리리다.**
