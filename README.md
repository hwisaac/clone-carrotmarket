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
| npx prisma studio                                | prisma studio 열기               |

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

> `planetscale` 은 `mysql` 과 '호환'되는 플랫폼이다. 이 호환을 위해서 몇가지 처리해줄 것이 있다.

> `foreign key` 제약 <br> > `planetscale` 은 `mysql` 과 달리 `foreign` 키로 DB에 해당 데이터가 있는지 체크하는 기능이 없기 때문에 안정성을 보완해줘야 한다.

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
- `const {data, error} = useSWR("URL_key", fetcherFn)` URL_key 는 API요청 URL 이자 캐싱할때의 key 값이다. 즉 동일한 url 으로 요청을 보내면 동일한 캐시 데이터를 사용하게 된다.

```ts
// libs/client/useUser.ts
import { useRouter } from "next/router";
import { useEffect } from "react";
import useSWR from "swr";

export default function useUser() {
  const { data, error } = useSWR("/api/users/me");
  const router = useRouter();
  useEffect(() => {
    if (data && !data.ok) {
      router.replace("/enter");
    }
  }, [data, router]);
  return { user: data?.profile, isLoading: !data && !error };
}
```

## 참고

> Next 가 리로드 될때마다 Prisma Client 가 생성되어 DB에 연결되길 반복되는데, DB 에 Prisma Client 가 무한정 연결될 수는 없다. <br>

```ts
import { PrismaClient } from "@prisma/client";

declare global {
  var client: PrismaClient | undefined;
}

// client 는 global object 에 저장돼 있는 client 를 사용한다.
// 만약 없으면 새로운 PrismaClient 를 생성한다.
const client = global.client || new PrismaClient();

// 개발환경일 경우 global.client 에 client 를 저장
if (process.env.NODE_ENV === "development") global.client = client;

export default client;
```

- 처음 서버가 실행되면 new PrismaClient 가 client 와 global.client 에 저장된다.
- 만약 리로드 되면, global.client 가 client 가 된다.

## Products

- 홈화면의 제품 정보를 DB에서 가져와서 보여주자

1. Model
2. DB
3. Mutation
4. SWR

```prisma
// schema.prisma
model User {
  id        Int       @id @default(autoincrement())
  phone     String?   @unique
  email     String?   @unique
  name      String
  avatar    String?
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  tokens    Token[]
  products  Product[]
}

model Product {
  id          Int      @id @default(autoincrement())
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  user        User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId      Int
  image       String
  name        String
  price       Int
  description String   @db.MediumText // 문자길이를 좀 늘려주자. 기본 191자밖에 안됨
}
```

### products api

> product를 업로드 하고 전체 products 를 home 화면에 출력하기

```tsx
// api/products
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  // 같은 URL 에 GET과 POST 둘다 있어야 한다.
  if (req.method === "GET") {
    const products = await client.product.findMany({}); //전부 가져오기
    res.json({
      ok: true,
      products,
    });
  }
  if (req.method === "POST") {
    // product 업로드
    const {
      body: { name, price, description },
      session: { user },
    } = req;
    const product = await client.product.create({
      data: {
        name,
        price: +price,
        description,
        image: "xx",
        user: {
          connect: {
            id: user?.id,
          },
        },
      },
    });
    res.json({
      ok: true,
      product,
    });
  }
}

export default withApiSession(
  withHandler({
    methods: ["GET", "POST"],
    handler,
  })
);
```

```tsx
// pages/products/upload.tsx

import { useEffect } from "react";
import { Product } from "@prisma/client";
import { useRouter } from "next/router";

interface UploadProductMutation {
  ok: boolean;
  product: Product; // prisma 가 만들어준 타입
}

const Upload: NextPage = () => {
  const router = useRouter();
  const { register, handleSubmit } = useForm<UploadProductForm>();
  const [uploadProduct, { loading, data }] =
    useMutation<UploadProductMutation>("/api/products"); // useMutation 은 POST 메소드를 사용한다.
  const onValid = (data: UploadProductForm) => {
    if (loading) return;
    uploadProduct(data);
  };
  useEffect(() => {
    if (data?.ok) {
      router.push(`/products/${data.product.id}`);
    }
  }, [data, router]);
  // ...
```

![](readMeImages/2023-01-29-19-51-20.png)
![](readMeImages/2023-01-29-19-53-01.png)

```ts
// pages/index.tsx
import useSWR from "swr";
import { Product } from "@prisma/client";

interface ProductsResponse {
  ok: boolean;
  products: Product[];
}

const Home: NextPage = () => {
  const { user, isLoading } = useUser();
  const { data } = useSWR<ProductsResponse>("/api/products"); // useSWR 은 GET 메소드를 사용
  console.log(data)
  return (
    <Layout title='홈' hasTabBar>
      <Head>
        <title>Home</title>
      </Head>
      <div className='flex flex-col space-y-5 divide-y'>
        {data?.products?.map((product) => (
          <Item
            id={product.id}
            key={product.id}
            title={product.name}
            price={product.price}
            comments={1}
            hearts={1}
          />
        ))}
        // ...
```

> console.log(data)
> ![](readMeImages/2023-01-29-20-11-03.png)

### product detail

> 상품 상세 페이지를 구현하자.

```ts
// pages/api/products/[id].ts

import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  // 프론트에 router.query 가 있듯이 백엔드에는 req.query 가 있음
  const { id } = req.query;
  const product = await client.product.findUnique({
    where: {
      id: +id.toString(),
    },
    include: {
      user: {
        select: {
          // user 가 가지는 데이터중 id,name,avatar 만 선택해서 가져오자
          id: true,
          name: true,
          avatar: true,
        },
      },
    },
  });
  console.log(product);
  res.json({ ok: true, product });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

> console.log(product)
> ![](readMeImages/2023-01-29-22-30-36.png)

```ts
// pages/products/[id].tsx
const ItemDetail: NextPage = () => {
  const router = useRouter();
  const { data, error } = useSWR(
    router.query.id ? `/api/products/${router.query.id}` : null
  ); // url 에 있는 id 에 해당하는 데이터를 요청 (삼항연산자로 라우터가 mount 중인 상황에 대한 방어코드도 작성하자)
    return (
    <Layout canGoBack>
      <div className="px-4  py-4">
	@@ -11,25 +18,24 @@ const ItemDetail: NextPage = () => {
          <div className="flex cursor-pointer py-3 border-t border-b items-center space-x-3">
            <div className="w-12 h-12 rounded-full bg-slate-300" />
            <div>
              <p className="text-sm font-medium text-gray-700">
                {data?.product?.user?.name}
              </p>
              <Link href={`/users/profiles/${data?.product?.user?.id}`}>
                <a className="text-xs font-medium text-gray-500">
                  View profile &rarr;
                </a>
              </Link>
            </div>
          </div>
          <div className="mt-5">
            <h1 className="text-3xl font-bold text-gray-900">
              {data?.product?.name}
            </h1>
            <span className="text-2xl block mt-3 text-gray-900">
              ${data?.product?.price}
            </span>
            <p className=" my-6 text-gray-700">{data?.product?.description}</p>
```

> 상세페이지 모습
> ![](readMeImages/2023-01-29-22-27-53.png)

#### Prisma Search API

> 상품을 클릭하면 나오는 디테일 페이지에서 해당 상품과 유사한 아이템들도 가져오고 싶다. '비슷한' 이름을 가진 상품을 골라내기 위한 검색/필터링에는 Prisma Search API 를 사용한다.
> [[문서]](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference#search)

```ts
// api/products/[id].ts
async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const { id } = req.query;
  const product = await client.product.findUnique({
    where: {
      id: +id.toString(),
    },
    include: {
      user: {
        select: {
          id: true,
          name: true,
          avatar: true,
        },
      },
    },
  });

  //
  const terms = product?.name.split(" ").map((word) => ({
    name: {
      contains: word,
    },
  }));
  const relatedProducts = await client.product.findMany({
    where: {
      OR: terms,
      AND: {
        id: {
          not: product?.id,
        },
      },
    },
  });
  res.json({ ok: true, product, relatedProducts });
}
```

```tsx
// pages/products/[id].tsx
import { Product, User } from "@prisma/client";

interface ProductWithUser extends Product { // Product 타입에 user 를 추가해준다
  user: User;
}
interface ItemDetailResponse {
  ok: boolean;
  product: ProductWithUser;
  relatedProducts: Product[];
}
const ItemDetail: NextPage = () => {
  const router = useRouter();
  const { data } = useSWR<ItemDetailResponse>(
    router.query.id ? `/api/products/${router.query.id}` : null
  );
  return (
    // ...
    <div className=" mt-6 grid grid-cols-2 gap-4">
            {data?.relatedProducts.map((product) => (
              <div key={product.id}>
                <div className="h-56 w-full mb-4 bg-slate-300" />
                <h3 className="text-gray-700 -mb-1">{product.name}</h3>
                <span className="text-sm font-medium text-gray-900">
                  ${product.price}
                </span>
              </div>
            ))}
          </div>
```

> 'Galaxy' term 의 일치로 연관된 Product
> ![](readMeImages/2023-01-29-23-13-33.png)

### Favorite Products

> 좋아요(하트)를 눌러서 Favorite 목록에 추가하거나 제거 하자

1. Fav 모델 생성

```prisma
// schema.prisma

model User {
  id        Int       @id @default(autoincrement())
  phone     String?   @unique
  email     String?   @unique
  name      String
  avatar    String?
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  tokens    Token[]
  products  Product[]
  fav       Fav[] // 추가
}
model Product {
  id          Int      @id @default(autoincrement())
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  user        User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId      Int
  image       String
  name        String
  price       Int
  description String   @db.MediumText
  favs        Fav[] // 추가
}

// Fav 모델 생성
model Fav {
  id        Int      @id @default(autoincrement())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  product   Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  productId Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

> 모델을 생성하면 `npx prisma db push`하고 NextJs 를 재실행한다.

2. 백엔드에 fav에 대한 handler 를 만들기

```ts
// api/products/[id]/fav.ts

import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    query: { id },
    session: { user },
  } = req;
  // findUnique 대신 findFirst 사용(unique 속성이 없으므로)
  const alreadyExists = await client.fav.findFirst({
    where: {
      productId: +id.toString(),
      userId: user?.id,
    },
  });
  if (alreadyExists) {
    // 있으면 fav를 삭제하자.
    // delete 는 unique필드가 있어야만 삭제 할 수 있다
    // unique 필드가 없으면 client.fav.deleteMany() 사용하자
    await client.fav.delete({
      where: {
        id: alreadyExists.id,
      },
    });
  } else {
    // 없으면 fav를 생성한다.
    await client.fav.create({
      data: {
        user: {
          connect: {
            // user.id 를 가진 user에 연결한다.
            id: user?.id,
          },
        },
        product: {
          connect: {
            // user.id 를 가진 product에 연결한다.
            id: +id.toString(),
          },
        },
      },
    });
  }
  res.json({ ok: true });
}

export default withApiSession(
  withHandler({
    methods: ["POST"],
    handler,
  })
);
```

3. 클릭시 하트를 빨갛게 만들자

#### Optimistic UI Update

> 백엔드에 요청을 보낼 때 백엔드의 응답을 기다리지 않고 변경사항을 반영한다.

- `isLiked` 데이터를 추가해서 눌렀는지 안눌렀는지를 체킹한다. (db에 fav 가 존재하는 거랑 별개로)

```tsx
// pages/products/[id].tsx
const ItemDetail: NextPage = () => {
  const router = useRouter();
  // SWR 에서 mutate 도 꺼내준다. 이 mutate 는 data 만 변경시키므로 bounded mutate 이다
  const { data, mutate } = useSWR<ItemDetailResponse>(
    router.query.id ? `/api/products/${router.query.id}` : null
  );
  // 클릭시 데이터를 보낸다. toggleFav 변수는 요청을 트리거 하는 함수
  const [toggleFav] = useMutation(`/api/products/${router.query.id}/fav`);
  const onFavClick = () => {
    if (!data) return;
    mutate({ ...data, isLiked: !data.isLiked }, false);
    toggleFav({});
  };
  return (
    // ...
              <button
                onClick={onFavClick}
                // 클래스명으로 isLinked 변수를 가지고 색을 바꾸자
                className={cls(
                  "p-3 rounded-md flex items-center hover:bg-gray-100 justify-center ",
                  data?.isLiked
                    ? "text-red-500  hover:text-red-600"
                    : "text-gray-400  hover:text-gray-500"
                )}
              >
                {data?.isLiked ? (
                  <svg
                    className="w-6 h-6"
                    fill="currentColor"
                    viewBox="0 0 20 20"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      fillRule="evenodd"
                      d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z"
                      clipRule="evenodd"
                    ></path>
                  </svg>
                ) : (
                  <svg
                    className="h-6 w-6 "
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    aria-hidden="true"
                  >
                    <path
                      strokeLinecap="round"
                      strokeLinejoin="round"
                      strokeWidth="2"
                      d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"
                    />
                  </svg>
                )}
```

> fav버튼 클릭시
> ![](readMeImages/2023-01-30-01-39-52.png)

- SWR 에서 mutate 를 꺼내서 쓰는이유: 전체 UI가 SWR 캐시에 있는 데이터를 표시하기 때문이다. 버튼 클릭 후 API 요청을 기다려서 버튼의 색상을 변경하지 않고 클릭하자마자 색상을 변경하고 싶기때문에 사용한다.
- `mutate({새로운 데이터}, Boolean)`:첫번째 인자는 swr의 캐시 데이터(data)대신 사용할 새로운 데이터, 두번째 boolean 인자는 SWR이 api요청을 보내서 재검증(revalidation) 할지 여부를 의미한다.
- `mutate` 의 새로운 데이터가 기존 캐싱 데이터를 덮어씌우기 때문에 `ui` 가 그 순간 즉각 변경된다.

```tsx
// api/products/[id]/index.ts
async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    query: { id },
    session: { user },
  } = req;
  // const product...
  // const terms...
  // const relatedProducts ...
  // const relatedProducts ...
    const isLiked = Boolean(
    await client.fav.findFirst({
      where: {
        productId: product?.id,
        userId: user?.id,
      },
      select: {
        id: true,
      },
    })
  );

  res.json({ ok: true, product, isLiked, relatedProducts });
```

- `bound mutation` : mutate가 여기있는 데이터만 변경
- `unbound mutation` : 직접요청한 데이터 뿐 아니라 다른 화면에 있는 다른 요청들의 데이터도 변경
- 즉 하트를 눌러서 즐찾을 하면 bound mutate 로 해당 제품의 즐찾 여부가 요청된다. 하트를 눌러서 즐찾 목록에 있는 제품에 대한 데이터를 변경하는 것은 unbound mutate 이다.
- unbound mutate 로 어디서든 자유롭게 모든 컴포넌트의 데이터를 변경할 수 있다.

> useUser 훅을 ItemDetail 페이지에서 사용한다고 해보자.

```tsx
// pages/products/[id].tsx
import useSWR, { useSWRConfig } from "swr";
import useMutation from "@libs/client/useMutation";
import useUser from "@libs/client/useUser";

const ItemDetail: NextPage = () => {
  const { user, isLoading } = useUser();
  const router = useRouter();
  const { mutate } = useSWRConfig(); // unbound mutate
  const { data, mutate: boundMutate } = useSWR<ItemDetailResponse>( // bound mutate
    router.query.id ? `/api/products/${router.query.id}` : null
  );
  const [toggleFav] = useMutation(`/api/products/${router.query.id}/fav`);
  const onFavClick = () => {
    if (!data) return;
    boundMutate((prev) => prev && { ...prev, isLiked: !prev.isLiked }, false);
    // mutate("/api/users/me", (prev: any) => ({ ok: !prev.ok }), false); //unbound mutate 는 변경할 데이터를 명시해줘야 한다.
    toggleFav({});
  };
  return (
```

- unbound mutate 는 변경할 데이터를 명시해줘야 한다.
- `mutate("/api/users/me")` : 해당 api 로 refetch한다
- `mutate("/api/users/me", (prev: any) => ({ ok: !prev.ok }), false)` : 해당 데이터로 mutate 한다

## 동네생활

1. 데이터를 실현할 모델(Post model) 을 만들자.

### Models

```prisma
// schema.prisma
//...
model User {
  id         Int         @id @default(autoincrement())
  phone      String?     @unique
  email      String?     @unique
  name       String
  avatar     String?
  createdAt  DateTime    @default(now())
  updatedAt  DateTime    @updatedAt
  tokens     Token[]
  products   Product[]
  fav        Fav[]
  posts      Post[]
  answers    Answer[]
  wonderings Wondering[]
}
// ...
model Post {
  id        Int         @id @default(autoincrement())
  createdAt DateTime    @default(now())
  updatedAt DateTime    @updatedAt
  user      User        @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  question  String      @db.MediumText
  answers   Answer[]
  wondering Wondering[]
}

model Answer {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  post      Post     @relation(fields: [postId], references: [id], onDelete: Cascade)
  postId    Int
  answer    String   @db.MediumText
}

model Wondering {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  post      Post     @relation(fields: [postId], references: [id], onDelete: Cascade)
  postId    Int
}
```

### write 페이지 구현하기

```ts
// pages/api/posts/index.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    body: { question },
    session: { user },
  } = req;
  const post = await client.post.create({
    data: {
      question,
      user: {
        connect: {
          id: user?.id,
        },
      },
    },
  });
  res.json({
    ok: true,
    post,
  });
}

export default withApiSession(
  withHandler({
    methods: ["POST"],
    handler,
  })
);
```

```tsx
// pages/community/write.tsx
import Button from "@components/button";
import Layout from "@components/layout";
import TextArea from "@components/textarea";
import { useForm } from "react-hook-form";
import useMutation from "@libs/client/useMutation";
import { useEffect } from "react";
import { Post } from "@prisma/client";
import { useRouter } from "next/router";

interface WriteForm {
  question: string;
}

interface WriteResponse {
  ok: boolean;
  post: Post;
}

const Write: NextPage = () => {
  const router = useRouter();
  const { register, handleSubmit } = useForm<WriteForm>();
  const [post, { loading, data }] = useMutation<WriteResponse>("/api/posts");
  const onValid = (data: WriteForm) => {
    if (loading) return;
    post(data);
  };
  useEffect(() => {
    if (data && data.ok) {
      router.push(`/community/${data.post.id}`);
    }
  }, [data, router]);
  return (
    <Layout canGoBack title="Write Post">
      <form onSubmit={handleSubmit(onValid)} className="p-4 space-y-4">
        <TextArea
          register={register("question", { required: true, minLength: 5 })}
          required
          placeholder="Ask a question!"
        />
        <Button text={loading ? "Loading..." : "Submit"} />
      </form>
    </Layout>
  );
```

```ts
// pages/api/posts/[id].ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    query: { id },
  } = req;
  const post = await client.post.findUnique({
    where: {
      id: +id.toString(),
    },
    include: {
      user: {
        select: {
          id: true,
          name: true,
          avatar: true,
        },
      },
      answers: {
        select: {
          answer: true,
          id: true,
          user: {
            select: {
              id: true,
              name: true,
              avatar: true,
            },
          },
        },
      },
      _count: {
        select: {
          answers: true,
          wondering: true,
        },
      },
    },
  });
  res.json({
    ok: true,
    post,
  });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

```ts
// pages/community/[id].ts
import type { NextPage } from "next";
import Layout from "@components/layout";
import TextArea from "@components/textarea";
import { useRouter } from "next/router";
import useSWR from "swr";
import { Answer, Post, User } from "@prisma/client";
import Link from "next/link";

interface AnswerWithUser extends Answer {
  user: User;
}

interface PostWithUser extends Post {
  user: User;
  _count: {
    answers: number;
    wondering: number;
  };
  answers: AnswerWithUser[];
}

interface CommunityPostResponse {
  ok: boolean;
  post: PostWithUser;
}
const CommunityPostDetail: NextPage = () => {
  const router = useRouter();
  const { data, error } = useSWR<CommunityPostResponse>(
    router.query.id ? `/api/posts/${router.query.id}` : null
  );
  console.log(data);
  return (
    // ...
```

> write 하는 경우
> ![](readMeImages/2023-01-31-00-13-03.png) > ![](readMeImages/2023-01-31-00-18-27.png)

> console.log(data);
> ![](readMeImages/2023-01-31-01-19-33.png)

### All posts

- "GET" 메서드로 모든 Posts 를 요청할 때 데이터가 너무 많기 때문에 pagination 을 해줘야 한다.

```ts
// pages/api/posts/index.ts
async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    body: { question },
    session: { user },
  } = req;
  if (req.method === "POST") {
    const post = await client.post.create({
      data: {
        question,
        user: {
          connect: {
            id: user?.id,
          },
        },
      },
    });
    res.json({
      ok: true,
      post,
    });
  }
  if (req.method === "GET") {
    const posts = await client.post.findMany({
      include: {
        user: {
          select: {
            id: true,
            name: true,
            avatar: true,
          },
        },
        _count: {
          select: {
            wondering: true,
            answers: true,
          },
        },
      },
    });
    res.json({
      ok: true,
      posts,
    });
  }
}

export default withApiSession(
  withHandler({
    methods: ["GET", "POST"],
    handler,
  })
);
```

```tsx
// pages/community/index.tsx
import useSWR from "swr";
import { Post, User } from "@prisma/client";

interface PostWithUser extends Post {
  user: User;
  _count: {
    wondering: number;
    answers: number;
  };
}

interface PostsResponse {
  ok: boolean;
  posts: PostWithUser[];
}

const Community: NextPage = () => {
  // useSWR 사용. api/posts 에 GET 요청을 보낸다
  const { data } = useSWR<PostsResponse>(`/api/posts`);
  return (
    // data 를 이용한 랜더링
```

### useCoords 훅: 사용자 위치정보를 이용해보기

> `navigator.geolocation` 메서드를 사용하자

```ts
// libs/client/useCoords.ts
import { useEffect, useState } from "react";

interface UseCoordState {
  longitude: number | null;
  latitude: number | null;
}

export default function useCoords() {
  const [coords, setCoords] = useState<UseCoordState>({
    latitude: null,
    longitude: null,
  });
  const onSuccess = ({
    coords: { latitude, longitude },
  }: GeolocationPosition) => {
    setCoords({ latitude, longitude });
  };
  useEffect(() => {
    navigator.geolocation.getCurrentPosition(onSuccess);
  }, []);
  return coords;
}
```

```prisma
model Post {
  id        Int         @id @default(autoincrement())
  createdAt DateTime    @default(now())
  updatedAt DateTime    @updatedAt
  user      User        @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  question  String      @db.MediumText
    latitude  Float? // 옵셔널
  longitude Float? // 옵셔널
  answers   Answer[]
  wondering Wondering[]
}
```

> 특정좌표 범위 내에 있는 post 만 찾아보자

- 경도 및 위도를 소수점 첫째 자리까지 바꿔도 크게 위치가 변동하기 때문에 소수점 둘째 자리(0.01)를 가지고 반경을 조절한다.

```tsx
//pages/comumunity/index.tsx

const Community: NextPage = () => {
  const { latitude, longitude } = useCoords();
  const { data } = useSWR<PostsResponse>(
    latitude && longitude
      ? `/api/posts?latitude=${latitude}&longitude=${longitude}`
      : null
  ); //  latitude 와 longitude 도 같이 보낸다
  // Next 는 페이지를 미리 만들어놓으려고 하기 때문에 코드를 실행하더라도 서버사이드에서는 좌표를 가져오는 useEffect가 실행되지 않기 때문에 null 을 초기 값으로 가지므로 null 값으로 생기는 에러를 방어해줘야 한다.
```

```tsx
// pages/api/posts/index.ts
async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  if (req.method === "POST") {
    const {
      body: { question, latitude, longitude },
      session: { user },
    } = req;
// ...
if (req.method === "GET") {
    const {
      query: { latitude, longitude },
    } = req;
    const parsedLatitude = parseFloat(latitude.toString());
    const parsedLongitue = parseFloat(longitude.toString());
// ...
```

## Profile

### Models

```prisma
// schema.prisma
// 추가 및 수정된 내용
model User {
  id              Int         @id @default(autoincrement())
  phone           String?     @unique
  email           String?     @unique
  name            String
  avatar          String?
  createdAt       DateTime    @default(now())
  updatedAt       DateTime    @updatedAt
  tokens          Token[]
  products        Product[]
  posts           Post[]
  answers         Answer[]
  wonderings      Wondering[]
  // Review[] 모델이 두번 나오기 때문에 name 으로 구별하기
  writtenReviews  Review[]    @relation(name: "writtenReviews")
  receivedReviews Review[]    @relation(name: "receivedReviews")
  fav             Fav[]
  sales           Sale[]
  purchases       Purchase[]
  record          Record[]
}
model Product {
  id          Int        @id @default(autoincrement())
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  user        User       @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId      Int
  image       String
  name        String
  price       Int
  description String     @db.MediumText
  favs        Fav[]
  sales       Sale[]
  purchases   Purchase[]
  records     Record[]
}

model Review {
  id           Int      @id @default(autoincrement())
  createdAt    DateTime @default(now())
  updatedAt    DateTime @updatedAt
  review       String   @db.MediumText // 길이제한은 medium
  // User 가 둘이므로 name으로 구별짓기
  createdBy    User     @relation(name: "writtenReviews", fields: [createdById], references: [id], onDelete: Cascade)
  createdById  Int
  // User 가 둘이므로 name으로 구별짓기
  createdFor   User     @relation(name: "receivedReviews", fields: [createdForId], references: [id], onDelete: Cascade) // createdFor는 그 id를 갖는 user 참조
  createdForId Int
}

model Sale {
  id        Int      @id @default(autoincrement())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  product   Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  productId Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Purchase {
  id        Int      @id @default(autoincrement())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  product   Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  productId Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Fav {
  id        Int      @id @default(autoincrement())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  product   Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  productId Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Record {
  id        Int      @id @default(autoincrement())
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  product   Product  @relation(fields: [productId], references: [id], onDelete: Cascade)
  productId Int
  createdAt DateTime @default(now())
  kind      Kind
  updatedAt DateTime @updatedAt
}

enum Kind {
  Purchase
  Sale
  Fav
}
```

- Review 모델은 createdFor와 createdBy 에서 User 모델을 두번이나 각각 가리키는데, Token 을 User에게 가리킬 때마다 relationship 을 반대로도 등록을 해줬어야 했기 떄문에 문제가 발생한다.
- user 가 token 을 생성했다는 것은, User 모델에서 유저가 어떤 Token 을 생성했는지도 알 수 있어야 한다는 것이다. (즉, Token이 User 를 가리키면 User도 무엇이 자길 가리키는지 알아야 한다)
- 따라서 User에서 구별할 수 있도록 연결에 서로 다른 이름을 줘야 한다 : @relation(name : ")

### Handler

```ts
// api/users/me/favs.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    session: { user },
  } = req;
  const favs = await client.fav.findMany({
    where: {
      userId: user?.id,
    },
    include: {
      product: true,
    },
  });
  res.json({
    ok: true,
    favs,
  });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

```ts
// api/users/me/purchases.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    session: { user },
  } = req;
  const purchases = await client.purchase.findMany({
    where: {
      userId: user?.id,
    },
    include: {
      product: true,
    },
  });
  res.json({
    ok: true,
    purchases,
  });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

```ts
// api/users/me/sales.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    session: { user },
  } = req;

  const sales = await client.sale.findMany({
    where: {
      userId: user?.id,
    },
    include: {
      product: true,
    },
  });
  res.json({
    ok: true,
    sales,
  });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

```ts
// api/reviews.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    session: { user },
  } = req;
  const reviews = await client.review.findMany({
    where: {
      createdForId: user?.id,
    },
    include: { createdBy: { select: { id: true, name: true, avatar: true } } },
  });
  res.json({
    ok: true,
    reviews,
  });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

### FrontEnd

> Profile, Sales, Purchase, Favorites 페이지

```tsx
// components/product-list.tsx
import { ProductWithCount } from "pages";
import useSWR from "swr";
import Item from "./item";

interface ProductListProps {
  kind: "favs" | "sales" | "purchases";
}

interface Record {
  id: number;
  product: ProductWithCount;
}

interface ProductListResponse {
  [key: string]: Record[];
}

export default function ProductList({ kind }: ProductListProps) {
  const { data } = useSWR<ProductListResponse>(`/api/users/me/${kind}`);
  return data ? (
    <>
      {data[kind]?.map((record) => (
        <Item
          id={record.product.id}
          key={record.id}
          title={record.product.name}
          price={record.product.price}
          hearts={record.product._count.favs}
        />
      ))}
    </>
  ) : null;
}
```

```tsx
// pages/profile/index.tsx
import type { NextPage } from "next";
import Link from "next/link";
import Layout from "@components/layout";
import useUser from "@libs/client/useUser";
import useSWR from "swr";
import { Review, User } from "@prisma/client";
import { cls } from "@libs/client/utils";

interface ReviewWithUser extends Review {
  createdBy: User;
}
interface ReviewsResponse {
  ok: boolean;
  reviews: ReviewWithUser[];
}

const Profile: NextPage = () => {
  const { user } = useUser();
  const { data } = useSWR<ReviewsResponse>("/api/reviews");
  return (
    <Layout hasTabBar title="나의 캐럿">
      <div className="px-4">
        <div className="flex items-center mt-4 space-x-3">
          <div className="w-16 h-16 bg-slate-500 rounded-full" />
          <div className="flex flex-col">
            <span className="font-medium text-gray-900">{user?.name}</span>
            <Link href="/profile/edit">
              <a className="text-sm text-gray-700">Edit profile &rarr;</a>
            </Link>
	@@ -86,71 +100,40 @@ const Profile: NextPage = () => {
            </a>
          </Link>
        </div>
        {data?.reviews.map((review) => (
          <div key={review.id} className="mt-12">
            <div className="flex space-x-4 items-center">
              <div className="w-12 h-12 rounded-full bg-slate-500" />
              <div>
                <h4 className="text-sm font-bold text-gray-800">
                  {review.createdBy.name}
                </h4>
                <div className="flex items-center">
                  {[1, 2, 3, 4, 5].map((star) => (
                    <svg
                      key={star}
                      className={cls(
                        "h-5 w-5",
                        review.score >= star
                          ? "text-yellow-400"
                          : "text-gray-400"
                      )}
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 20 20"
                      fill="currentColor"
                      aria-hidden="true"
                    >
                      <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                    </svg>
                  ))}
                </div>
              </div>
            </div>
            <div className="mt-4 text-gray-600 text-sm">
              <p>{review.review}</p>
            </div>
          </div>
        ))}
      </div>
    </Layout>
  );
};
```

> useUser 훅으로 user 데이터를 가져온다. 로그인이 아니면 Home 으로 보내면서 페이지를 보호해주는데 로그인 페이지를 제외한 모든 페이지에 보호가 있어야 한다.
> useUser 를 \_app.tsx 파일에 넣어서 모든 페이지에 적용이 되면서 useUser 를 수정하여 'public' 인자를 받도록 하여 이 값에 따라 보호여부를 결정하도록 코드를 수정해보자 (챌린지)

## STREAMS

> 사람들이 제품을 라이브 쇼케이스할때 작성할 form 을 만들자.

### model

```prisma
model Stream {
  id          Int       @id @default(autoincrement())
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  name        String
  description String    @db.MediumText
  price       Int
  user        User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId      Int
  messages    Message[]
}

model Message {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
  userId    Int
  message   String   @db.MediumText
  stream    Stream   @relation(fields: [streamId], references: [id], onDelete: Cascade)
  streamId  Int
}
```

### form : live stream 을 생성하기 위한 폼

```tsx
// pages/live/create.tsx

import type { NextPage } from "next";
import Button from "@components/button";
import Input from "@components/input";
import Layout from "@components/layout";
import TextArea from "@components/textarea";
import { useForm } from "react-hook-form";
import useMutation from "@libs/client/useMutation";
import { useEffect } from "react";
import { useRouter } from "next/router";
import { Stream } from "@prisma/client";

interface CreateForm {
  name: string;
  price: string;
  description: string;
}

interface CreateResponse {
  ok: boolean;
  stream: Stream;
}

const Create: NextPage = () => {
  const router = useRouter();
  const [createStream, { loading, data }] =
    useMutation<CreateResponse>(`/api/streams`);
  const { register, handleSubmit } = useForm<CreateForm>();
  const onValid = (form: CreateForm) => {
    if (loading) return;
    createStream(form);
  };
  useEffect(() => {
    if (data && data.ok) {
      router.push(`/streams/${data.stream.id}`);
    }
  }, [data, router]);
  return (
    <Layout canGoBack title='Go Live'>
      <form onSubmit={handleSubmit(onValid)} className=' space-y-4 py-10 px-4'>
        <Input
          register={register("name", { required: true })}
          required
          label='Name'
          name='name'
          type='text'
        />
        <Input
          register={register("price", { required: true, valueAsNumber: true })}
          required
          label='Price'
          name='price'
          type='text'
          kind='price'
        />
        <TextArea
          register={register("description", { required: true })}
          name='description'
          label='Description'
        />
        <Button text={loading ? "Loading..." : "Go live"} />
      </form>
    </Layout>
  );
};

export default Create;
```

- `valueAsNumber: true` : 스트링 input을 숫자로 바꿔서 받아준다.(form 은 디폴트로 모두 문자열로 받음)

### Handler: stream api

```tsx
// api/streams/index.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    session: { user },
    body: { name, price, description },
  } = req;
  if (req.method === "POST") {
    const stream = await client.stream.create({
      data: {
        name,
        price,
        description,
        user: {
          connect: {
            id: user?.id,
          },
        },
      },
    });
    res.json({ ok: true, stream });
  } else if (req.method === "GET") {
    const streams = await client.stream.findMany();
    res.json({ ok: true, streams });
  }
}

export default withApiSession(
  withHandler({
    methods: ["GET", "POST"],
    handler,
  })
);
```

```ts
// api/streams/[id].ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    query: { id },
  } = req;
  const stream = await client.stream.findUnique({
    where: {
      id: +id.toString(),
    },
  });
  res.json({ ok: true, stream });
}

export default withApiSession(
  withHandler({
    methods: ["GET"],
    handler,
  })
);
```

- 응답에 `ok : true` 를 넣는 이유는 사용자가 의도하지 않는 input 을 넣을 상황을 방어해주기 위해서이다. (링크를 통해서 이동하지 않고 직접 url 에 id값을 입력해서 응답할 수 없는 경우, `ok` 값으로 404 페이지를 보여줄수 있다.)

> ![](readMeImages/2023-02-03-15-37-26.png) > ![](readMeImages/2023-02-03-15-38-11.png) > ![](readMeImages/2023-02-03-15-40-56.png)
>
> > prisma studio
> > ![](readMeImages/2023-02-03-15-45-50.png)

### Send message: serverless에서 live 채팅 구현하기

```ts
// pages/api/streams/[id]/messages.ts
import { NextApiRequest, NextApiResponse } from "next";
import withHandler, { ResponseType } from "@libs/server/withHandler";
import client from "@libs/server/client";
import { withApiSession } from "@libs/server/withSession";

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    query: { id },
    body,
    session: { user },
  } = req;
  const message = await client.message.create({
    data: {
      message: body.message,
      stream: {
        connect: {
          id: +id.toString(),
        },
      },
      user: {
        connect: {
          id: user?.id,
        },
      },
    },
  });
  res.json({ ok: true, message });
}

export default withApiSession(
  withHandler({
    methods: ["POST"],
    handler,
  })
);
```

```ts
//pages/streams/[id].tsx
import useSWR from "swr";
import { useRouter } from "next/router";
import { Stream } from "@prisma/client";
import { useForm } from "react-hook-form";
import useMutation from "@libs/client/useMutation";

interface StreamResponse {
  ok: true;
  stream: Stream;
}

interface MessageForm { // 채팅 메세지에 대한 form
  message: string;
}

const Stream: NextPage = () => {
  const { user } = useUser();
  const router = useRouter();
  const { register, handleSubmit, reset } = useForm<MessageForm>();
  const { data, mutate } = useSWR<StreamResponse>(
    router.query.id ? `/api/streams/${router.query.id}` : null
  );
  const [sendMessage, { loading, data: sendMessageData }] = useMutation(
    `/api/streams/${router.query.id}/messages`
  );
  const onValid = (form: MessageForm) => {
    if (loading) return;
    reset();
    sendMessage(form);
  };
  useEffect(() => {
    if (sendMessageData && sendMessageData.ok) {
      mutate();
    }
  }, [sendMessageData, mutate]);
  return (
    // ...
            <form
              onSubmit={handleSubmit(onValid)}
              className="flex relative max-w-md items-center  w-full mx-auto"
            >
              <input
                type="text"
                {...register("message", { required: true })}
                className="shadow-sm rounded-full w-full border-gray-300 focus:ring-orange-500 focus:outline-none pr-12 focus:border-orange-500"
              />
              <div className="absolute inset-y-0 flex py-1.5 pr-1.5 right-0">
                <button className="flex focus:ring-2 focus:ring-offset-2 focus:ring-orange-500 items-center bg-orange-500 rounded-full px-3 hover:bg-orange-600 text-sm text-white">
                  &rarr;
                </button>
              </div>
            </form>
```

- stream 에 있는 메세지를 GET 하기: 새 useSWR 을 만들어서 `/api/streams/[id]/messages`로 GET요청을 보낸다 (이미 POST요청을 할 수 있으니깐 RESTful하다)

```ts
// pages/api/streams/[id]/index.ts

async function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseType>
) {
  const {
    query: { id },
  } = req;
  const stream = await client.stream.findUnique({
    where: {
      id: +id.toString(),
    },
    include: {
      messages: {
        select: {
          id: true,
          message: true,
          user: {
            select: {
              avatar: true,
              id: true,
            },
          },
        },
      },
    },
  });
  res.json({ ok: true, stream });
}
```

> 채팅창의 스크롤을 맨 밑으로 유지하는 법
> 아무런 처리를 해주지 않으면 처음 채팅 화면에 진입할 때 스크롤이 맨 위에 위치하고
> 새 메세지를 보낼 때 스크롤이 밑으로 늘어나지만 화면은 그대로인 상태가 됩니다
> 평소 사용하는 채팅 앱 등을 생각해보면 스크롤을 항상 맨 아래로 당겨줘야 합니다
> useRef와 scrollIntoView 를 사용해서 해결할 수 있습니다.

```js
// useRef로 스크롤할 DOM을 선택하고 useEffect와 scrollIntoView로 스크롤합니다
const scrollRef = useRef<HTMLDivElement>(null);
useEffect(() => {
scrollRef?.current?.scrollIntoView();
});
...
// 메세지들 목록 맨 밑에 빈 div를 만들어 ref를 설정합니다.
{data?.stream.messages.map...}
<div ref={scrollRef}/>
```

#### 최대한 실시간 경험 제공하기

> SWR 의 캐시를 mutate 하고 가짜 메세지를 직접 추가하자

- `pages/streams/[id].tsx` mutation 이 실행되고 끝나서 랜더링 할때까지 기다리지 말고 메세지를 백엔드로 보내기(`sendMessage(form)`) 전에 mutate 를 하자

```ts
// pages/streams/[id].ts
const Stream: NextPage = () => {
  const { user } = useUser();
  const router = useRouter();
  const { register, handleSubmit, reset } = useForm<MessageForm>();
  const { data, mutate } = useSWR<StreamResponse>(
    router.query.id ? `/api/streams/${router.query.id}` : null,
    {
      refreshInterval: 1000, // 서버를
    }
  );
  const [sendMessage, { loading, data: sendMessageData }] = useMutation(
    `/api/streams/${router.query.id}/messages`
  );
  const onValid = (form: MessageForm) => {
    if (loading) return;
    reset();
    mutate( //
      (prev) => // (prev)는 캐시의 모든 previous data
        prev &&
        ({
          ...prev,
          stream: {
            ...prev.stream,
            messages: [
              ...prev.stream.messages,
              {
                id: Date.now(),
                message: form.message,
                user: {
                  ...user,
                },
              },
            ],
          },
        } as any),
      false // mutate 의 두번째 인수는 revalidate(백엔드에서 더블체크해줄지)  여부
    );
    sendMessage(form);
  };
  return (
```

#### NextJs의 serverless 환경에선 realtime 을 만들 순 없다

> NextJS 와 api 라우터만 쓸 경우 실시간 채팅 기능을 구현할 수 없다. <br>
> realtime 기능은 서버가 필요하고 클라이언트와 연결을 유지하고 있어야 한다.

#### Prisma Seeding

> seeding 은 테스트를 목적으로 데이터베이스에 페이크 데이터를 엄청 빠르게 생성할 수 있다. <br>
> 이 기능을 이용해서 라이브스트림을 많이 생성해보자

```ts
// prisma/seed.ts

import { PrismaClient } from "@prisma/client";

const client = new PrismaClient();

async function main() {
  [...Array.from(Array(500).keys())].forEach(async (item) => {
    await client.stream.create({
      data: {
        name: String(item),
        description: String(item),
        price: item,
        user: {
          connect: {
            id: 1,
          },
        },
      },
    });
    console.log(`${item}/500`);
  });
}

main()
  .catch((e) => console.log(e))
  .finally(() => client.$disconnect());
```

- [...Array.from(Array(500).keys())] 는 [0,1,...499] 배열

> package.json 에 prisma 라는 새 key를 생성하자

```json
{
  "scripts": {
    // ...
  },
  "dependencies" : {
    // ...
  }
  "prisma": {
    "seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts"
  }
}
```

> 이 명령은 `npx prisma db seed` 명령어로 실행된다.<br>
> 실행하려면 `ts-node` 가 설치되어야 한다 : `npm install ts-node`

> 다음과 같은 에러가 나는 경우
> ![](readMeImages/2023-02-03-23-58-36.png)

- 원인: seed.ts 파일에서 id 1인 user를 찾는데 db에는 해당 user가 없기 떄문
- 해결: prisma studio 에 존재하는 유저 id로 변경하고 seed를 실행하면 된다.

> ![](readMeImages/2023-02-03-23-59-34.png)<br> > ![](readMeImages/2023-02-03-23-59-47.png)

> 다음과 같은 에러가 나는 이유
> ![](readMeImages/2023-02-04-00-11-52.png)

- 원인: `prisma` 의 connection pool (db에 동시에 연결을 얼마나 할 지)에 부딪혔기 때문
- `planetscale` 은 무료버전에서 1000개까지 동시 접속이 가능하므로 `prisma` 에도 해당 설정을 해줘야 한다.

```prisma
// connection 제한을 세팅하고 싶으면 다음 datasource configuration 을 prisma schema 에 설정해야 한다.
datasource db {
  provider = "postgresql"
  url      = "postgresql://johndoe:mypassword@localhost:5432/mydb?connection_limit=5"
}
```

```prisma
// connection pool timeout 을 설정하고 싶을 경우 (디폴트 10)
datasource db {
  provider = "postgresql"
  url      = "postgresql://johndoe:mypassword@localhost:5432/mydb?connection_limit=5&pool_timeout=2"
}
```

> seed 생성으로 많은 stream 이 생김
> ![](readMeImages/2023-02-04-00-19-59.png)

#### pagination

- `prisma` 에는 `pagination` 엔진이 있기 때문에 이를 이용하면 쉽게 구현할 수 있다.

```tsx
// api/streams/index.ts
} else if (req.method === "GET") {
    const streams = await client.stream.findMany({
      take: 10, // 10개만 가져온다
      skip: 20, // 앞에 20개는 무시한다
    });
    res.json({ ok: true, streams });
  }
```

- `GET` 요청을 보낼때 `findMany` 에 인자로 얼마나 가져올 것인지 옵션을 넣어주면 된다.
- 가져오는 데이터의 갯수를 한정시키면 네트워크 비용을 절약할 수 있다.

```ts
// api/posts/[id]/index.ts
const post = await client.post.findUnique({

answers: {
        select: {
          answer: true,
          id: true,
          user: {
            select: {
              id: true,
              name: true,
              avatar: true,
            },
          },
        },
        take: 10,
        skip: 20,
      },
```
