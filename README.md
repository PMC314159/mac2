# Pair Archive v25

이번 버전은 화면용 디자인과 저장용 디자인을 따로 만들지 않습니다.

## 저장 방식

`원본 그대로 PNG 저장` 버튼을 누르면 현재 화면의 `#captureArea` HTML, CSS 변수, 이미지와 폰트를 Vercel 함수로 전송합니다. Vercel 함수는 서버의 실제 Chromium 브라우저로 같은 HTML을 렌더링한 뒤 해당 요소를 2배 해상도 PNG로 촬영합니다.

즉 다음 방식을 사용하지 않습니다.

- 화면 스크린샷
- html2canvas
- html-to-image
- 별도 Canvas로 디자인 다시 그리기
- 화면 공유 API

브라우저 Chromium이 HTML/CSS를 직접 렌더링하므로 화면과 저장 결과의 레이아웃, 반투명 패널, 둥근 모서리, 글자 줄바꿈이 가장 가깝게 유지됩니다.

## GitHub에 올려야 하는 파일

루트 폴더에 다음 파일과 폴더를 모두 올려 주세요.

```text
index.html
package.json
vercel.json
api/
  render.js
```

`index.html`만 교체하면 API가 없어서 저장 버튼이 작동하지 않습니다.

## Vercel 배포

1. 위 파일을 GitHub 저장소에 모두 커밋합니다.
2. Vercel에서 기존 프로젝트를 다시 배포합니다.
3. Framework Preset은 `Other`로 둡니다.
4. Build Command와 Output Directory는 비워 둡니다.
5. Node.js 22 런타임과 Chromium 패키지는 `package.json`을 통해 자동 설치됩니다.

첫 저장은 Chromium을 준비하느라 조금 느릴 수 있습니다.

## 이미지 용량

Vercel 함수의 요청 본문에는 크기 제한이 있으므로 업로드한 이미지는 브라우저에서 WebP로 자동 축소됩니다.

- 메인 일러스트: 최대 2000px
- 전체·하단 배경: 최대 2200px
- 인물 프로필: 최대 1300px
- 상징 이미지: 최대 1400px
- 앨범 커버: 최대 900px

표시 영역에 필요한 해상도는 유지하면서 서버 전송 용량만 줄입니다.
