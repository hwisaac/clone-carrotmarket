# Carrot market clone

## stack

Next

##

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

- tawilwind 는 utility-first css 이다. (즉 테일윈드가 많은 클레스네임을 갖는 다는 말. 엄청 큰 css 파일이다.)
- 스타일을 할때 클래스 네임을 조합해서 하면 된다.
- vscode 의 extension : Tailwind CSS intelliSense 를 설치하면 자동완성 기능을 쓸 수 있다.
- 자동완성이 안뜨면 `컨트롤+스페이스` cmd + space 를 눌러본다

### 자주쓰는 것

- flex
- p 패딩
- m 마진
- bg 백그라운드
- space 자식요소에 마진을 준다 ex) space-y-5 (자식요소에 mt-5 와 mb-5 줄수 있다.)
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
-
