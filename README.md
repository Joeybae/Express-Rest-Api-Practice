# Express-Rest-Api-Practice

학습 목표

A. express app (Hello world) 생성

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
  
B. Rest-api

  1) 변수 추가 - app.js
  
    let users = [
      {
        id: 1,
        name: 'alice'
      },
      {
        id: 2,
        name: 'bek'
      },
      {
        id: 3,
        name: 'chris'
      }
    ]

  2) 라우팅 설정 - app.js
  
    app.get('/users', (req, res) => {
      // 여기에 라우팅 로직을 작성하면 됩니다.
    });
  
  3) 라우팅 로직 작성 - app.js
  
    app.get('/users', (req, res) => {
      return res.json(users);
    });

  4) API 테스트 - Terminal 창
  
    # curl -X GET '127.0.0.1:3000/users' -v
    
    결과 : [{"id":1,"name":"chris"},{"id":2,"name":"tim"},{"id":3,"name":"daniel"}]%

  5) id로 객체 검색
  
    app.get('/users/:id', (req, res) => {
      const id = parseInt(req.params.id, 10);
      if (!id) {
        return res.status(400).json({error: 'Incorrect id'});
      }

      let user = users.filter(user => user.id === id)[0]
      if (!user) {
        return res.status(404).json({error: 'Unknown user'});
      }

      return res.json(user);
    });

  6) 결과 확인 - Terminal 창
  
    # curl -X GET '127.0.0.1:3000/users/1' -v
    
    결과 : {"id":1,"name":"alice"}

  7) id 입력 Error(404) 확인
  
    # curl -X GET '127.0.0.1:3000/users/4' -v

    결과 : {"error":"Unknown user"}

  8) name 입력 Error(404) 확인
  
    # curl -X GET '127.0.0.1:3000/users/alice' -v

    결과 : {"error":"Incorrect id"}

  9) 특정 id 삭제 로직 추가
  
    app.delete('/users/:id', (req, res) => {
      const id = parseInt(req.params.id, 10);
      if (!id) {
        return res.status(400).json({error: 'Incorrect id'});
      }

      const userIdx = users.findIndex(user => user.id === id);
      if (userIdx === -1) {
        return res.status(404).json({error: 'Unknown user'});
      }

      users.splice(userIdx, 1);
      res.status(204).send();
    });

  10) 특정 id 삭제
  
    # curl -X DELETE '127.0.0.1:3000/users/1' -v
    
  11) 결과 확인
  
    # curl -X GET '127.0.0.1:3000/users/1' -v

    결과 : {"error":"Unknown user"}

  12) body-parser module 설치
  
    # npm i body-parser --save
  
  13) body-parser module 실행 - app.js 아래 내용 추가
  
    # const bodyParser = require('body-parser');
    # app.use(bodyParser.json());
    # app.use(bodyParser.urlencoded({ extended: true }));
  
  14) Post로 새로운 데이터 추가
  
    //데이터 추가
    app.post('/users', (req, res) => {
      const name = req.body.name || '';
      if (!name.length) {
        return res.status(400).json({error: 'Incorrenct name'});
      }

      const id = users.reduce((maxId, user) => {
        return user.id > maxId ? user.id : maxId
      }, 0) +1;

      const newUser = {
        id: id,
        name: name,
      };

      users.push(newUser);
      return res.status(201).json(newUser);
    });
  
  15) 데이터 추가 API 통신 
  
    # curl -X POST '127.0.0.1:3000/users' -d "name=daniel" -v

    결과 : {"id":4,"name":"daniel"}

  16) User Routing Module 만들기
  
   - users 폴더 생성
    
   - users/index.js 파일 생성
    
   - index.js에 아래의 내용 추가
    
          const express = require('express');
          const router = express.Router();

          // 저장된 데이터
          let users = [
            {
              id: 1,
              name: 'alice'
            },
            {
              id: 2,
              name: 'bek'
            },
            {
              id: 3,
              name: 'chris'
            }
          ]

          //전체 데이터 조회
          router.get('/', (req, res) => {
            return res.json(users);
          });

          // id로 조회
          router.get('/:id', (req, res) => {
            const id = parseInt(req.params.id, 10);
            if (!id) {
              return res.status(400).json({error: 'Incorrect id'});
            }

            let user = users.filter(user => user.id === id)[0]
            if (!user) {
              return res.status(404).json({error: 'Unknown user'});
            }

            return res.json(user);
          });

          //데이터 삭제
          router.delete('/:id', (req, res) => {
            const id = parseInt(req.params.id, 10);
            if (!id) {
              return res.status(400).json({error: 'Incorrect id'});
            }

            const userIdx = users.findIndex(user => user.id === id);
            if (userIdx === -1) {
              return res.status(404).json({error: 'Unknown user'});
            }

            users.splice(userIdx, 1);
            res.status(204).send();
          });

          //데이터 추가
          router.post('/', (req, res) => {
            const name = req.body.name || '';
            if (!name.length) {
              return res.status(400).json({error: 'Incorrenct name'});
            }

            const id = users.reduce((maxId, user) => {
              return user.id > maxId ? user.id : maxId
            }, 0) +1;

            const newUser = {
              id: id,
              name: name,
            };

            users.push(newUser);
            return res.status(201).json(newUser); 
          });

          module.exports = router;
    
   - app.js 아래와 같이 내용 수정
    
          const express = require('express');
          const bodyParser = require('body-parser');
          const app = express();

          app.use(bodyParser.json());
          app.use(bodyParser.urlencoded({ extended: true }));
          app.use('/users', require('./users'));

          app.get('/', (req, res) => {
            res.send('Hello World!\n');
           });

          app.listen(3000, () => {
            console.log('Example app listening on port 3000!');
          });
