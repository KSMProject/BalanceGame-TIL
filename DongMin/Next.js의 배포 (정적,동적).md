Next로 프로젝트를 배포를 할때 2가지의 방법이 있다.

하나는 output의 설정을 export로 하는 것과 다른 하나는 standalone이다.

이 두가지의 방법에는 어떤 차이가 있는지 araboja

우선 프로젝트를 생성할 때 `create-next-app@latest`로 생성했는데 이때 .next.config.mjs라는 파일이 생성되었다. `.next.config.js`와는 어떤 차이가 있는 것일까?

두 파일은 자바스크립트의 모듈 시스템의 차이이다.

# CommonJS vs. ES Modules

`next.config.js`: CommonJS 모듈 시스템을 사용한다. CommonJS는 Node.js의 기본 모듈 시스템으로, `require`와 `module.exports`를 사용한다.

```jsx
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  output: "standalone",
  // 기타 설정
}

module.exports = nextConfig;
```

`next.config.mjs`: ES Modules를 사용한다. ES Modules는 JavaScript의 공식 모듈 시스템으로, `import`와 `export`를 사용한다.

```jsx
** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  output: "export",
  // 기타 설정
}

export default nextConfig;
```

## 고려사항

### **호환성**

- CommonJS (`next.config.js`)는 현재 대부분의 Node.js 프로젝트에서 널리 사용
- ES Modules (`next.config.mjs`)는 최신 JavaScript 표준을 따르고 있으며, 더 많은 기능과 최적화를 제공할 수 있지만, 모든 환경에서 지원되는 것은 아님

### **프로젝트 요구사항**

- 새로운 프로젝트를 시작하거나 최신 표준을 사용하고 싶다면 ES Modules를 고려
- 기존 프로젝트를 유지하거나 라이브러리와의 호환성을 신경써야 한다면 CommonJS를 고려

---

그렇다면 standalone와 export의 차이는 무엇일까? [Next 공식문서](https://nextjs.org/docs/pages/api-reference/next-config-js/output)를 요약하면 이러하다.

# next.config {output : "standalone" | "export"}

- config파일을 통해 output을 설정할 수 있는데 이 설정은 빌드하는 동안 **Nex.js는 각 페이지와 해당 종속성을 자동으로 추적하여 배포하는 필요한 파일을 결정**한다.

<aside>
💡 **공식문서의 output 문서**

”이 기능은 배포 크기를 크게 줄이는 데 도움이 됩니다. 이전에는 도커로 배포할 때 다음 시작을 실행하려면 패키지 종속성의 모든 파일을 설치해야 했습니다. Next.js 12부터는 .next/ 디렉토리의 출력 파일 추적을 활용하여 필요한 파일만 포함할 수 있습니다.”

</aside>

### **standalone**

- 하나의 독립 실행파일로 빌드하는 설정
    - node.js 서버 환경에서 어플리케이션을 실행할 때 유용
- 은 Next.js의 일부 파일만을 포함하여 프로덕션 배포에 필요한 파일만 복사하는 폴더를 자동 생성할 수 있다.
- 설치를 하지 않고도 자체적으로 배포할 수 있는 폴더가 생성된다. (`.next/standalone`)
- SSR (서버사이드 렌더링) 및 동적 기능을 지원한다.
    - 기본적으로 public과 static폴더를 복사하지 않기 때문에 수동으로 복사해야한다.

### export

- 서버 없이 정적 파일을 호스팅하는 환경에 유용
- 각 페이지를 정적 HTML파일로 내보낸다.
- 서버 없이 CDN이나 정적 웹 호스팅서비스 (S3, vercel, Netlify)에서 직접 호스팅할 수 있다.
- 서버사이드 기능은 사용할 수 없다.
    - API Route와 같은 동적 라우팅은 불가능하다.
    - getServerSideProps와 같은 SSR이 필요한 기능은 불가능하다.

## 요약

- 게시판이나 게임과 같은 동적인 웹 애플리케이션을 배포할 때는 Next.js의 `standalone` 모드를 사용하여 서버 사이드 렌더링(SSR)과 API 라우트를 활용하는 것이 좋다.
    - Docker를 사용해 애플리케이션을 패키징하고, AWS Elastic Beanstalk를 통해 배포하면 손쉽게 확장 가능한 인프라를 구성할 수 있다.
    - 도메인 설정과 SSL 인증서를 추가하여 보안과 신뢰성을 강화할 수 있다.
- 정적파일 또는 서버가 필요없는 문서사이트, 정적 포트폴리오와 같은 기능의 웹은 export를 사용하는 것이 용이하다.
    - 빠른 로딩속도 (정적 파일로 제공되기 때문에 속도가 매우 빠르다)
    - 효율적 비용 (서버리스 환경에서 호스팅이 가능)

## 참고

[next.config.js Options: output](https://nextjs.org/docs/pages/api-reference/next-config-js/output)

[Deploying: Static Exports](https://nextjs.org/docs/app/building-your-application/deploying/static-exports)
