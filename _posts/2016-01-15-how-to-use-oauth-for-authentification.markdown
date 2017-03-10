---
layout : post
title : OAuth를 인증으로 쓰는법
date : 2016-01-15
tag : [oauth]
---

`OAuth`는 `authorization`의 목적으로 만들어졌습니다.
그런데 우리가 일반적으로 생각하는 인증은 `authroization`이 아니라 `authentification`입니다.
`authentification`은 사용자를 식별하는 것입니다. 그리고 `authroization`은 권한에 대한 허가입니다. 예를 들면, 사용자의 `개인정보 읽기` 권한을 허가 받으면 사용자의 개인정보를 읽을 수 있습니다.

`OAuth`는 `authorization`의 목적으로 만들어졌기 때문에 인증을 하기 위해선 `authorization`을 이용해서 `authentification`을 해야합니다.
예로 다음과 같은 방법으로 처리할 수 있습니다.

1. **사용자가 시스템에 들어왔을 때**
2. **페이스북의 OAuth api를 사용해 사용자의 고유 아이디를 획득하고**
3. **획득한 고유 아이디를 저장해두었다가**
4. **사용자가 다음에 시스템에 들어오면 다시 페이스북의 OAuth api를 사용해 고유 아이디를 획득해서 비교합니다.**

**-> 이렇게 전에 저장해두었던 고유 아이디와 새로 획득한 고유 아이디가 서로 같다면 인증에 성공했다고 할 수 있습니다.**
