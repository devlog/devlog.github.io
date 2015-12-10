---
layout: post
title:  "Rails 에서  relative url 설정 방법"
author: "yms9654"
date:   2015-12-10
categories: rails
---

Rails 에서는 기본적으로 `/`를 사용해서 어플리케이션을 돌리는데
하나의 서버에 여러개의 서비스를 올릴경우 변경이 필요할 때가 있습니다.
그럴때 사용하는 게 relative url 인데 rails 가이드에는 좀 부족한 부분이 있어서 정리해 보았습니다.

## config/application.rb 수정

```ruby
config.relative_url_root = "/app1"
```

레일즈 [가이드 문서](http://edgeguides.rubyonrails.org/configuring.html#deploy-to-a-subdirectory-relative-url-root)에는 이렇게만 나와있어서 테스트 해봤는데 제대로 동작하지 않았습니다.

## config.ru 수정

아래와 같이 수정을 해야 제대로 동작 하였습니다.

```ruby
map '/app1' do
  run Rails.application
end
```
