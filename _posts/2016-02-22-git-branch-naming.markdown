---
layout : post
title : Git Branch Naming
date : 2016-02-22
tag : [git, branch, naming, convention]
---

git에서 추천하는 브랜칭 전략은 release, master, develop, feature, bug, hotfix 총 6가지 prefix로 관리됩니다. 
다음 그림과 같은 형태입니다.

<img src="https://camo.githubusercontent.com/f011896cab0a6e086954a10d3a5132d57ca69468/687474703a2f2f662e636c2e6c792f6974656d732f3369315a336e3154316b3339327231413351306d2f676974666c6f772d6d6f64656c2e3030312e706e67"/>

git이 추천하는 branching 전략을 간소화한 프로젝트에 참여하면서 branch naming에 관해서 고민하게 되었습니다.
**제가 추천하는 명명법은 브랜치명을 표현적으로 명명하고 브랜치명으로 부터 정보를 유추할 수 있게하여 굳이 다른 문서를 참조하지 않도록 하는 것이 목적입니다.** 다음과 같은 브랜치 명명법을 추천합니다.

## 1. branch name prefix는 충분히 많아도 좋다.
예를 들어, 브랜치명을 정할 때 prefix를 표현적이지 않고 간단히 develop/ 같은 접두어를 붙인다면 이 브랜치가 develop에서 무엇을 처리하는 브랜치인지 전혀 감을 잡을 수 없습니다.
bug면 `bug/` prefix를, enhance면 `enhance/` prefix를 붙이는 것이 무엇을 위한 목적의 브랜치인지 브랜치명을 통해 쉽게 유추할 수 있도록 도와줍니다.

## 2. branch name에 부모 branch name을 포함 시키면 좋다.
develop 브랜치에서 나온 `deveolp/sub`는 develop prefix를 붙인 `develop/parent/sub`이 부모 프로젝트가 무엇인지 알 수 있어서 여러개의 sub project가 있을 경우, 관리하는 namespace가 작아지기 때문에 브랜치 관리가 쉬워집니다.

## 3. branch name에 issue번호는 필수로 넣어야 한다.
브랜치명에 이슈번호가 없다면, 브랜치에 대한 상세정보를 알기 위해 열심히 문서를 찾아보고 브랜치와 대조해봐야합니다. 하지만 이슈 번호가 브랜치명에 적혀있다면 단번에 이슈번호로 이슈 상세정보를 찾을 수 있습니다. 예를 들면, `feature/sub-1`은 `sub`의 1번 이슈라고 브랜치명만 봐도 쉽게 알 수 있습니다.

## 4. issue 번호는 부모 branch name을 prefix로 사용하는 것이 좋다.
부모 branch name을 prefix로 사용하면 특정 이슈가 어느 브랜치에서 시작된 이슈인지 쉽게 파악할 수 있습니다.

## 5. branch name에 branch를 간단히 설명할 수 있는 문구를 포함하면 좋다.
이슈 번호로 `어느 번호 이슈`에 관한 처리를 하는 브랜치인지 파악할 수 있지만, 그 정보만으로는 브랜치명만 봐서 `어떤 이슈`에 관한 처리를 하는 브랜치인지 알 수 없습니다. 간단히 `feature/parent/sub-1-optimized-string-builder`처럼 브랜치명만 보고도 이 브랜치가 어떤 이슈를 다루는지 쉽게 알 수 있습니다.

## 6. issue가 큰 규모의 branch는 sub project가 될 수 있으므로 branch directory의 자식으로 만드는 것이 좋다.
`feature/parent/sub-2-very-very-large-feature`의 경우 `feature/parent/sub-2-very-very-large-feature/main`과 같이 만들어두어야 `feature/parent/sub-2-very-very-large-feature/another-issue`처럼 확장해서 사용할 수 있습니다. 만약 `/main`과 브랜치 끝을 추가적으로 열어두지 않으면 `ref`에 directory가 아닌 file이 생성되기 때문에 확장할 수 없습니다.


## 참고 
http://stackoverflow.com/questions/273695/git-branch-naming-best-practices
