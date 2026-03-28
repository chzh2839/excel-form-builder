# 📋 엑셀 양식 메이커 (Excel Form Builder)

> AI 추천을 활용해 원하는 엑셀 양식을 손쉽게 만들고 다운로드할 수 있는 웹 앱

[![HTML](https://img.shields.io/badge/HTML-Single%20File-orange?logo=html5)](./excel_form_maker.html)
[![Gemini](https://img.shields.io/badge/AI-Gemini%202.0%20Flash-blue?logo=google)](https://aistudio.google.com)
[![SheetJS](https://img.shields.io/badge/Excel-xlsx--js--style-green)](https://github.com/gitbrent/xlsx-js-style)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](#)

---

## ✨ 주요 기능

| 기능 | 설명 |
|------|------|
| 🤖 AI 헤더 추천 | Gemini API로 양식 용도에 맞는 헤더를 자동 추천 |
| 🎨 헤더 스타일 | 색상 10가지, 드래그앤드롭 순서 변경, 인라인 이름 편집 |
| 📊 데이터 타입 | 텍스트 / 숫자 / 날짜 / 선택(드롭다운) 설정 |
| 👁️ 실시간 미리보기 | 헤더 구성 즉시 오른쪽 패널에 반영 |
| 💾 양식 저장/불러오기 | localStorage에 최대 10개 양식 설정 저장 |
| 📁 파일명 커스터마이징 | 다운로드 파일명 직접 입력 |
| 🔒 헤더 틀 고정 | 엑셀 스크롤 시 헤더 + 안내 행 고정 |
| ✅ 데이터 유효성 검사 | 타입별 입력 제한 및 안내 메시지 자동 적용 |

---

## 🚀 사용 방법

### 1. API 키 준비

[Google AI Studio](https://aistudio.google.com/app/apikey)에서 **무료** Gemini API 키를 발급받습니다.

> ⚠️ Google Cloud Console 키가 아닌 **AI Studio 키**를 사용해야 무료 티어가 적용됩니다.

### 2. 앱 실행

별도 설치 없이 `excel_form_maker.html` 파일을 브라우저에서 바로 열면 됩니다.

```bash
# 파일을 브라우저로 열기 (Windows)
start excel_form_maker.html
```

### 3. 양식 만들기

```
1️⃣  API 키 입력
2️⃣  양식 용도 입력  →  AI 헤더 추천 받기
3️⃣  원하는 헤더 선택 / 직접 추가
4️⃣  색상·타입·순서 설정
5️⃣  파일명 입력  →  엑셀 다운로드 (.xlsx)
```

---

## 🛠️ 기술 스택

| 영역 | 기술 |
|------|------|
| Frontend | HTML + CSS + Vanilla JS (프레임워크 없음) |
| AI | Google Gemini 2.0 Flash (`v1beta`) |
| Excel 생성 | [xlsx-js-style](https://github.com/gitbrent/xlsx-js-style) 1.2.0 |
| 드래그앤드롭 | HTML5 Drag and Drop API |

**비용: 완전 무료** (Gemini API 무료 티어: 일 최대 1,500회)

---

## 📦 파일 구조

```
excel-form-builder/
├── excel_form_maker.html   # 모든 기능이 담긴 단일 파일
├── README.md
└── CLAUDE.md               # 개발 인수인계 문서
```

---

## 🖥️ 화면 구성

```
┌─────────────────────────────────────────────────────┐
│  📋 엑셀 양식 메이커    [1 용도입력] [2 헤더구성] [3 다운로드] │
├──────────────────────┬──────────────────────────────┤
│                      │                              │
│  API 키              │       실시간 미리보기         │
│  저장/불러오기        │                              │
│  양식 용도           │  ┌─────┬──────┬─────┐        │
│  AI 추천 헤더        │  │ 헤더1│ 헤더2│헤더3│        │
│  헤더 목록           │  ├─────┼──────┼─────┤        │
│  (색상·타입·순서)    │  │안내행│      │     │        │
│  빈 행 수            │  ├─────┼──────┼─────┤        │
│  틀 고정 옵션        │  │     │      │     │        │
│                      │                              │
│              [파일명______] [초기화] [⬇ 다운로드]  │
└──────────────────────┴──────────────────────────────┘
```

---

## 📝 라이선스

MIT License
