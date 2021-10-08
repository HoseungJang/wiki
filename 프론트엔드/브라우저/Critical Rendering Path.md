> 이 문서는 [이 글](https://wonism.github.io/critical-rendering-path/)을 참조하여 작성되었습니다.

## 목차
- [Critical Rendering Path?](#critical-rendering-path)
- [DOM 트리 생성](#dom-트리-생성)
- [CSSOM 트리 생성](#cssom-트리-생성)
- [Javascript 실행](#javascript-실행)
- [렌더 트리 생성](#렌더-트리-생성)
- [레이아웃 생성](#레이아웃-생성)
- [페인팅](#페인팅)

## Critical Rendering Path?
Critical Rendering Path란, 브라우저가 서버로부터 응답받은 HTML을 유저가 실제로 보는 화면으로 그려내기까지의 과정을 말한다. 이 과정의 각 단계가 최대한 효율적으로 이루어지도록 하는 것이 중요하다.

Lighthouse CI를 사용하다 보면 점수가 낮은 부분을 개선하는 것에 대한 가이드라인을 제시해주는데, 그걸 참고하면 재미난 최적화 방법들을 둘러볼 수 있다.

Critical Rendering Path는 아래의 6단계로 이루어진다.

1. DOM 트리 생성
2. CSSOM 트리 생성
3. Javascript 실행
4. 렌더 트리 생성
5. 레이아웃 생성
6. 페인팅

## DOM 트리 생성
DOM 트리는 HTML이 Javascript가 사용할 수 있는 Object로 파싱된 프로그래밍 인터페이스이다.

```html
<html>
<head>
  <title>Understanding the Critical Rendering Path</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>Understanding the Critical Rendering Path</h1>
  </header>
  <main>
    <h2>Introduction</h2>
    <p>Lorem ipsum dolor sit amet</p>
  </main>
  <footer>
    <small>Copyright 2017</small>
  </footer>
</body>
</html>
```

위 HTML이 파싱되면 아래와 같은 DOM 트리가 생성된다.

![](/프론트엔드/브라우저/images/dom.png)

## CSSOM 트리 생성
HTML을 파싱하는 과정에서 CSS도 마주치게 되는데, CSS는 HTML처럼 파싱되어 트리 형태의 CSSOM으로 만들어진다.

HTML에서 inline으로 작성된 CSS, CSS 파일에 따로 정의한 CSS, 브라우저의 기본 CSS 설정값 등이 모두 합쳐져서 CSSOM을 이룬다.

```css
body { font-size: 18px; }

header { color: plum; }
h1 { font-size: 28px; }

main { color: firebrick; }
h2 { font-size: 20px; }

footer { display: none; }
```

[위 HTML 예제](#dom-트리-생성)에 이와 같은 CSS를 적용하면, 아래와 같은 CSSOM이 생성된다.

![](/프론트엔드/브라우저/images/cssom.png)

CSS는 파싱되는 동안 렌더링을 차단한다.

## Javascript 실행
Javascript의 실행은 HTML 파싱을 차단한다. 따라서 문서를 참조하는 Javascript 코드가 존재하는 경우, 반드시 script 태그가 해당 문서보다 아래에 위치해야 한다.

Javascript가 HTML 파싱을 차단하는 것을 피하기 위해 [async 또는 defer 속성](/프론트엔드/script%20태그의%20async,%20defer%20속성.md)을 script 태그에 부여해줄 수 있다.

## 렌더 트리 생성
렌더 트리는 DOM과 CSSOM이 합쳐져 최종적으로 렌더링할 내용을 나타내는 트리이다. 즉, 유저에게 보여지는 화면이 렌더 트리의 내용이다.

위의 [DOM](#dom-트리-생성), [CSSOM](#cssom-트리-생성)은 결과적으로 아래와 같은 렌더 트리를 생성한다.

![](/프론트엔드/브라우저/images/render-tree.png)

## 레이아웃 생성
렌더 트리까지 다 만들어지면, 렌더 트리의 노드들을 화면에 어떻게 배치할지 정하는 레이아웃이 생성된다.

현재 뷰포트에 따라서 노드들이 어떻게 배치되어야 하는지를 구한다.

만약 width가 50%인 요소가 있고 윈도우를 리사이징 한다고 가정하면, 렌더 트리의 노드들은 변화가 없으니 레이아웃 생성 단계만 다시 거쳐서 화면을 그리게 된다. 그리고 이것이 reflow다.

## 페인팅
레이아웃 생성을 마치면 렌더 트리의 노드들을 실제로 화면에 그리게 된다.
