---
title: "Electron 데스크탑 앱 빠르게 시작하기"
date: 2025-08-08 12:00:00 +0900
categories: [Electron, Desktop]
tags: [electron, react, 개발환경, 크로스플랫폼]
author: chanjonglee
toc: true
---

## 개요

Electron은 **웹 기술(HTML/CSS/JS)** 을 기반으로 **윈도우, 맥, 리눅스 데스크탑 앱**을 만들 수 있는 프레임워크입니다.  
Slack, Visual Studio Code, Discord 같은 다양한 데스크톱 어플리케이션들이 Electron 기반으로 만들어졌습니다.

### ✅ Electron의 장점!

- **크로스 플랫폼**: 한 번 개발하면 간단한 설정을 통해 macOS, Windows, Linux에서 모두 실행 가능
- **웹 기술 기반**: React, Vue, Next.js 등 웹 프레임워크 그대로 사용 가능
- **빠른 개발 속도**: 프로토타이핑 및 MVP 제작에 탁월
- **강력한 커뮤니티와 생태계**: 다양한 플러그인과 자동 업데이트, 빌드 툴 존재

### ⚠️ 단점도 존재합니다 ㅠㅠ..

- **무거운 실행 파일**: Chromium 기반이라 용량이 크고 메모리 사용량이 높음
- **배터리/성능 최적화가 필요함**: Native 앱에 비해 자원 사용이 많음
- **보안에 민감할 수 있음**: 메인/렌더러 분리 및 IPC 설계에 주의 필요

> 개인적으로는 Electron의 **높은 생산성**에 깊이 반했습니다.  
> React + Electron 조합으로 데스크톱 앱을 만들면서  
> 몇 시간 만에 UI/기능이 완성되는 경험은 매우 인상적이었습니다.

이 포스팅에서는 Electron 프로젝트를 처음 시작하는 방법과 간단한 예제 코드를 소개합니다.

---

## 설치

터미널에서 다음 명령어로 Electron을 설치할 수 있습니다.

```bash
npm init -y
npm install --save-dev electron
```

---

## 메인 프로세스 구성

Electron 앱은 **메인 프로세스(main.js)** 와 **렌더러 프로세스(index.html 등)** 로 구성됩니다.  
아래는 최소 구성의 예제입니다.

```js
// main.js
const { app, BrowserWindow } = require("electron");
const path = require("path");

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, "preload.js"), // 선택사항
    },
  });

  win.loadFile("index.html");
}

app.whenReady().then(() => {
  createWindow();

  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) createWindow();
  });
});

app.on("window-all-closed", () => {
  if (process.platform !== "darwin") app.quit();
});
```

---

## index.html 예시

렌더러 영역에서 표시할 간단한 HTML 예제입니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello Electron</title>
  </head>
  <body>
    <h1>Electron 앱이 실행되었습니다 🎉</h1>
  </body>
</html>
```

---

## 실행 방법

`package.json`에 아래 스크립트를 추가하세요.

```json
"scripts": {
  "start": "electron ."
}
```

그 다음 실행:

```bash
npm run start
```

---

## 마무리

이제 Electron을 이용해 기본적인 데스크탑 앱을 만들 수 있습니다.  
단 몇 줄의 코드만으로도 macOS, Windows에서 실행 가능한 앱이 만들어지는 경험은 굉장히 흥미롭습니다.

앞으로는 다음과 같은 주제를 중심으로 **Electron 실전 활용법**을 다뤄보고 싶습니다:

- ✅ **Next.js + Electron 통합 개발 환경 구축**
- ✅ 자동 업데이트와 앱 배포 (Mac/Windows)
- ✅ Ant Design, Framer Motion을 활용한 데스크탑 UI 구성
- ✅ 업무 자동화를 위한 Python + Electron 연동
- ✅ React Native와 비교한 데스크탑 앱 vs 모바일 앱 개발 경험

> 특히 **Next.js와 Electron을 함께 사용하는 구조**에 대해 중점적으로 다룰 예정입니다.  
> 웹에서 다루던 구조와 상태관리, 라우팅 개념을 그대로 데스크탑에 적용할 수 있는 점이 매력적이었습니다.

---

## 참고자료

- [Electron 공식 문서](https://www.electronjs.org/docs)
- [Visual Studio Code가 Electron으로 만들어졌다는 사실](https://code.visualstudio.com/)
