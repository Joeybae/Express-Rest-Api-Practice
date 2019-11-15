# Express-Rest-Api-Practice

학습 목표

A. express app (Hello wordl) 생성

B. rest-api 만들기

  1) GET /users
  
  2) GET /users/:id
  
  3) POST /users
  
  4) DELETE /users/:id
  
  5) PUT /users/:id
  
---------------------------------------------------------------------

실습하기

참고 : http://webframeworks.kr/tutorials/nodejs/api-server-by-nodejs-02/

A. express app 생성

  1) package.json 생성
  
    # npm init -y
  
  2) app.js 생성
  
    const express = require('express');
    const app = express();

    app.get('/', (req, res) => {
      res.send('Hello World!\n');
     });

    app.listen(3000, () => {
      console.log('Example app listening on port 3000!');
    });
  
  3) express module 설치
  
    # npm install express --save
  
  4) npm start로 시작하도록 package.json에 아래와 같이 변경
  
    {
      "name": "express-rest-api-practice",
      "version": "1.0.0",
      "description": "",
      "main": "app.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "node app.js"
      },
      "keywords": [],
      "author": "",
      "license": "ISC",
      "dependencies": {
        "express": "^4.17.1"
      }
    }

  5) app 실행
  
    # npm start
  
