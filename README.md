# Carrot market clone

## stack

Next

## Next 시작하기

1. npx create-next-app@latest --typescript
2. 삭제
   - pages/api 폴더 삭제
   - styles.Home.module.css 삭제
   - index.tsx 파일에서 return 에 있는거 전부삭제
3. 테일윈드 셋업
   1. `npm install -D tailwindcss postcss autoprefixer` 설치
   2. `npx tailwindcss init -p` 초기설정파일 생성
   3. `tailwind.config.js` 파일 수정

```css
/* global.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```javascript
// tailwind.config.js
module.exports = {
  content: [
    // tailwind 를 사용한 곳
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

## Tour of Tailwind

- `tawilwind` 는 `utility-first css` 이다. (즉 테일윈드가 많은 클레스네임을 갖는 다는 말. 엄청 큰 css 파일이다.)
- 스타일을 할때 클래스 네임을 조합해서 하면 된다.
- `vscode` 의 `extension` : `Tailwind CSS intelliSense `를 설치하면 자동완성 기능을 쓸 수 있다.
- 자동완성이 안뜨면 `컨트롤+스페이스` cmd + space 를 눌러본다

### 자주쓰는 것

- flex
- p 패딩
- m 마진
- bg 백그라운드
- space 자식요소에 마진을 준다 ex) space-y-5 (자식요소에 mt-5 와 mb-5 줄수 있다.) -> tailwind 가 계산해준다 [ helper class ]
- rounded
- gap
- shadow 그림자
- font 글자 굵기
- text 글자크기, 색상, 정렬
- justify 플렉스정렬
- items 플렉스 정렬
- border 테두리
- w 너비 ex) w-2/4 는 width: 50%;
- h 높이
- overflow 오버플로우
- relative : relative 포지션
- -top 음수넣고 싶을 때 ex) -top-14 는 -14 만큼 올라감
- min 최소값 정할 때 ex) min-height-screen
- max
- aspect ex) aspect-square

### Modifier [[문서]](https://tailwindcss.com/docs/hover-focus-and-other-states)

- 일반 css 에서 마우스 호버시 효과를 주려면 `:hover` 선택자를 사용했다.
- tailwind 에서는 `modifier` 를 사용할 수 있다.
- `조건:프로퍼티` ex) `hover:text-black`

```html
<button
  className="mt-5 bg-blue-500 text-white p-3
  text-center rounded-xl w-3/4 mx-auto 
  hover:bg-teal-500 hover:text-black
  active:bg-yellow-500 focus:bg-red-500
  ">
  Checkout
</button>
```

### transition

- focus modifier
- ring utility: ex) `ring-2` , `ring-offset-2`, `ring-yellow-500`
  - 모두 앞에 `focus` 를 해줄 필요는 없다. 다른 애들은 변수에 대한 설정이므로 `focus:ring-2` 만 해도 충분

```html
<div className="space-x-2">
  <button
    className="w-5 h-5 rounded-full bg-yellow-500 focus:ring-2 ring-offset-2 ring-yellow-500 transition" />
  <button
    className="w-5 h-5 rounded-full bg-indigo-500 focus:ring-2 ring-offset-2 ring-indigo-500 transition" />
  <button
    className="w-5 h-5 rounded-full bg-teal-500 focus:ring-2 ring-offset-2 ring-teal-500 transition" />
</div>
```

![](readMeImages/2022-12-30-16-11-05.png)

### modifier for lists

```javascript
<ul>
  {[1, 2, 3, 4].map((i) => (
  <div
    key="{i}"
    className="flex justify-between my-2 odd:bg-blue-50 even:bg-yellow-50 first:bg-teal-50 last:bg-amber-50">
    <span className="text-gray-500">Grey Chair</span>
    <span className="font-semibold">$19</span>
  </div>
  ))}
</ul>
<ul>
  {["a", "b", "c", ""].map((c, i) => (
  <li className="bg-red-500 py-2 empty:bg-blue-500" key="{i}">{c}</li>
  ))}
</ul>
```

- `empty` 에 대해서도 디자인 입힐 수 있다. ex) `empty:hidden`
- hidden 을 해버리면 `display : none` 과 같다
  ![](readMeImages/image.png.png)

### Modifier for group

- 기본 css에서는 특정 컨테이너에 마우스를 올릴때 그 안의 아이템의 스타일을 변경하고 싶다면 아래처럼 했다.

```css
.container:hover .item {
}
```

- tailwind 에서는 이렇게 하면 된다.
  1. 컨테이너에 `group` 이라는 클래스명을 준다
  2. 아이템에게 group 에 마우스를 hover 하면 변화할 것을 정해준다 `group-hover:bg-red-300`

```javascript
<div className="group">
  <div className="group-hover:bg-red-300">
</div>
```

### modifier for forms

- `placeholder-shown`
- `disabled`
- `required`
- `invalid`
- `valid`

  #### peer modifier

  - input의 상태에 따라 span 의 스타일을 변경할 수 있다.
    1. input 을 정하고 `peer` 클래스명을 추가한다.
    2. peer 의 sibling 레이블에 `peer-modifier` 로 스타일을 입힌다.

```html
<form className="flex flex-col space-y-2  p-5 ">
  <input
    type="text"
    required
    placeholder="Username"
    className="border p-1 peer border-gray-400 rounded-md " />
  <span className="hidden peer-invalid:block peer-invalid:text-red-500">
    This input is invalid
  </span>
  <span className="hidden peer-valid:block peer-valid:text-teal-500">
    Awesome username
  </span>
  <span className="hidden peer-hover:block peer-hover:text-amber-500">
    Hello
  </span>
  <input type="submit" value="Login" className="bg-white" />
</form>
```

![](readMeImages/2022-12-30-16-28-26.png)

### more modifiers

#### 많은 사람들이 사용했던 패턴

- `selection` 텍스트를 드래그 할때 스타일
- `open` details 가 열렸을 떄
- `marker` ul 리스트의 왼쪽에 있는 bullet의 스타일

```javascript
<details className='selection-none open:bg-indigo-400'>
  <summary className='selection-none cursor-pointer'>details 의 제목</summary>
  <span className='selection:bg-indigo-600'>내용</span>
</details>
```

![](readMeImages/2022-12-30-17-41-26.png)

```javascript
<ul className='list-disc marker:text-teal-500'>
  <li>item</li>
  <li>item</li>
  <li>item</li>
</ul>
```

#### 파일 인풋 디자인하기

```javascript
<input type="file" className="file:border-0 file:rounded-md file:bg-purple-400" >
```

![](readMeImages/2022-12-30-17-46-28.png)

### modifier 중첩시키기

```javascript
<div className='flex flex-col space-y-2  p-5 '>
  <p className='first-letter:text-7xl first-letter:hover:text-purple-400'>
    Hello everyone!
  </p>
</div>
```

![Hello everyone!](readMeImages/2022-12-30-17-37-15.png)

### 응답형 modifier

- tailwind 는 모바일 화면을 위한 스타일이 디폴트다.
- 더 사이즈 큰 경우에 대해 따로 스타일을 설정해 줘야 한다.
- `sm` , `md`, `lg`, `xl`, `2xl`, `portrait`, `landscape`
- 적용시 오른쪽으로 갈수록 큰 사이즈에 대해 스타일을 적용한다.
- `portrait`, `landscape` 는 모바일 방향에 따른 것이다 (device simulator 에서 테스트 해볼 수 있다.)

```javascript
<div className='bg-white sm:hover:bg-pink-800 sm:bg-red-400 md:bg-teal-400 lg:bg-indigo-400 xl:bg-yellow-400 2xl:bg-pink-500  p-6 rounded-3xl shadow-xl'></div>
```

### Dark mode

- 다크모드를 어떻게 활성화시킬지에 따라 고를수 있는 옵션들이 있다
- 디폴트로 다크모드는 컴퓨터 환경에 따라 설정된다. (pc나 모바일을 다크모드로 설정했으면 브라우저도 따라간다.)

1. 브라우저 설정을 따르지 않게 하려면 `tailwind.config.js` 에 `darkmode` 옵션을 `class` 로 변경한다. (`deefault` 는 `darkMode: "media"` )
   - 이렇게 되면, `dark` 클래스를 갖는 부모요소의 모든 자식요소도 다크모드가 적용된다.
   - `class 모드`에서 **다크모드를 토글**하고 싶으면, `dark:` 선택자를 사용하는 요소의 부모요소에 `dark` 클래스를 추가하면 된다. ex) 보통은 body 나 html 태그에 추가한다
2. `dark` 를 클래스명에 넣는다

```javascript
// tailwind.config.js
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  darkMode: "class",
  plugins: [],
};
```

```javascript
      <div className="bg-white dark:bg-black flex flex-col justify-between p-6 rounded-3xl shadow-xl">
        <span className="font-semibold dark:text-white text-2xl">
          Select Item
        </span>
        <ul>
          <div className="flex justify-between my-2 ">
            <span className="text-gray-500 dark:text-gray-100">Grey Chair</span>
            <span className="font-semibold dark:text-white">$19</span>
          </div>
          <div className="flex justify-between my-2 ">
            <span className="text-gray-500 dark:text-gray-100">Grey Chair</span>
            <span className="font-semibold dark:text-white">$19</span>
```

### tailwind 는 큰파일? (JIT)

- 에전에 배포를 위해 빌드를 하면 필요 없는 css 들을 없애서 용량을 줄이는 과정을 거쳤다. 일명 purging 프로세스
- 지금은 코드를 감시하면서 실시간으로 css를 업데이트 한다

#### tailwind 가 제공하지 않는 옵션을 사용하고 싶은 경우

> `text-9xl` 보다 크게 하려면?

- `text-[500px]` 를 하면 된다! JIT 덕분

- 백그라운드 이미지 넣을 때 : `bg-[url('https://~')]`

## Tailiwnd practice

### tailwind 함수 만들기

-

```javascript
function cls(...classnames: string[]) {
  return classnames.join(" ");
}

<button
  className={cls(
    "pb-4 font-medium border-b-2 ",
    method == "email"
      ? "border-orange-500 text-orange-400"
      : "border-transparent"
  )}></button>;
```

### tailwind plugin : tainwindcss/form

1. form 의 기본 스타일을 갖게 해주는 플러그인을 추가해보자
2. `npm i @tailwindcss/forms` : input 에 reset layer 를 추가할수 있는 플러그인을 설치한다
3. `tailwind.config.js` 에서 `plugins` 배열에 추가해준다. : `plugins: [require("@tailwindcss/forms")]`

```javascript
module.exports = {
  content: [
    "./pages/**/*.{js,jsx,ts,tsx}",
    "./components/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  darkMode: "media", // class
  plugins: [require("@tailwindcss/forms")],
};
```

#### form 태그 사용해보기

1. `appearance-none` 으로 혹시 input 이 갖고 있을지 모르는 기본 스타일을 리셋 시켜보자

```javascript
<form className='flex flex-col mt-8'>
  <label className='text-sm font-medium text-gray-700'>
    {method === "email" ? "Email address" : null}
    {method === "phone" ? "Phone number" : null}
  </label>
  <div className='mt-1'>
    {method === "email" ? (
      <input
        type='email'
        className='appearance-none w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-orange-500 focus:border-orange-500'
        required
      />
    ) : null}
    {method === "phone" ? (
      <div className='flex rounded-md shadow-sm'>
        <span className='flex items-center justify-center px-3 rounded-l-md border border-r-0 border-gray-300 bg-gray-50 text-gray-500 select-none text-sm'>
          +82
        </span>
        <input
          type='number'
          className='appearance-none w-full px-3 py-2 border border-gray-300 rounded-md rounded-l-none shadow-sm placeholder-gray-400 focus:outline-none focus:ring-orange-500 focus:border-orange-500'
          required
        />
      </div>
    ) : null}
  </div>
  <button className='mt-5 bg-orange-500 hover:bg-orange-600 text-white py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium focus:ring-2 focus:ring-offset-2 focus:ring-orange-500 focus:outline-none '>
    {method === "email" ? "Get login link" : null}
    {method === "phone" ? "Get one-time password" : null}
  </button>
</form>
```

### tip: divde 클래스

- 컴포넌트 리스트에서 border-bottom 속성으로 밑줄을 그었을 때 마지막 컴포넌트의 밑줄이 안그려지게 하고싶다.
- 방법1: `last:border-b-0` 클래스 명을 추가해주면 된다.

```tsx
<div className='py-10'>
  {[1, 1, 1, 1, 1, 1, 1].map((_, i) => (
    <div
      key={i}
      className='flex px-4 cursor-pointer py-3 items-center space-x-3 border-b last:border-b-0'>
      <div className='w-12 h-12 rounded-full bg-slate-300' />
      <div>
        <p className='text-gray-700'>Steve Jebs</p>
        <p className='text-sm  text-gray-500'>
          See you tomorrow in the corner at 2pm!
        </p>
      </div>
    </div>
  ))}
</div>
```

- 방법2: 부모 요소에서 `divide` 클래스를 사용한다. (divide-y-[1px] 세로방향으로 나눠서 쌓는다. 경계는 1px)
- divide 는 요소 옆에 형제 요소가 있으면 사이에 border를 넣어준다.

```tsx
<div className='py-10 divide-y-[1px] '>
  {[1, 1, 1, 1, 1, 1, 1].map((_, i) => (
    <div
      key={i}
      className='flex px-4 cursor-pointer py-3 items-center space-x-3'>
      <div className='w-12 h-12 rounded-full bg-slate-300' />
      <div>
        <p className='text-gray-700'>Steve Jebs</p>
        <p className='text-sm  text-gray-500'>
          See you tomorrow in the corner at 2pm!
        </p>
      </div>
    </div>
  ))}
</div>
```

### tip: aspect

- `aspect-video`: 영상비율(16:9) 로 만들어준다. (가로를 반응형으로 바꾸면 세로도 알아서 조절됨)
- `aspect-square` : 정사각형 비율로 만들어준다.

## Prisma

- Prisma 는 Node.js 와 Typescript ORM 이다
- SQL 같은 데이터베이스 언어를 안쓰고 Typescript 코드만 작성할 수 있다.
- 한마디로 Prisma 는 편리하게 DB를 사용할 수 있게 해준다.

> ORM (Object Relational Mapping) 이란? <br>
> 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑해주는 것을 말한다.
> 일종의 번역기라 보면 되는데, 자바스크립트(타입스크립트) 코드와 DB 사이를 연결해주는 것이다.

- Prisma 를 사용하기 전에 schema.prisma 파일을 통해 데이터베이스가 어떻게 생겼는지 알려줘야 한다.
  > schema.prisma 는 데이터베이스에 대한 모든 설명을 담은 파일이다.

![](readMeImages/2023-01-25-14-07-22.png)

1. content String? : string 타입. required는 아님
2. published Boolean @default(false) : boolean타입. 디폴트값은 false
3. author User? @relation(fields : [authorId], references: [id]) : User타입(User는 또다른 model)

- Prisma 에 설명해주면 Prisma 는 데이터베이스의 타입을 알게되고 client 를 생성해 줄 수있다.
- client 를 이용하면 타입스크립트로 데이터베이스와 직접 상호작용할 수 있다.
- Prisma Studio 를 통해 시각화된 데이터를 읽을 수도 있다.

### Setup

1. 우선 vscode 에서 `Prisma` extension 을 설치해준다.
2. `npm i prisma -D` : prisma 설치
3. `npx prisma init` : prisma 초기 세팅
   1. .env 파일에 있는 DATABASE_URL 을 설정해준다. -> PlanetScale
   2. prisma/schema.prisma 파일에서 datasource의 provider(사용할 DB)를 설정해준다. -> MySQL
   3. DB에 사용할 model 을 만든다.

```prisma
// prisma/schema.prisma
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql" //
  url      = env("DATABASE_URL")
}

model User {
  id Int @id @default(autoincrement()) // id로 구분할수 있으며 디폴트로 증가하는 값이다.
  phone Int? @unique // 옵셔널한 Int값이며 유니크하다
  email String? @unique
  name String
  avatar String?
  createdAt DateTime @default(now()) // 현재날자를 디폴트로한다
  updatedAt DateTime @updatedAt // 업데이트마다 이 필드가 변한다
}
```

- prisma 는 schema.prisma 파일을 읽고 변경점을 deploy 한다
- schema.prisma 파일을 읽고 타입스크립트 client 를 생성해 준다.

## PlanetScale

- MySQL 과 호환되는 serverless 데이터베이스 플랫폼
- Database platform : 데이터베이스를 제공한다.
- serverless: 서버를 관리 및 유지보수할 필요가 없다
- vitess 를 사용한다: scaling 기능이 뛰어난 오픈소스 데이터베이스 클러스터링 시스템
- git 을 사용하는 것처럼 데이터베이스를 사용할 수 있는 CLI 를 제공한다.

### 설치하기

1. `brew install planetscale/tap/pscale`
2. `brew install mysql-client`
3. `brew upgrade pscale` : 최신버전으로 업데이트
4. `pscale auth login` : 계정로그인
5. 이제 CLI 기능을 이용할 수 있다.

### PlanetScale 사용하기 [문서](https://github.com/planetscale/cli#installation)

| 명령어                                           | ---                              |
| ------------------------------------------------ | -------------------------------- |
| pscale region list                               | 서버 목록을 보여준다             |
| pscale database create <DB이름>                  | DB를 생성한다                    |
| pscale database create <DB이름> -region <SLUG명> | 해당 지역 서버에 DB를 생성한다.  |
| pscale connect <DB명>                            | PlanetScale서버의 DB와 연결한다. |
| npx prisma db push                               | DB정보를 push한다                |

#### DB를 생성하고 연결하기

1. `pscale database create <DB이름> -region ap-northeast` 로 도쿄 서버에 DB를 생성하자.

> 보통 DB 플랫폼에서는 DB를 만들면 암호를 생성하고 관리해야 하는데 보안에 취약할 수 있기 때문에, 진짜DB 대신 서버를 작동시킬수 있는 가짜DB 를 사용한다. 이 후 실제 배포할 때 AWS 나 Heroku를 이용하면 된다.<br>
> planetscale 에서는 보안 tunnel 을 사용할 수 있다!(가짜 DB를 다운받고 설치하고 실행할 필요가 없고, .env 에 정보를 보관할 이유도 없다.)

2. `pscale connect <DB명>` 를 하면 PlanetScale 서버의 DB와 연결된다.
3. 연결시 나오는 주소를 복사하여 `.env` 의 DATABASE_URL 에 넣어준다.

```js
// .env
DATABASE_URL = "mysql://127.0.0.1:3306/<DB명>";
```

#### DB 를 push 해보기

> planetscale 은 mysql 과 '호환'되는 플랫폼이다. 이 호환을 위해서 몇가지 처리해줄 것이 있다.

> foreign key 제약 <br>
> planetscale 은 mysql 과 달리 foreign 키로 DB에 해당 데이터가 있는지 체크하는 기능이 없기 때문에 안정성을 보완해줘야 한다.

1. `schema.prisma` 파일의 `datasource db` 에다가 `relationMode ="prisma"` 를 추가해준다.

```prisma
// schema.prisma

datasource db {
provider = "mysql"
url = env("DATABASE_URL")
relationMode = "prisma"
}
```

2. `npx prisma db push` 를 하면 schema.prisma 를 읽고 해당 정보를 push 한다.

> Prisma Client 가 생성됨
> ![](readMeImages/2023-01-25-17-26-29.png)

3. https://app.planetscale.com/[계정명]/[DB명]/branches - [main] 브랜치 - [Schema] 탭 - [Refresh Schema] 를 눌러서 확인하면 SQL 버전의 스키마를 볼수 있다.

![](readMeImages/2023-01-25-17-29-27.png)

### Prisma Studiio

- `npx prisma studio`
  ![](readMeImages/2023-01-25-17-53-12.png)
- [Add record] 버튼을 누르면 데이터를 추가할 수 있다.

### Prisma Client

#### 클라이언트 초기화 하는 방법:

1. `npm i @prisma/client`
2. libs폴더에 `client.ts` 파일을 만들자
3. `client.ts` 파일에 다음을 작성한다.

```ts
import { PrismaClient } from "@prisma/client";

export default new PrismaClient();
```

4. 이제 client 인스턴스를 이용해서 DB와 소통할 수 있다.
5. `npx prisma generate` 를 하면 node_modules/.prisma/client/index.d.ts 에 우리의 스키마에 해당하는 type 이 생성된 걸 알 수 있다.

![](readMeImages/2023-01-25-18-02-47.png) 6. 또한 PrismaClient 의 인스턴스로 자동완성으로 user모델을 찾을 수 있는 걸 알 수 있다.

![](readMeImages/2023-01-25-18-05-30.png)

> 사용예시

```ts
// libs/client.ts
import { PrismaClient } from "@prisma/client";

const client = new PrismaClient();

client.user.create({
  data: {
    id: 12,
    name: "helo",
  },
});

export default client;
```

### API Routes

> PrismaClient 는 브라우저에서 작동하진 않는다. 왜냐면 db에 접근할수 있는 파일을 프론트엔드인 브라우저에 추가하기 때문

```ts
// pages/indes.tsx
import "../libs/client"; // 에러발생
```

- 원래는 `react`는 프론트엔드를, `node.js` 는 백엔드를 다뤄야 한다.
- 그런데, `NextJS` 는 API 를 만들기 위해 반드시 서버를 따로 구축할 필요가 없다!
- `pages/api` 폴더를 생성하면 **NextJS 서버에 API 가 생성**된다.

> api 폴더 내의 파일은 `export default` 로 함수를 내보내야 한다.

#### api 라우트 만들기

- `pages/api` 폴더에서 `connection 핸들러`인 함수를 `export default` 한다.

```ts
// pages/api/client-test.tsx
import { NextApiRequest, NextApiResponse } from "next";

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  res.json({
    ok: true,
    data: "abc",
  });
}
```

> NextApiRequest

![](readMeImages/2023-01-25-19-42-26.png)

> NextApiResponse

![](readMeImages/2023-01-25-19-43-37.png)

- /api/client-test 에 해당하는 url 에 가면 위에서 정의한 `json`이 나온다
  ![](readMeImages/2023-01-25-19-52-54.png)

> client-test 에 PrismaClient 를 import 해보자

```tsx
// pages/api/client-test.tsx

import { NextApiRequest, NextApiResponse } from "next";
import client from "../../libs/client";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  await client.user.create({
    data: {
      email: "abc@naver.com",
      name: "hello",
    },
  });
  // res.status(200).end(); 로 하면 항상 200 신호를 보낸다
  res.json({
    ok: true,
  });
}
```

> (브라우저) 핸들러가 실행되면 DB를 업데이트하고 다음 json을 리턴한다.
> ![](readMeImages/2023-01-25-19-59-20.png)

> (PrismaStudio) DB가 업데이트 된 모습
> ![](readMeImages/2023-01-25-20-00-28.png)

### 예시

> 유효한 form 에서 submit

```tsx
// enter.tsx
const [submitting, setSubmitting] = useState(false);
const { register, handleSubmit, reset } = useForm<EnterForm>();

const onValid = (data: EnterForm) => { // submit 이 유효할 경우 작동
  setSubmitting(true);
  fetch("/api/users/enter", {
    method: "POST",
    body: JSON.stringify(data),
    headers: {
      "Content-Type": "application/json",
    },
  }).then(() => {
    setSubmitting(false);
  });
};
// return (
<form
  onSubmit={handleSubmit(onValid)}
  className='flex flex-col mt-8 space-y-4'>
```

```tsx
// pages/api/users/enter.tsx
import { NextApiRequest, NextApiResponse } from "next";
import client from "../../../libs/client";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method !== "POST") {
    res.status(401).end(); // POST 가 아닌 메소드로 보내면 에러발생
  }
  console.log(req.body.email); // undefined 나올수 있음
  console.log(req.body); // {"email":"abc@naver.com"}
  res.status(200).end();
}
```

- 헤더를 빼놓고 요청할 경우 `req.body` 는 'req의 인코딩'을 기준으로 `parse`하기 때문에 `req.body.email` 로 접근하면 `undefined` 가 나온다!
- `프론트`에서 요청할 때 `headers` 를 설정해줘야만 한다 : `header : { "Content-Type" : "application/json" }`

#### 커스텀 훅 만들기 순서

1. 사용자 경험을 결정한다. 해당 훅이 무엇을 인자로 받고, 무엇을 리턴할지, 리턴값으로 로직을 어떻게 간추릴 것인지 정한다.
2. 위에서 정한 조건에 맞게 훅의 인풋과 리턴, 그리고 타입에 대해서 정의한다.
3. 훅의 로직을 정의한다.

## 상대경로에서 절대경로 바꾸기

1. `tsconfig.json` 파일에서 `baseUrl: "."` 를 추가한다 : 프로젝트의 baseUrl 을 `tsconfig.json` 파일의 위치로 함
2. `paths` 추가한다 : 파일과 컴포넌트들을 어떻게 import 할지 정한다.

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@libs/*": ["libs/*"], // "@libs/*" 라는 경로는 libs폴더를 의미한다.
      "@components/*": ["components/*"]
    }
  }
}
```

- "../../../libs/server/withHandler" -> "@libs/server/withHandler"
- "../../components/abc.ts" -> "@components/abc.ts" 로 쓸 수 있다.

## Authentication

1. 유저가 폰번호를 전송하면 DB에서 검색해서 존재하는지를 판별한다.
2. 존재하지 않으면 회원가입하고, 존재하면 정보를 DB에서 가져오자.
3. 그리고 유저를 위한 토큰(랜덤넘버)을 발급한다
4. 유저의 폰에 랜덤넘버를 보낸다
5. 유저의 프론트엔드에서는 토큰을 받을 수 있는 화면으로 변경된다.
6. 유저가 토큰을 입력하면 백엔드에서 토큰을 검색한다.
7. 토큰을 찾으면 유저 정보를 가져오고, 로그인 하게 한다.
8. 로그인 상태일 때만 보이는 화면을 구현해야 한다.
9. 어떤 유저가 API 요청을 보냈는지도 알아야 한다.
   > 토큰 model 필요.

### 유저를 검색하고 존재하면 데이터를 가져오고 없으면 생성하기

```tsx
// api/users/enter.tsx
if (email) {
  // 클라이언트를 사용해서 DB에서 email 에 해당하는 user를 검색
  user = await client.user.findUnique({
    where: {
      email,
    },
  });
  if (user) console.log("found it.");
  if (!user) {
    // 유저가 없으면 생성한다.
    console.log("Did not find. Will create.");
    user = await client.user.create({
      data: {
        name: "Anonymous",
        email,
      },
    });
  }
  console.log(user);
}
if (phone) {
  // 클라이언트를 사용해서 DB에서 phone 에 해당하는 user를 검색
  user = await client.user.findUnique({
    where: {
      phone: +phone,
    },
  });
  if (user) console.log("found it.");
  if (!user) {
    console.log("Did not find. Will create.");
    user = await client.user.create({
      data: {
        name: "Anonymous",
        phone: +phone,
      },
    });
  }
  console.log(user);
}
```

> 이렇게 DB를체크해서 있으면 가져오고 없으면 생성하는 로직은 빈번하기 때문에, 위 코드를 간단히 하는 기능이 이미 갖춰져 있다.

#### upsert : create하거나 update하거나 insert할 때 사용

```tsx
const user = await client.user.upsert({
  where: {
    ...payload,
  },
  create: {
    // 없으면 생성하기
    name: "Anonymous",
    ...payload,
  },
  update: {}, // 해당 유저를 찾을 경우 어떤 업데이트를 할 것인지
});
console.log(user);
```

## Token 로직

1. `schema.prisma` 에 Token 모델추가

```prisma
// schema.prisma
model User {
  id Int @id @default(autoincrement())
  phone Int? @unique
  email String? @unique
  name String
  avatar String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  tokens    Token[]
}
model Token {
  id        Int      @id @default(autoincrement())
  payload   String   @unique // 확인해야 하는 값. 필수이자 유일한 값
  user      User     @relation(fields: [userId], references: [id]) // userId 는 User모델의 id 임을 명시함
  userId    Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

2. 코드 작성 후 `npx prisma db push`
3. 서버도 재시작 해주자
   > planetscale 의 해당 브랜치의 schema 에 업데이트 됨
   > ![](readMeImages/2023-01-26-15-15-53.png)

```tsx
// api/users/enter.tsx
const user = phone ? { phone: +phone } : { email };
const payload = Math.floor(100000 + Math.random() * 900000) + "";
const token = await client.token.create({
  // 토큰을 생성해서 db에 올림
  data: {
    payload,
    user: {
      connectOrCreate: {
        // 생성한 유저와 토큰(payload)를 연결한다.
        where: {
          ...user,
        },
        create: {
          name: "Anonymous",
          ...user,
        },
      },
    },
  },
});
```

> (npx prisma studio)
> ![](readMeImages/2023-01-26-15-27-29.png)

> connect : 새로운 토큰을 이미 존재하는 유저와 연결<br>
> create: 새로운 토큰과 유저를 만듬<br>
> createOrCreate : 유저를 찾고 찾으면 토큰과 connect 하고 찾지 못하면 생성해준다.

## Twilio

> Twilio 는 사람들한테 문자메세지 보내기, robocall, 폰번호 숨기기, 이메일기능 등 기능이 있다.

### 시작하기

1. twilio 사이트에 가입한다
2. `ACCOUNT SID` 코드를 복사해서, `.env` 파일에 `TWILIO_SID=코드문자열` 을 넣고 저장한다.
3. `AUTH TOKEN` 코드도 복사해서, `.env` 파일에 `TWILIO_SID=코드문자열` 을 넣고 저장한다.

![](readMeImages/2023-01-26-20-35-46.png)

### 메세지 서비스

1. [Messaging] - [services] - [create messaging service]
2. `서비스 이름`을 적고 [Notify my users] 선택 - [create messaging service] 클릭
   ![](readMeImages/2023-01-26-20-38-20.png)
   ![](readMeImages/2023-01-26-20-39-33.png)

3. Sender Pool 화면이 나오는데, 우선 [Try it out]메뉴-[Get Set Up] 으로 가본다. 메세지 서비스를 시작할수 있다.
   ![](readMeImages/4.%20.png)
4. [Start setup] - [select messaging service] 하면 기본적으로 월 1달러에 폰번호를 하나 제공해준다. (trial 로 15달러가 있으니까 1년은 사용한다)
   ![](readMeImages/2023-01-26-20-43-14.png)
5. [Provision and add this number] 클릭
6. [Try SMS] 버튼 클릭하면 메세지를 보내기 위해 필요한 폰번호, api요청을 확인할 수 있는 페이지가 나온다.

![](readMeImages/2023-01-26-21-27-00.png)

- 양식을 채운 후
- [Send Test SMS] 를 누르면 핸드폰으로 메세지가 온다.

### Twilio SDK : 실제로 메세지 보내기

> 필수: 대시보드에 있는`ACCOUNT SID` 와 `AUTH TOKEN` 코드를 .env 파일에 제대로 저장했는지 확인한다.<br> > [Messaging]-[Service] 에서 Sid 코드도 .env 에 복사해야 한다. (test로 나 자신에게 보내야 하므로)
> 폰번호도 .env 파일에 추가하면 편리함

![](readMeImages/2023-01-26-23-20-57.png)

1. `npm i twilio` 설치
2. `const twilioClient = twilio(process.env.TWILIO_SID, process.env.TWILIO_TOKEN);` :twilio 를 import 한 뒤 twilio 클라이언트를 생성한다.
3. twilio 클라이언트를 생성하고 문자를 보내는 코드를 작성한다.

```tsx
import twilio from "twilio";
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";

// twilio 클라이언트를 생성
const twilioClient = twilio(process.env.TWILIO_SID, process.env.TWILIO_TOKEN);

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const { phone, email } = req.body;
  const user = phone ? { phone: +phone } : email ? { email } : null;
  if (!user) return res.status(400).json({ ok: false });
  const payload = Math.floor(100000 + Math.random() * 900000) + "";
  const token = await client.token.create({
    data: {
      payload,
      user: {
        connectOrCreate: {
          where: {
            ...user,
          },
          create: {
            name: "Anonymous",
            ...user,
          },
        },
      },
    },
  });
  if (phone) {
    // 메세지를 생성하고 보낸다.
    const message = await twilioClient.messages.create({
      messagingServiceSid: process.env.TWILIO_MSID,
      to: process.env.MY_PHONE!, // !는 확실히 존재하는 변수임을 Typescript에게 알려준다.
      body: `Your login token is ${payload}.`,
    });
    console.log(message);
  }
  return res.json({
    ok: true,
  });
}

export default withHandler("POST", handler);
```

> console.log(message) 로 출력된 값
> ![](readMeImages/2023-01-26-23-37-13.png)

### Sand Grid이메일 보내기

1. 대시보드 - [Explore Products] 메뉴 - [Email] 을 클릭하면 [SendGrid](https://sendgrid.com/solutions/email-api/) 사이트로 이동한다.
2. [Try for Free] 버튼을 누르고 회원가입을 한다.
3. [Create a Single Sender] 버튼을 누른다.

![](readMeImages/2023-01-27-00-24-07.png) 4. 이메일, 닉네임 등 필수입력사항을 기입한 뒤 입력한 메일주소로 verify 하면 된다. 5. sendgrid에서 [Email API] - [Integration Guide] - [Web API]

![](readMeImages/2023-01-27-00-32-26.png)

6. 언어선택: Node.js
7. api 키의 이름을 기입한뒤 [Create Key] 를 눌러서 api 를 생성한다.
8. 해당 api key 를 프로젝트의 .env 파일에 넣는다.
9. `npm install --save @sendgrid/mail`
10. 백엔드 코드에 `import mail from "@sendgrid/mail"` 을 적고 이메일을 보내는 코드를 작성한다.

```tsx
// api/users/enter.tsx
import mail from "@sendgrid/mail";

const email = await mail.send({
  from: "abcd@naver.com",
  to: "abcd@naver.com",
  subject: "Your Carrot Market Verification Email",
  text: `Your token is ${payload}`,
  html: `<strong>Your token is ${payload}</strong>`,
});
console.log(email);
```

> `console.log(email);` 가 찍힌 모습
> ![](readMeImages/2023-01-27-00-54-29.png)

## Token UI

> 로그인할 경우에만 UI 를 보이도록 구현하자

- DB 에서 User 가 삭제 될 경우, Token 도 같이 삭제되어야 한다. (Token 의 모델이 user 와 연결돼 있으므로 )
- @relation 에 onDelete: Cascade : 부모 레코드가 삭제되면 자식 레코드도 같이 삭제됨
- @relation 에 onDelete: SetNull : 삭제시 user값을 null로 바꾸고 token 은 내버려둠

```prisma
// @relation 에 onDelete: Cascade 를 추가한다.
model Token {
  id        Int      @id @default(autoincrement())
  payload   String   @unique
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

> 저장후 `npx prisma db push`

1. token 을 서버로 보내는 코드를 작성

> backend

```tsx
// api/users/confirm.tsx
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const { token } = req.body;
  console.log(token);
  res.status(200).end();
}

export default withHandler("POST", handler);
```

> frontend

```tsx
// pages/enter.tsx
const Enter: NextPage = () => {
  const [confirmToken, { loading: tokenLoading, data: tokenData }] =
    useMutation<MutationResult>("/api/users/confirm");
  const { register: tokenRegister, handleSubmit: tokenHandleSubmit } =
    useForm<TokenForm>();
  const onTokenValid = (validForm: TokenForm) => {
    if (tokenLoading) return;
    confirmToken(validForm);
  };

  return <>{/*보여지는 코드*/}</>;
};
```

### serverless 세션

> 백엔드에서 iron session 을 이용하여, 유저에게 쿠키를 주고 유저가 요청할 때 누구인지 알 수 있게 하자 <br>
> iron session : 서명/암호화된 쿠키를 사용하는 NodeJs 의 stateless 세션 툴이다.

#### JWT(Json Web Token) 와 다른점

- JWT 는 암호화되지 않고 서명을 하고, 서명과 함께 토큰을 보낸다.
- JWT 에서는 서명을 확인하고 신뢰하게 된다.
- iron session 은 쿠키를 암호화 하고 복호화 하는 과정이 있다.

#### 사용법

1. `npm i iron-session` 로 iron-session 설치
2. `withIronSessionApiRoute`이 함수로 핸들러(또다른 함수를 리턴하는 함수)를 감싸주면 iron session 이 req 객체 안에 요청하는 세션 유저들을 담아 보내준다.

> 적용 예시

```tsx
// pages/api/login.ts

import { withIronSessionApiRoute } from "iron-session/next";

export default withIronSessionApiRoute(
  async function loginRoute(req, res) {
    // 핸들러
    // get user from database then:
    req.session.user = {
      // req.session.user 생성
      id: 230,
      admin: true,
    };
    await req.session.save(); // 세션을 저장
    res.send({ ok: true });
  },
  {
    cookieName: "myapp_cookiename",
    password: "complex_password_at_least_32_characters_long",
    // secure: true should be used in production (HTTPS) but can't be used in development (HTTP)
    cookieOptions: {
      secure: process.env.NODE_ENV === "production",
    },
  }
);
```

3. 적용하기

```tsx
// api/users/confirm.tsx
// 주어지는 토큰에 해당하는 유저를 db에서 찾고 유저id를 세션에 저장한다.
import { withIronSessionApiRoute } from "iron-session/next";
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";

// TS에게 req.session 의 모습을 알려준다
declare module "iron-session" {
  interface IronSessionData {
    user?: {
      id: number;
    };
  }
}

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const { token } = req.body;
  // Prisma로 토큰과 일치 하는 데이터를 찾는다.
  const exists = await client.token.findUnique({
    where: {
      payload: token,
    },
  });
  if (!exists) return res.status(404).end();
  req.session.user = { // 세션객체에에 user 객체를 만들어서 넣는다.
    id: exists.userId,
  };
  await req.session.save(); // 세션을 저장한다.
  res.status(200).end();
}

export default withIronSessionApiRoute(withHandler("POST", handler), {
  cookieName: "carrotsession",
  password:// password 는 .env 파일에 넣어주자
});
```

> 세션이 쿠키에 저장된 모습
> ![](readMeImages/2023-01-27-21-19-27.png)

```tsx
// pages/api/users/me.tsx

import { withIronSessionApiRoute } from "iron-session/next";
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";

declare module "iron-session" {
  interface IronSessionData {
    user?: {
      id: number;
    };
  }
}

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  console.log(req.session.user);
  const profile = await client.user.findUnique({
    where: { id: req.session.user?.id },
  });
  res.json({
    ok: true,
    profile,
  });
}
// "GET" 메소드를 쓴다
export default withIronSessionApiRoute(withHandler("GET", handler), {
  cookieName: "carrotsession",
  password: //  ,
});

```

> `/api/users/me` url 로 접근시, `console.log(req.session.user)` 에 의해 다음 내용이 출력된다.
> ![](readMeImages/2023-01-27-21-30-08.png)

```tsx
// pages.enter.tsx
// 토큰 데이터가 ok 되면 '/' 으로 라우팅
const router = useRouter();
useEffect(() => {
  if (tokenData?.ok) {
    router.push("/");
  }
}, [tokenData, router]);
```

### NextAuth.js

> NextAuth 는 Next.js 에서 authentication 구현을 도와주는 패키지이다.<br>
> 간단한 설정만으로 인증 기능을 처리해준다. (해당 프로젝트에서 사용하진 않을 것임)

## Authorization (FE)

> 인증되지 않은 유저로부터 페이지를 보호하는 훅을 만들어보자

- 로그인하지 않은 채로 핸들러를 요청하면, id값이 없기 때문에 에러가 난다. 이런 에러에 대한 처리도 해줘야 한다.

1. 유저가 로그인 상태인지 아닌지 체크하자

```tsx
// lib/server/withHandler.ts
export interface ResponseType {
  ok: boolean;
  [key: string]: any;
}
interface ConfigType {
  method: "GET" | "POST" | "DELETE";
  handler: (req: NextApiRequest, res: NextApiResponse) => void;
  isPrivate?: boolean; // 이 값으로 로그인 여부 체크
}
export default function withHandler({
  method,
  isPrivate = true,
  handler,
}: ConfigType) {
  return async function (
    req: NextApiRequest,
    res: NextApiResponse
  ): Promise<any> {
    if (req.method !== method) {
      return res.status(405).end();
    }
    // private 이고 user 가 없으면, 에러를 발생시킨다.
    if (isPrivate && !req.session.user) {
      return res.status(401).json({ ok: false, error: "Plz log in." });
    }
    try {
      await handler(req, res);
    } catch (error) {
      console.log(error);
      return res.status(500).json({ error });
    }
  };
}
```

> 로그인하지 않은 채로 api 요청시
> ![](readMeImages/2023-01-28-22-24-29.png)

### useUser

- 유저데이터에 접근할 수 있는 훅을 만들고 각 페이지에서 데이터를 불러오자 (페이지를 개별적인 관점으로 관리)

```ts
// libs/client/useUser.ts
// 유저 정보를 가져오는 훅
import { useRouter } from "next/router";
import { useEffect, useState } from "react";

export default function useUser() {
  const [user, setUser] = useState();
  const router = useRouter();
  useEffect(() => {
    fetch("/api/users/me")
      .then((response) => response.json())
      .then((data) => {
        if (!data.ok) {
          return router.replace("/enter"); // 히스토리에 남지 않게 그냥 페이지를 교체할때 replace 쓴다
        }
        setUser(data.profile);
      });
  }, [router]);
  return user;
}
```

> 개선할 점: api 요청으로 데이터를 가져올 때 , 캐싱해서 쓰는 것이 네트워크 자원을 절약하는 데 좋다. <br>
> SWR 을 사용해 보자!

### SWR (State While Revalidate) : HTTP 캐시 무효화 전략

- 유저가 홈 화면으로 간 경우 useUser 훅으로 API 호출을 해서 정보를 가져온다. 프로필 페이지로 갔따가 다시 홈 화면으로 돌아온 경우, SWR 은 데이터를 다시 요청하지 않고 예전 데이터를 가져온다. 따라서, 로딩을 하지 않는다. 동시에 SWR 은 API 요청을 보내서 데이터 내용이 바뀌었는지 체크를 한다. 새로운 사항이 있으면 업데이트를 해준다.

> 위에 작성된 useUser 훅을 SWR 로 교체 해보자

- `npm i swr` 설치
