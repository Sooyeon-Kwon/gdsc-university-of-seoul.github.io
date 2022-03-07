---
layout: post
title:  "상태 관리, 어셈블"
authors: [LeeMir]
tags: ["Web"]
image: assets/images/post-react-state-management/thumb.png
description: "react state management"
featured: true
---
## 시작하기 전에 잠깐

### 상태 관리란?

프론트엔드의 영원한 숙제인 **상태 관리**에서 **상태**란, 사용자에게 보여줄 데이터입니다. 이 데이터가 정적인 데이터뿐이라면 웹 서비스를 개발하는 것은 `HTML`만 잘 작성하면 될 정도로 간단한 일이었을 것입니다. 그러나 그러한 서비스는 사전과 같은 서비스가 아니라면 쉽게 질릴 수밖에 없습니다.

현재의 웹 서비스들은 웹 앱이라고 부를 만큼 사용자가 누구인지, 사용자가 어떤 제스처를 입력했는지 등등 다양한 상황에 따라 데이터를 여러 방법으로 표현하고, 이런 데이터들은 SPA를 따르는 현재 웹 서비스의 특성상 **화면 깜박임 없이** 업데이트 됩니다.

여기서 **SPA**(Single Page Application) 이야기를 잠깐 하자면, JS에 있는 변수 값을 보여주는 정적인 페이지를 만들었을 때 이 변수의 값이 1초 후 바뀌는 코드를 짠다고 해서 페이지에 보이는 값이 바뀌지 않습니다. 일종의 **Observer** 패턴으로 변수의 값이 업데이트되면 자동으로 `HTML` 렌더링을 다시 하는 로직이 필요한데요, 이 렌더링을 도와주는 대표적인 라이브러리가 **React**이고, 이런식으로 새로고침(깜빡임) 없이 한 사이트 내에서 데이터와 UI들이 변하는 웹 앱을 **SPA**라고 부릅니다.

SPA를 개발할 때 주의해야 할 점은 사용자는 항상 정해진 시간에 특정한 행동을 하는 게 아니라, 언제 어떤 행동을 할지 예측할 수 없다는 것입니다. 즉 상태는 이곳저곳에서 실시간으로, 비동기적으로 예측할 수 없게 변화해 제어하기 힘들기 때문에 **상태 관리**라는 개념이 생겼고 이는 프론트엔드의 숙제 중 하나가 되었습니다.

## 시작

안녕하세요, GDSC UOS FE팀에서 코어 멤버로 활동하고 있는 이명재입니다. FE팀은 이번 2월 테크톡 및 스터디 주제로 상태 관리 라이브러리들을 각자 하나씩 맡아 공부하고, 간단한 `카운터`를 만들어서 소개하는 활동을 진행했습니다.

> 라이브러리 선정 기준은 절대적인 기준이 아닌 팀원들이 한번쯤은 이름 들어봤을 법한 것들로 선정하였고, 누가 어떤 것을 공부해올지는 사다리타기로 공정하게 정했음을 미리 말씀드립니다.

## React에서의 상태 관리

앞서 상태가 업데이트 될 때 자동으로 렌더링을 할 수 있게 도와주는 대표적인 라이브러리가 React라고 언급했습니다. React에서 상태의 값이 변경됨을 감지할 수 있는 이유는 상태를 업데이트할 때 배정문을 쓰는 게 아니라, `setState()`라는 특별한 함수를 사용하기 때문입니다. 자세히 이야기하면 길어질 것 같으니 바로 예제를 작성해보도록 하겠습니다.

```react
import React, { useState } from 'react';

const Component = ({ prop }) => {
  const [state, setState] = useState(0);
  return (
    <span>{state}</span>
  );
};

export default Component;
```

예전에는 클래스형 컴포넌트로 주로 작성했었기 때문에 `this.state`로 상태를 관리했으나, 현재는 함수형 컴포넌트와 Hook을 적극적으로 활용하는 트렌드이기 때문에 `useState()`로 예제를 작성했습니다.

이 컴포넌트가 다시 렌더링되는 조건은 다음과 같습니다.

> 1. `setState()`로 `state`가 변경되었을 때
> 2. `prop`의 값이 달라졌을 때
> 3. 부모 컴포넌트가 렌더링될 때
> 그 외 기타 등등

React를 처음 보신 분이라도 앞에서 언급했다시피 `setState()`는 `state`를 변경한 후 React에게 렌더링을 해야 한다고 알려주는 함수라고 이해하셨을 텐데, `prop`은 다소 생소하실 것 같습니다. 우리가 보는 웹 페이지는 `DOM`이라고 하는 요소들로 이루어져있는데요, 이 `DOM`들은 부모-자식처럼 상하 관계가 뚜렷한 트리 형태로 페이지를 구성합니다. (이것을 `DOM Tree`라고 부릅니다.)

![image](https://user-images.githubusercontent.com/42960217/155074132-a08dd6ec-e0cf-43f5-93ac-650633465fdb.png)

React의 컴포넌트들도 부모 컴포넌트와 자식 컴포넌트로 나뉩니다. 이들은 마치 매개변수처럼 부모에서 자식으로 상태(State)를 넘겨줄 수 있는데, 이렇게 부모에서 자식으로 넘긴 상태들을 `props`라고 부릅니다. 따라서 React에서는 부모 컴포넌트가 다시 렌더링 되면 당연히 자식 컴포넌트도 렌더링이 되겠지만, 부모가 자식에게 전달한 `props`의 값이 변경되어도 재 렌더링이 발생합니다.

그리고 부모의 상태(State)만 `props`로 전달하는 게 아니라, 부모가 부모의 부모로부터 넘겨 받은 `props`도 그대로 전달할 수 있습니다. 그래서 React로 개발하다보면 전혀 예상치 못한 곳 또는 타이밍에 렌더링이 일어나기 쉽고, 재 렌더링 횟수를 최대한 줄이는 최적화 작업을 해야 합니다. 그러면 부모가 자식한테 `props`를 넘겨주지 않으면 되지 않냐고 생각하실 수 있는데, 다음과 같은 상황이 있다면 `props` 없이는 골치 아플 것입니다.

![image](https://user-images.githubusercontent.com/42960217/154804958-8b2c73b3-f456-4b0b-a0c3-ee9f51f9deb9.png)

`Display`는 `Counter`의 `State`를 사용자에게 보여주는 컴포넌트이고, `AddButton`은 `State`를 `+1` 해주는 컴포넌트입니다. 물론 `Counter`에서 자식 컴포넌트 없이 혼자 View와 Logic을 모두 수행할 수 있지만, 그러면 `Display` 혼자 여러 역할을 맡게 되면서 코드 길이가 늘어나고, 이는 좋은 코드라고 부를 수 없게 됩니다. 좋은 코드라고 부를 수 있는, 한 컴포넌트가 명확하게 한가지 역할을 수행하게 짠다면 이렇게 여러 컴포넌트들이 공통의 상태를 공유하기 위해 부모의 상태를 공유할 일이 잦고, 이때 우리는 `props`를 이용합니다.

앞서 `DOM`들은 **트리** 형태를 이룬다고 했습니다. 개발을 진행하다보면 트리의 `depth`가 커지는 일은 불가피하고, 컴포넌트에서 항상 부모의 상태를 전달받는 게 아니라 조부모의, 조상의 컴포넌트로부터 `props`로 전달 전달 전달받아야 하는 상황과도 마주치게 됩니다. 이런 상황을 우리는 `Props Drilling`이라고 표현하며, 코드를 복잡하게 만드는 주범입니다.

> Props Drilling은 React에서 상태를 관리하는 방법이며, 그렇게 짠다고 해서 전혀 문제가 되는 코드가 아니니 오해하시면 안돼요!

`Props Drilling`의 가장 골치 아픈 점은 `props` 값이 갱신될 경우 `props`가 지나온 모든 컴포넌트가 다 같이 재 렌더링 된다는 점입니다. 컴포넌트끼리 서로 **의존성**이 생긴다고 표현하는데, 조상 컴포넌트도 말단 컴포넌트도 아닌데 `props`를 전달하기 위한 컴포넌트라는 이유만으로, 또는 상태를 전달받지 않지만 그 컴포넌트의 자식 컴포넌트라는 이유만으로 렌더링이 같이 일어나기 때문에 정신이 매우 혼미해집니다.

길게 돌아 돌아 설명했지만 `Props Drilling`으로부터 발생하는 복잡한 코드와 높은 컴포넌트 의존성을 해결하기 위해, 그리고 상태 관리를 편하게 하기 위해 **전역 상태**와 같은 개념이 생겼으며 그를 편하게 쓸 수 있게 지원하는 라이브러리들이 다양하게 만들어졌습니다.

> 이제부터는 라이브러리에 대한 소개가 이어집니다. 분량을 생각해 코드나 사용 방법은 따로 첨부하지 않을 것이며, 자세한 내용은 하단 참고 자료에 있는 저희 팀 스터디 레포를 참고해주세요!

### React의 전역 상태 관리, Context API

![image](https://miro.medium.com/max/2000/1*Ha2vNB0ILaYKPXk6oyTZSQ.png)
*왼쪽: Props Drilling / 오른쪽: Context API*

`Context API`는 라이브러리는 아니고 React에서 직접 지원하는 `Props Drilling` 해결 방법입니다. Provider와 Consumer의 개념이 있으며, Context라는 것을 공유합니다. 다만 `Props Drilling`을 근본적으로 해결한 것이 아니라, 코드 상으로만 표현이 안될 뿐이지 React 내부적으로는 `Props Drilling`이 일어나기 때문에 재 렌더링 되는 컴포넌트들은 `Props Drilling`으로 개발했을 때와 동일합니다. 따라서 컴포넌트 구조가 복잡한데 코드를 조금 더 깔끔하게 쓰고 싶을 때 적합한 방법인 것 같습니다.

### 절대 강자 Redux

![image](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

`Redux`는 `Flux`라는 디자인 패턴의 구조를 갖는 라이브러리입니다. Flux 패턴은 제가 [저번 글](https://gdsc-university-of-seoul.github.io/React_Design_Pattern/)에 잠깐 언급했었는데요, 컴포넌트 사이에 **통신**을 해서 상태를 전달한다라는 개념을 바탕으로 한 방향으로 상태를 **메시지**처럼 흘려보내 의존성을 줄이고자 했습니다.

`Redux`는 React 초창기부터 당연하다시피 같이 사용되어와서 지금까지도 많이 사용되는 것은 맞지만, [Redux 공식 홈페이지](https://redux.js.org/)에서 소개하는 문구에 "A Predictable State Container for **JS** Apps"로 언급했듯이 React 전용 라이브러리는 아닙니다. 그러다 보니 사용 방법이 React와는 조금 거리가 있고, React에서 사용하려면 순수 `Redux`가 아닌 `React-Redux`를 이용합니다.

앞서 언급했다시피 `Context API`의 경우 컴포넌트들이 모두 재 렌더링 되기 때문에 상태가 자주 바뀌어야 하는 상황에 적합하지 않지만, `Redux`는 말단에 있는 View 컴포넌트들이 Store를 직접 구독하는 방식이라 이를 어느 정도 해결했습니다. 다만 React에 맞게 Hooks를 지원한다고 해도 세팅하고 사용하는 방법이 React와는 조금 거리가 있어 공부해야 할 양이 늘어난다는 부담이 존재하고, 하나의 상태를 전역 상태로 관리하기 위해서 짜야할 코드의 양이 불필요하게 느껴질 만큼 많다는 생각도 듭니다.

이러한 문제를 해결하기 위해 요새는 `Redux`나 `React-Redux`만 사용하지 않고, `Redux Toolkit`이나 `redux-saga` 등등과 함께 사용하는 추세라고 합니다.

### JS의 봄(Spring): MobX

![image](https://user-images.githubusercontent.com/42960217/154940849-e20d9101-95c4-4756-80a4-1d7070288882.png)

`MobX`는 예전 React에서 상태 관리 라이브러리를 어떤 것을 사용해야 할지 고민할 때 `Redux`와 함께 비교되던 라이브러리입니다. `Redux`와 비슷하면서도 `Redux`에 비해 간결하고 깔끔한 느낌입니다. 이 역시 React 전용 라이브러리는 아니며, 가장 큰 특징은 객체 지향적인 구조와 함께 `Decorator`(@)를 사용하기 때문에 보고 있는 코드가 JS가 아니라 Java Spring인지 착각하게 할 정도로 JS 원툴인 저로서는 굉장히 머리가 아팠습니다.

`MobX`가 `Redux`와 흔히 비교되던 점은 간단함입니다. 하나의 상태를 만들기 위해 비교적 많은 코드를 작성해야 하는 `Redux`와 달리 `MobX`는 적은 코드로 상태 관리를 할 수 있습니다. 다만 `Redux`든 `MobX`든 이제는 Hooks를 지원하기 때문에 작성해야 하는 양이 둘 다 많이 줄었습니다. ~~물론 Recoil과 비교하면...~~

`MobX`의 치명적인 단점은 너무나 **OOP**스럽다는 점입니다. **OOP**를 잘 이해하고 있다면 `MobX`를 배우기는 쉬우나, 함수형으로 작성하는 것이 트렌드인 React와는 맞지 않습니다. Hooks를 지원하기는 하지만 컨셉이 바뀌는 것은 아니기 때문에 이질감이 들 수밖에 없었습니다.

### Of the, By the, For the React: Recoil

![image](https://user-images.githubusercontent.com/42960217/154282385-e57be9c2-4bea-4ac2-9147-3f07ec421166.png)
*왼쪽: Redux / 오른쪽: Recoil*

> Recoil은 정말 강력합니다. 개인적으로 제가 좋아하는 라이브러리라 글에 사심이 노출될 수 있습니다.

`Recoil`은 오로지 React에서 상태 관리를 위해 만들어진 라이브러리입니다. `Atom`이라는 독립적인 Store로 전역으로 관리하고 싶은 상태가 있으면 언제든지 `Atom`을 찍어내면 되며, 위 사진에서 볼 수 있다시피 트리 구조를 무시하고 말단 컴포넌트들이 각각의 `Atom`을 구독(Observe)합니다.

`Recoil`의 가장 강력한 장점은 *React 코드에 집어넣었을 때 전혀 어색함이 없다*는 점입니다. 이것은 React가 자체적으로 지원하는 `Context API`의 장점과 겹치지 않냐라고 할 수 있지만, 개인적으로는 React 기본 Hook인 `useState()`와 유사하게 사용할 수 있기 때문에 `Context API`보다 좀 더 쉽다고 느꼈습니다. 그러면서 `Context API`의 단점인 연결된 컴포넌트들이 불필요하게 재 렌더링 되는 것이 `Recoil`에서는 일어나지 않기 때문에 좀 더 매력적이었습니다.

`Recoil`이 재 렌더링을 방지할 수 있었던 이유는 사실 `Recoil`이 `Context API` + `useRef` + `useEffect`의 조합으로 만들어졌기 때문입니다. 최상위에 생성한 Store Ref를 `Context API`로 담아뒀다가, 말단 컴포넌트에서 꺼내서 Store에 접근한 다음에 `useEffect()`로 상태 값이 변하면 강제로 렌더링 할 수 있게 하는 것이 `Recoil`의 원리입니다.

다만 `Recoil`은 글을 쓰고 있는 22.02.20 기준 0.6.1이 가장 최신 버전인 아직 정식으로 출시되지 않은 라이브러리입니다. `Recoil`에 있는 일부 API들은 [공식 홈페이지](https://recoiljs.org/)에서도 불안정(UNS)하다고 언급할 만큼 정식 버전 출시까지는 좀 더 기다려야 하는 상태임에도 불구하고, 편리함과 성능 때문에 정식 출시도 하기 전에 사람들이 많이 찾는 라이브러리로 자리 잡게 되었습니다.

## 마치며

GDSC UOS FE팀에서는 매 달 테크톡을 할 때마다 색다른 방법으로 하곤 합니다. 어떤 상반되는 주제를 갖고 팀을 나눠서 토론을 진행한 적도 있고, 하나의 주제에 대해서 공부해온 다음 공부하면서 궁금했던 부분을 공유하는 등 다양하게 진행해봤는데, 이번에 하나의 큰 주제 내에서 각자 세부적인 부분을 하나씩 맡아서 소개해주는 시간을 가져보면서 이런 시간도 좋은 것 같다고 느꼈습니다.

다음에 공부해야지 하고 미루고 미루던 것들을 다른 팀원들의 발표를 들으면서 대리(?) 공부를 할 수 있었던 점이 특히 좋았던 것 같습니다. ~~가성비 최고..~~

개인적으로는 언젠가 한 번 순수 JS(Vanilla JS)로 웹 서비스 클론 코딩을 한 적이 있는데, 그때 정말 골치 아팠던 부분이 **상태 관리**였습니다. View와 Logic을 구분한 다음에 Logic에 있는 상태를 View와 동기화시키려면 Observer 패턴을 이용해야 하고, 이를 모든 컴포넌트에서 사용할 수 있게 일반 함수로 고통받으면서 만들었었는데, 그러고 나니 React의 `useState()`가 정말 혁신적인 친구였다는 걸 깨달았고 "`Recoil`은 신이야"를 외치게 된 것 같습니다. ~~아마 뭐든지 직접 만들어보라는 한 FE 마스터님의 영향이 아닐까요..~~

## 참고자료

[GDSC UOS FE Study Repo. in Feb.](https://github.com/GDSC-University-of-Seoul/2021-fall-frontend/tree/main/02_February)