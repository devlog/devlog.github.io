---
layout: post
title:  "git cherry-pick 사용해서 remote 브랜치 커밋 가져오기"
author: "yms9654"
date:   2015-12-03
tags: 
  - git
  - cherry-pick
  - remote
  -  branch
---

## 만약 projectA에서 projectB의 커밋을 가져오고 싶은 경우

```bash
$ cd /home/you/projectA
$ git remote add projectB /home/you/projectB
$ git fetch projectB
$ git cherry-pick <first_commit>..<last_commit>
```
