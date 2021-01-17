---
layout: post
title:  "Nodejs - BASE"
subtitle: "Nodejs 기초 학습"
type: "Nodejs"
blog: true
text: true
author: "Hongil Son"
post-header: true
header-img: "img/nodejs_logo.jpg"
order: 8
---

# INDEX

- [공부를 시작한 계기](#nodejs-공부를-시작하게된-계기)
- [Nodejs 설치 / 패키지 매니저(PM2)](#nodejs-설치--패키지-매니저pm2)
- [Nodejs 웹서버 만들기](#nodejs-웹서버-만들기)
- [Nodejs URL 이해](#nodejs-url-이해)
- [Nodejs 파일 읽기 / 파일 목록 확인](#nodejs-파일-읽기--파일-목록-확인)
- [Nodejs 동기 / 비동기 / 콜백](#nodejs-동기--비동기--콜백)
- [Nodejs 모듈](#nodejs-모듈)

## Nodejs 공부를 시작하게된 계기

- 2학년 2학기 [데이터베이스 수업 프로젝트🔗](https://sonhl0723.github.io/project/db_hotel_web/)를 진행하며 웹 코딩에 눈을 뜨게 되었다. <br>
배경지식이 하나도 없고 시간도 넉넉하지 않았기 때문에 투자한 시간에 비해 결과물은 형편 없었지만 웹 코딩의 매력을 느꼈다.
웹에 대해 공부를 하고 싶었지만 어떤 것을 학습해야 할지 감이 잡히지 않았고 일단 접한 경험이 있는 `nodejs`에 대해 학습해 보기로 결심했다.

## Nodejs 설치 / 패키지 매니저(PM2)
- 이미 nodejs가 설치되어 있지만 복습할 겸 설치에 대해서 찾아봤다.<br>
[nodejs.org/ko/](https://nodejs.org/ko/){: target="_blank"}에 들어가면
<img src="img/nodejs_download.jpg" width="50%">이러한 화면을 볼 수 있는데 여기서 LTS(Long Term Support-장기간 지원)를 다운받아 주면 된다.
<pre><code>node --version</code></pre>
터미널에 이 명령어를 사용하여 `nodejs`가 잘 설치되었는지 확인 가능하다.
- 생활 코딩 강좌에서는 `PM2`를 사용하여 node 프로세스를 관리하였다. `PM2`를 사용하는 이유가 무엇일까 궁금해서 구글링을 한 결과
로그 처리, 프로세스 재시작 등등 신경써야하는 여러가지 문제점들을 `PM2`로 관리할 수 있기 때문이라는 것을 알았다.
    - PM2 설치
    <pre><code>npm install pm2 -g</code></pre>
    `-g`를 해주는 이유는 글로벌 세팅, 즉 pm2의 명령어를 모든 디렉토리에서 사용가능하게 하기 위해서이다.
    - PM2 시작
    <pre><code>pm2 start main.js</code></pre>
    - PM2 리스트 확인
    <pre><code>pm2 list</code></pre>
    - PM2에 등록된 프로세스 모니터링
    <pre><code>pm2 monit</code></pre>
    - PM2 자동 reloading
    <pre><code>pm2 start main.js --watch</code></pre>
    수정 사항을 저장만 시키면 자동으로 reloading되는 어마어마한 코드다.

## Nodejs 웹서버 만들기
<img src="img/web_start.jpg" width="70%">
<pre><code>
var http = require('http');
var fs = require('fs');

</code></pre>
- 먼저 이 두 줄의 코드를 이해해 보겠다. Node.js가 내장하고 있는 http 모듈을 로드하기 위해서 `require('http')` 전역함수를 이용한다. 똑같은 흐름으로 Node.js가 내장하고 있는 File System 모듈을 로드하기 위해서 `require('fs')` 전역함수를 이용한다. 두 코드로 인해 하는 http 모듈과 File System 모듈을 사용할 수 있는 것이다.

<pre><code>
var app = http.createServer(function(request,response){
    var url = request.url;
    if(request.url == '/'){
      url = '/index.html';
    }
    if(request.url == '/favicon.ico'){
      return response.writeHead(404);
    }
    response.writeHead(200);
    response.end(fs.readFileSync(__dirname + url));
 
});
app.listen(3000);

</code></pre>
- 이제 웹 서버의 핵심이라고 할 수 있는 나머지 코드이다. node 웹 서버 애플리케이션은 `createServer`를 이용하여 웹 서버 객체를 만든다. 이 app으로 오는 http 요청마다 `createServer`에 전달된 함수가 그 때마다 호출된다는 의미이다. http 요청이 app으로 오면 node가 transaction을 다루기 위해 `request`와 `response` 객체를 전달하며 요청 핸들러 함수를 호출한다. 이러한 요청을 처리하기 위해서는 `listen` 메서드가 객체에서 호출되어야한다. 이 코드에서는 내가 사용하려는 포트 번호인 3000을 `listen`메서드에 전달하였다.<br>
`request.url` 형태로 url를 호출하게되면 `/디렉토리?id=aaaa` 형태로 받게 된다. 만약 `request.url`이 `/`이면 `/index.html`을 `url`에 넘겨주어 http 상태 코드를 200으로 설정하고 `fs` 모듈을 이용하여 현재 폴더 경로를 나타내는 `__dirname`과 `/index.html`를 결합하여 내 폴더안에 있는 `index.html`을 출력해 준다. `request.url`이 `/favicon.ico`이면 http 상태 코드를 404로 설정해주어 찾을 수 없는 페이지를 요청한다.

## Nodejs URL 이해
<img src="img/URL_under.jpg" width="70%">
> 출처: https://nodejs.org/api/url.html


## Nodejs 파일 읽기 / 파일 목록 확인

## Nodejs 동기 / 비동기 / 콜백

## Nodejs 모듈

## 출처
- [생활 코딩](https://opentutorials.org/course/3332){: target="_blank"}
- [Node.js Documentation](https://nodejs.org/dist/latest-v6.x/docs/api/documentation.html){: target="_blank"}

