---
title: 'GraphQL로 영화 서버 만들기'
date: 2020-07-05 17:21:13
category: 'GraphQL'
draft: true
---

[노마드코더]<https://nomadcoders.co/graphql-for-beginners/>
GraphQL 강의를 듣고 정리한 문서.

백엔드 개발을 정말 쉽게 해주는 쿼리 언어

## 환경설정

document 폴더 생성
아래에
"movie ql" 폴더 생성
yarn init

아래를 참조하여 작업
![terminal](/images/graphql/graphql_202805.png)

깃헙 레포지토리 만들기
![레포만들기](/images/graphql/graphql_202658.png)

graphql-yoga 설치(스프링 스타터와 비슷)
yarn add graphql-yoga

참고 : yarn의 package.json 파일

```yml
{
  'name': 'movieql',
  'version': '1.0.0',
  'description': 'Movie API with Graphql',
  'main': 'index.js',
  'repository': 'https://github.com/davTaehyeok/movieql',
  'author': 'Thlim',
  'license': 'MIT',
  'dependencies': { 'graphql-yoga': '^1.14.8' },
}
```

## GraphQL로 기술적으로 해결할 수 있는 문제

1. OverFetch
   모든 유저들 이름을 웹사이트에 보여준다
   /users/get
   모든 유저 정보 포함
   (사진, 성명 등... DTO 객체로)
   DB에 사용하지 않을 영역도 받음
   `요청 정보보다 많은 정보를 서버에서 받는것`

Frontend가 DB에 사용자 명만 요청가능

2. Under-fetching
   앱을 시작하면 많은 정보 필요.
   /feed/
   /notification/
   /user/1/
   REST 하나를 완성 시 많은 소스를 요청해야함.
   한 쿼리에 원하는 정확한 정보 요청 가능.
   필요한 모든 정보를 URL에 포함
   GraphQL에 URL은 존재 X
   only one endpoint
   /grahpql
   /api

```
/feed/
/notifications/
/user/1/

이 많은 것을 one url로 받음.
```

```graphql
# request
query{
    feed{
        comments
        likeNumber
    }
    notifications{
        isRead
    }
    user{
        username
        profilePic
    }
}
# response
{
    feed: [{
        comments : 1
        likeNumber : 20
    ]},
    notifications: [{
        isRead :true
    }],
    user: {
        username : "thlim"
        profilePic : "test.png"
    }
}
```

요청 정보들의 json만 반환.

요약 :
`Over-fetching` : 객체 안에 필요없는 정보 포함
`Under-fetching` : 쿼리들을 조합해야함
필요한 정보들 모두 one url로 요청

### GraphQL Yoga로 서버 만들기

```SHELL
yarn global add nodemon
yarn add babel-node --dev
yarn global add babel-cli
```

파일 수정 시마다 서버 재시작 해줌

```yml
# package.json
{
  'name': 'movieql',
  'version': '1.0.0',
  'description': 'Movie API with GraphQL',
  'main': 'index.js',
  'repository': 'https://github.com/devtaehyeok/graphql',
  'author': 'devtaehyeok',
  'license': 'MIT',
  'dependencies': { 'graphql-yoga': '^1.18.3' },
  'scripts': { 'start': 'nodemon --exec babel-node index.js' },
}
```

이제 nodemon이 index.js 주시함.

```SHELL
yarn start
yarn add babel-cli babel-preset-env babel-preset-sta
ge-3 --dev
```

`.babelrc`

```TEXT
{
    "presets": ["env","stage-3"]
}
```

여기까지 스키마 에러만 나면 정상.

추가 :

```
강의 업로드 시점 대비 몇가지 바뀐 점이 있습니다.
(babel stage 3 가 deprecated 됐다던지... )
기본적인 세팅하실 것들은 다음과 같습니다.

npm i nodemon -D
npm i @babel/cli -D
npm i @babel/core -D
npm i @babel/node -D
npm i @babel/preset-env -D

뒤에 붙은 -D 는 --save-dev 의, i 는 install의 약어입니다.

이렇게 package.json이 설정되고 나면 강의 진행이 정상적으로 가능합니다. 참고하세요.

"The requested module 'graphql-yoga' is expected to be of type CommonJS,"
터미널에 이러한 오류가 뜨신다면...

import pkg from "graphql-yoga";
const { GraphQLServer } = pkg;
이렇게 하신다면 오류가 사라질거에요..!
```

### scheme

무엇을 받고 무엇을 줄 것인가에 대한 설명
사용자가 무엇을 할지 정의
하나는 DB로부터 정보 얻음 query
DB로 정보 보냄
쿼리는 정보를 받을 때만 쓰임
Mutation은 DB나 메모리에서 정보 변형
QUERT / MUTATION
자세한 설명과 유형을 서버에 올려둠
GraphQL 서버에서는 어떤 Mutation과 Query를 가졌는가 설명해준다.
Node.js는 Query의 기능을 프로그래밍 해야 함.

@`resolver`
view나 url 없음. 오직 Query와 Reslovers
resolver를 원하는 대로 프로그래밍 할 수 있음.
다른 디비로 메모리로 API로...

```yml
type Query {
  name: String!
  # 쿼리에 이름을 보내면 스트링을 보낸다는 설명
  # !는 필수 의미
}
```

`resolver.js`

```js
const resolvers = {
    Query : {
        name: () => "thlim"
    }
};
export default resolvers;
```

`실행 결과`
4000번에 접속해서 확인해보자.

![](/images/graphql/graphql_002450.png)
postman 같이 테스트 해주는 
모든 요청은 post (서버가 받아야함.)

`graphql/resolvers.js`
```js
const thlim = {
  name: "thLim",
  age: 28,
  gender: "male",
};

const resolvers = {
  Query: {
    name: () => "thLim",
    person: () => thlim,
  },
};
export default resolvers;



```

`graphql/schema.graphql`
```yml

type Thlim {
    name: String!
    age: Int!
    gender: String!
}


type Query {
    name: String!
    person: Thlim!
}

```

graphql은 type에서 원하는 정보를 설정해줘야함
![](/images/graphql/graphql_004221.png)

## 조금 더 복잡한 쿼리

Resolver가 약간 어려울 수 있음
DB에 가서 뭘 관찰하고 뭘 리턴하고...

Quert 
Mutation
Subscription

리스트와 한 객체 얻기
![](/images/graphql/graphql_012108.png)
참고 : DOCS 이용하기

nodemon graphql 수정은 작동안됨.

`graphql/resolvers.js`
```js 
import { people } from "./db";

const resolvers = {
  Query: {
    people: () => people,
    person: (_, { id }) => getById(id)
  }
};

export default resolvers;
```

`schema.graphql`
```GraphQL
type Person {
  id: Int!
  name: String!
  age: Int!
  gender: String!
}

type Query {
  people: [Person]!
  person(id: Int!): Person
}
```


`graphql/db.js`
```js 
export const people = [
  {
    id: "0",
    name: "Nicolas",
    age: 18,
    gender: "female"
  },
  {
    id: "1",
    name: "Jisu",
    age: 18,
    gender: "female"
  },
  {
    id: "2",
    name: "Japan Guy",
    age: 18,
    gender: "male"
  },
  {
    id: "3",
    name: "Daal",
    age: 18,
    gender: "male"
  },
  {
    id: "4",
    name: "JD",
    age: 18,
    gender: "male"
  },
  {
    id: "5",
    name: "moondaddi",
    age: 18,
    gender: "male"
  },
  {
    id: "6",
    name: "flynn",
    age: 18,
    gender: "male"
  }
];

export const getById = id => {
  const filteredPeople = people.filter(person => people.id === String(id));
  return filteredPeople[0];
};
```

### Creating Queries with Arguments 

GraphQL Resolver는 서버에서 요청을 받음
GraphQL 서버가 Query나 Mutation의 정의를 발견하면
Resolver를 찾아 함수를 실행
(_,args) => 에서 _는 object임
id도 {id} 이렇게 선언해줘야함
url parameter formdata parsebody 안해도됨 !

1. Operation에서 data가 어떻게 보일지 정의한다. (Query)
2. 질문을 해결(Resolve)하는 함수를 만든다.

### MUTATION

change of state 
DB의 상태를 바꾸는 함수.
Query와 똑같이 그냥 함수일 뿐이다!.

SCHEMA 

무슨 함수가 있는지
어떤 필수사항이 있는지 
어떤 리턴인지.

API를 다 보여줌!
완벽한 API에 대한 이해 가능

addMovie 구현
```js
// scheme
type Movie {
  id: Int!
  name: String!
  score: Int!
}

type Query {
  movies: [Movie]!
  movie(id: Int!): Movie
}

type Mutation {
  addMovie(name: String!, score: Int!): Movie!
}
// resolver
import { getMovies, getById, addMovie } from "./db";

const resolvers = {
  Query: {
    movies: () => getMovies(),
    movie: (_, { id }) => getById(id)
  },
  Mutation: {
    addMovie: (_, { name, score }) => addMovie(name, score)
  }
};

export default resolvers;

// db

export const addMovie = (name, score) => {
  const newMovie = {
    id: `${movies.length + 1}`,
    name,
    score
  };
  movies.push(newMovie);
  return newMovie;
};
```
Repository, service처럼 db에도 같은 함수 넣어줘야 함.
REST처럼 parse, argument, parameeter가 필요없다!

삭제 구현

```js

// db
export const deleteMovie = id => {
  const cleanedMovies = movies.filter(movie => movie.id !== id);
  if (movies.length > cleanedMovies.length) {
    movies = cleanedMovies;
    return true;
  } else {
    return false;
  }
  
}
// resolver
  Mutation: {
    addMovie: (_, { name, score }) => addMovie(name, score),
    deleteMovie: (_, { id}) => deleteMovie(id)
  }

// seheme
type Mutation {
  addMovie(name: String!, score: Int!): Movie!
  deleteMovie(id: Int!): Boolean!
}
//

```

## wrapping a rest api

```shell
# API URL에서 data fetch해줌
yarn install node-fetch
```

```js
// db
import fetch from "node-fetch";
const API_URL = "https://yts.am/api/v2/list_movies.json";


export const getMovies = (limit, rating) => {
  let req_url = API_URL;
  if(limit>0){
    req_url += `limit=${limit}`;
  }
  if(rating>0){
    req_url += `minimum_rating=${rating}`;
  }
  return fetch(`${API_URL}`)
    .then(res => res.json())
    .then(json => json.data.movies);
};
// resolver
import { getMovies } from "./db";
const resolvers = {
  Query: {
    movies: (_,{limit,rating}) => getMovies(limit,rating),
  }
};

export default resolvers;

// scheme
type Movie {
  id: Int!
  title: String!
  rating: Float!
  summary: String!
  language: String!
  medium_cover_image: String!
}

type Query {
  movies(limit: Int,rating:Float): [Movie]!
}
```