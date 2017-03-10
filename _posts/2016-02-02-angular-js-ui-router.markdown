---
layout : post
title : Angular js UiRouter의 장단점
date : 2016-02-02
tag : [javascript, angularjs, ui-router]
---

# 1. 장점
  * 한 페이지에서 multiple views를 사용할 수 있다. 즉 페이지 하나를 여러 컴포넌트로 분할하여 개발할 수 있다.
  * state를 사용해서 app의 상태를 state로 직접적으로 표현할 수 있다. nested state는 물론이고, state의 사용으로 uri를 일일이 기억할 필요도 없다.
  * resolve dependency로 state 전환시 필요한 자원 획득을 보장받을 수 있다.

# 2. 단점
  * 기본 module이 아니기 때문에 추가적으로 기술 습득을 위한 비용이 들어간다.
  * 아직 stable한 버전이 존재하지 않는다. 현재 0.2.17이 최신버전이며, 1.0의 alpha 버전을 테스트중이다.
