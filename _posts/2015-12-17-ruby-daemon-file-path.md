---
layout: post
title:  "Ruby 스크립트로 daemon 만들때 file path 문제로 인한 삽질기"
author: "yms9654"
date:   2015-12-17
categories: ruby
---

smtp mock 서버를 ruby 스크립트로 만들어서 간단하게 사용하고 있었는데
최근에 이걸 운영환경에 적용해야 할 일이 생겼다.
그러기 위해서는 서비스로 만들어서 쉽게 시작시키고 종료시키고 상태를 모니터링 하는 방법이 필요했다.
구글링을 해보니 [Daemons](https://github.com/thuehlinger/daemons) 라는 gem이 있어서 이걸 써보기로 했다.

## 첫번째 작업

README 파일을 보니 사용법은 굉장히 간단해 보였다.
rb 파일을 아래와 같이 하나 만들면 되었다.

```ruby
# this is myserver_control.rb
require 'daemons'

Daemons.run('myserver.rb')
```

## 첫번째 오류

위에서 만든 파일을 실행해 보니 에러가 났다.
`myserver.rb`에서 `require` 하고 있는 다른 rb 파일을 찾지 못하는 오류였다.
이때부터 뭔가 path가 달라지는 조짐이 보였는데
별다른 고민 없이 구글링을 통해 `require_relative`를 이용해서 해결했다.
(참조하는 다른 rb 파일의 경로를 상대경로로 지정해주는 명령어다.)

## 두번째 오류

이젠 되겠지 하는 생각으로 다시 실행해보았다.

```
/Users/mickey/projects/smtp/vendor/bundle/gems/parseconfig-1.0.6/lib/parseconfig.rb:45:in `validate_config': Permission denied - ./config/settings.conf is not readable (Errno::EACCES)
        from /Users/mickey/projects/smtp/vendor/bundle/gems/parseconfig-1.0.6/lib/parseconfig.rb:37:in `initialize'
        from /Users/mickey/projects/smtp/smtp.rb:8:in `new'
        from /Users/mickey/projects/smtp/smtp.rb:8:in `<top (required)>'
```
털석... 이 메세지는 약간 이해가 안갔다. `Permission denied` 라니...
기존에 잘 돌던 코드이고 내 계정 아래에 있는 파일인데 왜 그런거지?

## 디버깅

침착하게 [byebug](https://github.com/deivid-rodriguez/byebug)를 사용해서 디버깅을 해보았다.
처음엔 byebug도 내가 작성한 원래 스크립트에서 사용하면 위와 같은 에러가 발생해서
`Daemons.run('myserver.rb')` 이 라인 앞에서 `byebug`를 설정했다.

```ruby
[23, 32] in /Users/mickey/projects/smtp/vendor/bundle/gems/daemons-1.2.3/lib/daemons/daemonize.rb
   23:   # because in :ontop mode, we normally want to see the output
   24:   def simulate(logfile_name = nil, app_name = nil)
   25:     $0 = app_name if app_name
   26:
   27:     # Release old working directory
=> 28:     Dir.chdir '/'
   29:
   30:     close_io
   31:
   32:     # Free STDIN and point it to somewhere sensible
(byebug)
```
빙고! 여기가 문제였다... 데몬으로 만들면서 경로를 `/`로 변경하는 코드가 있었다.
생각해보면 서비스 데몬의 경우 어떤 경로에서 실행할지 가정하면 안되니 이렇게 해야될것 같긴 하다.

## 해결 방안

어떻게 해결할지 고민하다가 Daemons에서 제공하는 예제가 있어서 좀 찾아보았다.
[예제](https://github.com/thuehlinger/daemons/blob/master/examples/run/ctrl_normal.rb) 파일을 보니 아래 처럼 코드를 수정하면 될것 같았다.

```ruby
$config = ParseConfig.new(File.join(File.dirname(__FILE__),"config/settings.conf"))
```
저런 식으로 `File.dirname(__FILE__)`을 사용하면 현재 실행하는 파일의 경로를 가져온다.

## 결론

```
smtp.rb: process with pid 13324 started.
2015-12-17 14:33:45 +0900: Starting MySmtpd...
2015-12-17 14:33:46 +0900: Server started!
```
성공적으로 서버가 올라갔다!
생각해보면 내가 잘 몰라서 고생을 한것이었다.
하지만 이런 문제의 경우 알면 쉽지만 모르면 해결방법을 찾기 위해 삽질을 좀 해야하는 문제였다.

