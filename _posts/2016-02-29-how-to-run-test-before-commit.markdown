---
layout : post
title : pre-commit hook으로 commit 직전에 테스트 돌리기
date : 2016-02-29
tag : [git, hook, pre-commit]
---

## 사용법
1. ${프로젝트경로}/.git/hooks/pre-commit 파일을 생성합니다. **(실행 권한이 없다면 추가해주세요.)**
2. pre-commit 파일을 아래의 내용으로 채워주세요.
3. 테스트용 커밋을 만들어봅니다. Running mvn clean test라고 뜨면 pre-commit로 작성한 hook이 잘 실행되는겁니다. (테스트가 실패하면 실패에 관한 메세지가 summary만 grep되어 출력됩니다.)


```bash
#!/bin/bash
 
echo "Running mvn clean test for errors"

# retrieving current working directory
CWD=`pwd`
MAIN_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
# go to main project dir
cd $MAIN_DIR/../../
# running maven clean test
MVN_RESULT=$(mvn clean test 2>&1)
if [ $? -ne 0 ]; then
  echo
  echo "${MVN_RESULT}" | ((tee /dev/fd/5 | grep -A 10 -B 2 "Reactor Summary:" >/dev/fd/4) 5>&1 | sed -n -e '/^Failed tests:/,/Tests run:.*$/ p' ) 4>&1
  echo
  echo "Error while testing the code"
  # go back to current working dir
  cd $CWD
  exit 1
fi
# go back to current working dir
cd $CWD
```

## 경고
로컬에서 돌리는 테스트가 느릴 경우, 매 커밋마다 기다리는 시간이 길어질 수도 있습니다.

## 참고
https://git-scm.com/book/uz/v2/Customizing-Git-Git-Hooks
https://gist.github.com/mallocator/34332f7a6a68d15a419c
