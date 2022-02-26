---
title: Git branch 변경
updated: 2022-02-26 17:54
category: linux
---

[출처 - HAHWUL님 블로그](https://www.hahwul.com/2021/07/17/changing-the-github-default-branch/)

> 재작년 10월쯤 부터 Github branch의 default 값이 `master` → `main`으로 변경 되었다. `Black Lives Matter` 로 인해 master/slave 등 IT 쪽에서도 단어 관련된 이야기들이 나올 때 github에서도 이에 맞춰 변경되었다고 함.

# 1. Change default branch

- [https://github.com/settings/repositories](https://github.com/settings/repositories) 에서 변경


# 2. git command를 통한 변경

- -m flag를 통해 master branch를 main branch로 이름을 변경하여 push 한 후 기존 master branch를 삭제하면 됨

```shell
$ git branch -m master main
$ git push -u origin main
$ git branch -d master
```

# 3. github 웹 페이지를 통한 개별 변경

- github repo의 `SETTING` → `Branches` 에서 `edit` 아이콘을 눌러 rename 해버리면 됨


## ETC. 1번, 3번 방법을 통해 git branch를 변경한 경우(1)

- branch를 원격 저장소의 default branch 이름으로 변경한 후 기존 branch를 삭제하면 됨
    - 로컬 저장소의 branch명이 중복되면 안됨 주의

```shell
# 원격 저장소 branch 확인
$ git branch -r # 원격 저장소의 branch 리스트 조회
$ git branch -a # 로컬, 원격 모든 저장소의 branch 리스트 조회

# 원격 저장소의 branch 가져오기
$ git checkout -t origin/main
$ git branch -d master # 기존 master 브랜치 제거
```

## ETC. 1번, 3번 방법을 통해 git branch를 변경한 경우(2)

- 이후 local clone 경로에선 -m으로 branch를 변경하고 fetch 후 remote를 설정하여 변경한 branch를 따라가도록 설정해주면 됨

```shell
git branch -m master main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a
```