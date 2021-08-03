React에서 엘리먼트 사이에 text를 집어넣을 때, 개행 처리가 필요한 경우가 있다.

나는 원래 아래처럼 br 태그를 끼워넣는 방식으로 했었다.

```jsx
function Description() {
  return (
    <S.Container>
      <div className="description">
        {description.split("\n").map((d) => (
          <>
            {d}
            <br />
          </>
        ))}
      </div>
    </S.Container>
  );
}

const S = {
  Container: styled.div`
    width: 100%;

    > .description {
      width: 100%;
    }
  `,
};
```

하지만 개행문자는 CSS의 white-space 속성을 pre-line으로 설정해서 처리해줄 수도 있다.

개인적으로 이 방법이 더 깔끔한 것 같아서 앞으로 참고해야겠다.

```jsx
function Description() {
  return (
    <S.Container>
      <div className="description">
        {description}
      </div>
    </S.Container>
  );
}

const S = {
  Container: styled.div`
    width: 100%;

    > .description {
      width: 100%;

      white-space: pre-line;
    }
  `,
};
```
