# 가볍게 시작해보는 Express

## 설치

가볍게 npm으로 설치해봅시다.

```
npm i express
```

## 타이핑

Javascript에선 아직 타이핑이 되어있지 않은 코드들이 대부분 입니다.

지금 활용할 express 역시 직접적으로 타이핑을 제공하지 않습니다.
고로 이를 위한 타이핑만 담긴 패키지를 따로 설치할 필요가 있습니다.

이를 해결하기위해 여러 방법이 있지만, 저는 typesync를 쓰시는게 가장 편하다고 생각합니다.

우선 설치합니다.

```sh
npm i -D typesync
```

그리고 `typesync`라는 이름으로 스크립트를 추가하고 동시에 `preinstall`에 그 스크립트를 넣어줍니다.

```json
{
  "preinstall": "npm run typesync",
  "typesync": "typesync",
}
```

이제 `npm run typesync`를 써보시면, `@types/express`라는 패키지가 새로 추가된 걸 알 수 있습니다.

이후 `npm i`를 사용해서 `@types/express`를 제대로 설치해 줍니다.


## 코드 작성

앞서 설정에선 `src`를 타입스크립트 소스코드를 넣어둘 디렉토리로 설정했으니, 디렉토리를 생성해서 다음과 같은 파일을 만들어 줍시다.

`src/app.ts`

```ts
// express를 불러옵니다.
import * as express from 'express'

// express로 앱을 만들어 줍니다.
const app = express()

// 라우팅 설정을 해줍니다.
app.use('/', function (req, res, next) {
  res.json({
    message: 'Hello!'
  })
})

// 서버를 작동시킵니다.(3000번 포트를 듣도록 합니다.)
app.listen(3000)
console.log('Serving... http://localhost:3000')
```

> `import * as express from 'express'` 보시면 바로 express를 불러는게 아니라 `* as express`와 같이 장황한 방식으로 불러오고 있습다.
>   이건 node.js는 es6의 import를 지원하지 않으므로 발생하는 문제로, node.js로 만들어진 대부분 `module.exports`를 통해 모듈들을 익스포트 시키고 있어서 발생합니다. 한마디로, `default`라는 이름으로 익스포트 시키지 않기 때문에 `import express from 'express'`와 같이 쓰는 건 불가능 합니다. 고로, `* as express` 형식으로 불러야 망가뜨리지 않고 그대로 다 불러 올 수 있습니다.
>
>   혹은, Node.js에 바로 쓰는 것처럼 `require`를 써도 됩니다. 단, `require`의 경우, 타입이나 인터페이스는 익스포트가 불가능합니다는


## 빌드

이제, 작성한 코드를 컴파일해줍시다.

```
npm run tsc
```

실행하고 나면 `build` 디렉토리에 자바스크립트 코드가 생성 된 걸 볼 수 있습니다.
그리고, `-w` 워치 옵션을 사용하였기에 코드를 수정하면 다시 자동으로 컴파일을 해줍니다.

## 실행

실행 스크립트를 추가해봅시다. 다시 `package.json`의 `scripts`로 가서 `start` 스크립트를 추가해 줍니다.

```json
{
  "start": "node build/app.js",
}
```

실행 이후 문제가 없으면 로그메세지를 보게 될 것이고, 이제 [http://localhost/3000](http://localhost/3000)에서 우리가 만든 json 응답을 볼 수 있을 겁니다.

## 개발할 때 더 나은 실행법

앞서 작성한 `src/app.ts`의 코드를 수정해보면 코드는 재생성 되지만 서버는 껏다 켜지않으면 수정이 반영되어있지 않습니다.

고로, 서버는 한번 껏다 다시 킬 필요가 있는데, 이걸 매번 수작업으로 하면 상당히 귀찮으므로, 우리의 `tsc` 스크립트처럼 편하게 재시작을 하게 해주는 라이브러리를 설치해서 쓰겠습니다.

```
npm i -D nodemon
```

다시한번 `scripts`로 돌아가서, `dev` script를 추가해줍니다.

```json
{
  "dev": "nodemon --watch build build/app.js"
}
```

서버는 타입스크립트를 컴파일해서 새로운 자바스크립트가 생성되었을 때만 이어야하므로, 보는 위치를 빌드 폴더로 둬야합니다. 그 외엔 `start` 코드와 동일하게 `build/app.js`를 실행하게 하면 됩니다.

이제 `npm run dev`로 실행 할 수 있습니다.

여기서 응답으로 내어주는 오브젝트의 메세지를 `Hi!`로 바꿔보면, 서버를 껏다 켤 필요 없이 브라우저만 새로고침하면 바뀐 결과를 볼 수 있습니다.

서버는 타입스크립트를 컴파일해서 새로운 자바스크립트가 생성되었을 때만 이어야하므로, 보는 위치를 빌드 폴더로 둬야합니다. 그 외엔 `start` 코드와 동일하게 `build/app.js`를 실행하게 하면 됩니다.

이제 `npm run dev`로 실행 할 수 있습니다.

여기서 응답으로 내어주는 오브젝트의 메세지를 `Hi!`로 바꿔보면, 서버를 껏다 켤 필요 없이 브라우저만 새로고침하면 바뀐 결과를 볼 수 있습니다.
