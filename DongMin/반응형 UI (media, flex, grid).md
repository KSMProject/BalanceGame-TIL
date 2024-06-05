# 미디어쿼리

## 미디어쿼리의 문제점

미디어쿼리는 반응형에 대한 문제를 해결할 수있는 좋은 해결책처럼 보이지만 한계가 명확하다.

웹이 점점 크고 복잡해진 레이아웃으로 성장했기 때문이다.

### 문제점1. 뷰포트만을 고려

- 뷰포트만을 고려하기때문에 컴포넌트 단위의 작은 내용물들이 복잡해지고 많아지는 웹의 경우에는 미디어쿼리가 민감하게 반응하기 어렵다.
- 브라우저의 크기를 조절하고 콘텐츠 레이아웃이 깨지는 정확한 지점을 알 수 없다.

### 문제점2. 관리가 어렵다

- 미디어쿼리는 컴포넌트를 둘러싼 내용물을 알지 못하며 개발자는 이 모든 최적의 지점을 찾아야한다.
- 컴포넌트들의 크기나 내용이 바뀌면 이에 따라 여러 미디어 쿼리가 필요해진다

### 문제점3. 생각보다 반응적이지 않다

- 깨질때마다 새로운 미디어쿼리를 조정하는 것은 반응적이지 않다.
- 이는 반응형이기보단 적응형에 가깝다

<aside>
💡 더 나은 선택지인 flexbox와 grid, 반응형단위와 수학함수 등이 있다. 
컨테이너쿼리도 등장했지만 아직은 초기단계이다.

</aside>

# Flexbox

- 미디어쿼리와 주로 같이 사용되며 Flexbox는 특정 방향으로 레이아웃을 설정하고 미디어쿼리는 특정한 뷰표트일 때 방향을 변경하는 역할을 한다.

```css
main {
  display: flex;
  flex-flow: row wrap;
}

main article {
  width: 50vw;
  padding: 10px;
  border: 1px solid #000;
}

@media (width < 700px) {
  main article {
    width: 100vw;
  }
```

### 문제점. 너무 뷰포트 중심이다

- 컨테이너의 방향을 변경하는 지점을 뷰포트 너비만을 고려하고 있다.
- 뷰포트너비만을 고려했기 때문에 더 작은 컨테이너 내에서 요소를 사용할 수 없으며 내부나 주변 요소가 있을 경우 어색해질 수 있다
- 좁은 화면의 디바이스는 깨질 수 있다

### 해결책. flexbox를 활용

- 제일 좋은 해결책은 미디어쿼리를 사용하지 않는 것이다.

```css
main {
  display: flex;
  flex-flow: row wrap;
}

main article {
  flex: 1 1 400px;
}
```

- `flex-grow: 1;`: article 요소가 다른 요소들과 함께 남은 공간을 나누어 가질 수 있음
    - 0일 경우 `flex-basis`의 값 고정
- `flex-shrink: 1;`: article 요소가 다른 요소들과 함께 공간이 부족할 때 크기를 줄임
- `flex-basis: 400px;`: article 요소의 기본 크기를 400픽셀로 설정

# Grid

- flexbox도 좋지만 각 행의 크기를 다르게 하고 싶다면 grid가 더 나은 선택일 수 있다.
- flexbox에 width를 지정해야한다면 grid가 나은 선택일 수 있다.

```css
main {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
}
```

- **`auto-fit`**: 가능한 많은 열을 맞추고 남은 공간이 있으면 열을 확장합니다.
- **`minmax`**: 열의 최소 너비를 지정하는데 이 코드에서는 `500px`이 최솟값입니다.

### 참조

https://velog.io/@typo/beyond-css-media-quries?utm_source=substack&utm_medium=email
