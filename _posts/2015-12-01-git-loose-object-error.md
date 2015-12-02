---
layout: post
title:  "git loose object 문제 해결하기."
author: "noname"
date:   2015-12-01
categories: git
---

###### 제가 격었던 상황을 해결한 얘기(터미널에 남아있는 로그) 입니다. 결론부터 보시려면 마지막부터 보시면 됩니다.

### 부팅이 되고 일상처럼 리파지토리 상태를 확인하기위해서 명령을 날립니다. 그런데..
( *어제 커밋메시지 작성하고 저장을 안하고 집에갔던 불길한 기분이...* )

```bash
$ git status
error: object file .git/objects/2e/f529faaaed03b2384b9f4d49a2ea95d7833894 is empty
error: object file .git/objects/2e/f529faaaed03b2384b9f4d49a2ea95d7833894 is empty
fatal: loose object 7b36029a951eacd979d24e993e020c4d018ca265 (stored in .git/objects/2e/f529faaaed03b2384b9f4d49a2ea95d7833894) is corrupt
```

여느때 처럼 변경된 파일들이 나와야 하는데, 에러가 발생했습니다. 무언가가 비어있고, 깨졌다고.
구글링을 통해 나오는 건 소스 백업하고, 다시 클론 받아라라는 소리가 대부분... 아직 머지는 물론 푸시도 안해놓은건데 그럴 수는 없었습니다.

그래서 일단 백업해두고, 이제까지 한 이력을 날리게 될테니 한번 지워볼까? 하고
깨졌다는 파일 지웠습니다.

```bash
$ ~/gitlab (feature/dowload_feedback*)
$ rm .git/objects/55/62499ea432f451cfb9a3e493d2035839a0ffd4
rm: remove write-protected regular empty file '.git/objects/55/62499ea432f451cfb9a3e493d2035839a0ffd4'? y
```

그랬더니

```bash
~/gitlab (feature/dowload_feedback*)$ git status
fatal: bad object HEAD
```

파일시스템 체크를 해보니

```bash
$ ~/gitlab (feature/dowload_feedback*)$ git fsck
Checking object directories: 100% (256/256), done.
Checking objects: 100% (102179/102179), done.
error: HEAD: invalid sha1 pointer f529faaaed03b2384b9f4d49a2ea95d7833894
error: refs/heads/feature/dowload_feedback: invalid sha1 pointer f529faaaed03b2384b9f4d49a2ea95d7833894
dangling blob a72600543530e34e935095fb6871476429b63e19
dangling commit 5ccd4c66b0b4a0b798414f97b698a050135f3bb0
dangling blob 8d01217ec409d243a313482a117deca207280b0e
dangling commit 152ca9b08592a9c5614a5586a6aca4423d602b55
dangling commit ea7be51ecdaf68a54275a448a012c82d0c77de05
dangling blob 0a81655bfd6ac9c88e3a86cdb02f927e243381a0
dangling commit 91712a97ad8b22d2ca2370ab6bc9e514de50a6d4
dangling commit 446e3704199c91ae71cdbd5e36a467a94a8e70ed
dangling commit b8fc2b8f63a57173dc498d170cf5be6cc1f6df9b
```

그렇죠... 제가 마지막 커밋을 지웠으니 포인터가 잘못된거죠.
그리고 여전히 bad object HEAD

```bash
$ ~/gitlab (feature/dowload_feedback*)$ git status
fatal: bad object HEAD
```

로그도 마찬가지로 bad object HEAD

```bash
$ ~/gitlab (feature/dowload_feedback*)$ git log
fatal: bad object HEAD
```

`bad object HEAD`로 다시 구글링 해보니 파일 하나를 add 하면 될거다라고 해서 파일 추가.

```bash
$ ~/gitlab (feature/dowload_feedback*)$ touch test
$ ~/gitlab (feature/dowload_feedback*)$ git add test
$ ~/gitlab (feature/dowload_feedback*)$ git status
fatal: bad object HEAD
$ ~/gitlab (feature/dowload_feedback*)$ git log
fatal: bad object HEAD
$ ~/gitlab (feature/dowload_feedback*)$ git commit
fatal: could not parse HEAD
```

하지만, 여전히 `bad object HEAD`

그러면 리셋해볼까?

```bash
$ ~/gitlab (feature/dowload_feedback*)$ git reset
fatal: Could not parse object 'HEAD'.
```

HEAD를 해석할 수 없다.
그러면 다른 브랜치로 옮겨볼까?

```bash
$ ~/gitlab (feature/dowload_feedback*)$ git checkout next
error: Your local changes to the following files would be overwritten by checkout:
        app/controllers/admin/etc/lego_project_feedbacks_controller.rb
        app/helpers/lego_project_feedbacks_helper.rb
        app/views/admin/etc/index.html.haml
        config/application.rb
        config/routes.rb
Please, commit your changes or stash them before you can switch branches.
Aborting
```

파일이 변경된게 있어서 안된다. 어?
그러면 파일 변경사항은 안다는건데.. 그러면 이력을 하나만 돌려볼까?

```bash
$ ~/gitlab (feature/dowload_feedback*)$ git reset HEAD@{1}
Unstaged changes after reset:
M       Gemfile
M       Gemfile.lock
M       app/controllers/admin/etc/lego_project_feedbacks_controller.rb
M       app/helpers/lego_project_feedbacks_helper.rb
M       app/services/lego_schedule_service.rb
M       app/views/admin/etc/lego_project_feedbacks/_form.html.haml
M       app/views/admin/etc/lego_project_feedbacks/index.html.haml
M       app/views/admin/etc/lego_project_feedbacks/show.html.haml
M       config/routes.rb
M       db/schema.rb
error: Trying to write ref ORIG_HEAD with nonexistent object f529faaaed03b2384b9f4d49a2ea95d7833894
error: Cannot update the ref 'ORIG_HEAD'.
$ ~/gitlab (feature/dowload_feedback*)$ git log
commit adb82e62873ac0baba315413e9f028b043a7166d
```

됬네요. 성공했습니다!!

## git `loose object` 문제 발생시 결론.
### 방법1
- 소스를 백업한다.
- 정상적인 리파지토리를 클론한다.
- 소스를 원복한다.
- 끝

### 방법2
- 깨진파일을 지운다.
- HEAD를 하나 전으로 돌린다. `git reset HEAD@{1}`
- 끝

두번째 방법이 더 간단하고, 이력도 살릴 수 있어서 좋습니다. ( *물론 안되면 방법1을 해야죠* )
