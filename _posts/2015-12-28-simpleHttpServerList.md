---
layout: post
title:  "한줄로 띄우는 HTTP 서버 (각 언어별 방법 총정리)"
author: "noname"
date:   2015-12-28
categories: tip
---

## 한줄로 띄우는 HTTP 서버 
#### (각 언어별 방법 총정리)

웹서버에서 static page를 서비스하는 방법은 여러가지가 있습니다. nginx나 아파치 같은 웹서버에서 서비스하도록 설정하는 방법이 가장 안정적(?)이겠지만, 테스트가 목적이라면 운영중인 서버에서 테스트 할 수는 없고, 로컬에다가 아파치나 nginx같은 웹서버를 띄우자니, 그건 좀 맘에 안들고 할때 사용할 수 있는 방법이 있습니다. *그것도 명령어 한줄로!!* 예를 들면 파이썬에서 simplehttpserver를, 노드 패키지인 http-server를 이용할 수 있습니다. 다음 방법들은 모두 실행하는 폴더 안의 정적인 파일들을 HTTP로 서비스하는 방법을 설명해 둔 것입니다. 

### Python 2.x

```shell
$ python -m SimpleHTTPServer 8000
```

### Python 3.x

```shell
$ python -m http.server 8000
```

### Twisted <sub><sup>(Python)</sup></sub>

```shell
$ twistd -n web -p 8000 --path .
```

또는:

```shell
$ python -c 'from twisted.web.server import Site; from twisted.web.static import File; from twisted.internet import reactor; reactor.listenTCP(8000, Site(File("."))); reactor.run()'
```

### Ruby ( 1.9.2+ : 이전 버전에서는 더 복잡하게.. ) 

```shell
$ ruby -run -e httpd . -p8000
```

### adsf <sub><sup>(Ruby)</sup></sub>

```shell
$ gem install adsf   # install dependency
$ adsf -p 8000
```

### Sinatra <sub><sup>(Ruby)</sup></sub>

```shell
$ gem install sinatra   # install dependency
$ ruby -rsinatra -e'set :public_folder, "."; set :port, 8000'
```

### Perl

```shell
$ cpan HTTP::Server::Brick   # install dependency
$ perl -MHTTP::Server::Brick -e '$s=HTTP::Server::Brick->new(port=>8000); $s->mount("/"=>{path=>"."}); $s->start'
```

### Plack <sub><sup>(Perl)</sup></sub>

```shell
$ cpan Plack   # install dependency
$ plackup -MPlack::App::Directory -e 'Plack::App::Directory->new(root=>".");' -p 8000
```

### Mojolicious <sub><sup>(Perl)</sup></sub>

```shell
$ cpan Mojolicious::Lite   # install dependency
$ perl -MMojolicious::Lite -MCwd -e 'app->static->paths->[0]=getcwd; app->start' daemon -l http://*:8000
```

### http-server <sub><sup>(Node.js)</sup></sub>

```shell
$ npm install -g http-server   # install dependency
$ http-server -p 8000
```

*Note: http-server은 상대경로를 좀 특이하게 취급한다고 하네요. 예를들어 `test/index.html` 파일이 있다면 `index.html`파일을 로드 하고 /test로 접속하지만, 상대경로를 `/`에서 오는 것처럼 취급한다고 합니다. 제가 실제로 확인해보지는 않았습니다. 

### node-static <sub><sup>(Node.js)</sup></sub>

```shell
$ npm install -g node-static   # install dependency
$ static -p 8000
```

### PHP <sub><sup>(>= 5.4)</sup></sub>

```shell
$ php -S 127.0.0.1:8000
```

### Erlang

```shell
$ erl -s inets -eval 'inets:start(httpd,[{server_name,"NAME"},{document_root, "."},{server_root, "."},{port, 8000},{mime_types,[{"html","text/html"},{"htm","text/html"},{"js","text/javascript"},{"css","text/css"},{"gif","image/gif"},{"jpg","image/jpeg"},{"jpeg","image/jpeg"},{"png","image/png"}]}]).'
```

### busybox httpd

```shell
$ busybox httpd -f -p 8000
```


### webfs

```shell
$ webfsd -F -p 8000
```


### IIS Express

```shell
C:\> "C:\Program Files (x86)\IIS Express\iisexpress.exe" /path:C:\MyWeb /port:8000
```


###### 출처 : [willurd/web-servers.md](https://gist.github.com/willurd/5720255)
###### 여기에 없는 기발한 방법을 알고 계시다면 위에 페이지에 추가해 주세요~
