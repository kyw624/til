# 타입스크립트 환경 세팅

## package.json 생성

프로젝트 디렉터리 생성 후 `$ yarn init -y`

> build 스크립트 추가

```
{
  "name": "ts-practice",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "typescript": "^3.8.3"
  },
  "scripts": {
    "build": "tsc"
  }
}
```

<br />

## 프로젝트에 typescript 설치

> 글로벌

    $ yarn global add typescript
    // 또는 npm install -g typescript

> 로컬 (일반적으로 로컬 패키지 사용)

    $ yarn add typescript
    // 또는 npm install --save typescript

<br />

## tsconfig.json 생성 (TS 설정 파일)

`$ tsc --init`

      tsconfig.json 파일 기본값

      {
        "compilerOptions": {
          "target: "es5",
            // 컴파일 된 코드의 실행 환경 정의,
               es5: 화살표 함수 사용 시 function 함수식으로 변환

          "module": "commonjs",
            // 컴파일 된 코드가 사용 할 모듈 시스템 정의

          "strict": true,
            // 모든 타입 체킹 옵션 활성화

          "esModuleInterop: true,
            // 모듈 형태로 이루어진 파일을 es2015 모듈 형태로 불러와줌

          "outDir": "./built"
            // 기본 설정에서 추가한 속성
               컴파일 된 파일들이 저장되는 경로를 지정
        }
      }

<br />

## 타입스크립트 파일 생성

`*.ts` 확장자로 test.ts 파일 생성

```ts
const text: string = "텍스트입니다";
console.log(text); // '텍스트입니다'
```

text 상수의 선언문 중 `: string`은 해당 변수 값의 타입이 문자열임을 명시해준다.  
해당 값에 문자열이 아닌 타입의 값이 오면 에러가 발생하지만 `.js` 파일은 생성된다.

<br />

## test.ts 파일 컴파일

기본 명령어는

```
$ tsc
// 또는 npx tsc
```

이지만 위에서 `package.json` 파일에 build 스크립트를 추가해놨기에  
`$ yarn build` 라고 입력하면 된다.

컴파일 수행 시 `tsconfig.json` 파일에서 `outDir`로 지정한 경로에 컴파일되어  
`test.js` 파일이 생성된다.
