# 엑셀 양식 메이커 — Claude Code 인수인계 문서

## 프로젝트 개요

**프로젝트명:** 엑셀 양식 메이커 (Excel Form Maker)  
**목적:** 비개발자도 AI 추천을 활용해 원하는 엑셀 양식을 손쉽게 만들고 다운로드할 수 있는 웹 앱  
**경로:** `D:\IDR-AI교육\excel-form-builder`  
**현재 상태:** MVP 완성 (단일 HTML 파일), 추가 기능 개발 예정

---

## 기술 스택

| 영역 | 기술 | 비고 |
|------|------|------|
| 프론트엔드 | HTML + CSS + Vanilla JS | 프레임워크 없음 |
| AI 헤더 추천 | Google Gemini API (gemini-2.0-flash) | 무료 티어 사용 |
| 엑셀 생성 | SheetJS (xlsx 0.18.5) | CDN 로드 |
| 드래그앤드롭 | HTML5 Drag and Drop API | 라이브러리 없음 |
| 스타일 | 순수 CSS (CSS 변수 기반, 다크모드 지원) | |

**비용:** 전체 무료 (Gemini API 무료 티어: 일 최대 1,000회 요청)

---

## 현재 구현된 기능 (MVP)

### 핵심 기능
- [x] Gemini API 키 입력 (표시/숨기기 토글)
- [x] 양식 용도 텍스트 입력
- [x] 빠른 선택용 프리셋 버튼 10종
- [x] Gemini AI 헤더 추천 (gemini-2.0-flash 모델)
- [x] 추천 헤더 칩 선택/해제 토글
- [x] 헤더 직접 입력 추가 (최대 20개)
- [x] 헤더 드래그앤드롭 순서 변경
- [x] 헤더 색상 변경 (10가지 색상 팔레트)
- [x] 헤더명 인라인 편집
- [x] 데이터 형식 설정 (텍스트/숫자/날짜/선택)
- [x] 빈 행 수 슬라이더 (5~200행)
- [x] 실시간 미리보기 그리드 (샘플 데이터 3행 표시)
- [x] 진행 단계 표시 (스텝 인디케이터)
- [x] xlsx 다운로드 (헤더 색상 적용, 열 너비 자동 조정)
- [x] 초기화 버튼 (confirm 다이얼로그)
- [x] CSS 다크모드 자동 지원

---

## 파일 구조 (현재)

```
excel-form-builder/
└── excel_form_maker.html    ← 모든 기능이 담긴 단일 파일 (MVP)
```

---

## 향후 개발 예정 기능

아래 기능들을 우선순위 순으로 추가 개발 예정:

### 🔴 높은 우선순위
1. **샘플 데이터 직접 입력** — 미리보기 셀을 클릭해서 샘플값 직접 입력
2. **드롭다운 옵션 설정** — 데이터 형식이 '선택'일 때 옵션 목록 입력 (엑셀 데이터 유효성 검사 연동)
3. **양식 저장/불러오기** — localStorage에 양식 설정 저장, 불러오기

### 🟡 중간 우선순위
4. **다중 시트 지원** — 시트 탭 추가/삭제, 시트별 헤더 설정
5. **헤더 고정 행 옵션** — 엑셀 틀 고정(freeze panes) 적용 여부 선택
6. **파일명 커스터마이징** — 다운로드 파일명 직접 입력

### 🟢 낮은 우선순위
7. **양식 템플릿 공유** — JSON으로 내보내기/가져오기
8. **엑셀 → 양식 역변환** — 기존 xlsx 업로드해서 헤더 추출

---

## 주요 코드 구조 (HTML 단일 파일 기준)

```
[전역 변수]
- headers[]        : 현재 헤더 목록 { name, type, color }
- recHeaders[]     : AI 추천 헤더 목록 (문자열 배열)
- dragSrcIdx       : 드래그 중인 헤더 인덱스
- openColorIdx     : 색상 피커가 열린 헤더 인덱스

[주요 함수]
- init()                 : 프리셋 칩 렌더링, 초기 상태 설정
- toggleKey()            : API 키 표시/숨기기
- getAISuggestions()     : Gemini API 호출 → recHeaders 업데이트
- renderRecChips()       : 추천 헤더 칩 UI 렌더링
- addHeader()            : 헤더 직접 추가
- renderHeaderList()     : 헤더 목록 UI 렌더링 (드래그, 색상 피커 포함)
- renderPreview()        : 우측 미리보기 그리드 렌더링
- updateSteps()          : 상단 스텝 인디케이터 상태 업데이트
- downloadExcel()        : SheetJS로 xlsx 생성 및 다운로드
- resetAll()             : 전체 초기화
```

---

## Gemini API 호출 스펙

```
Endpoint : https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent
Method   : POST
Auth     : ?key={API_KEY} (쿼리 파라미터)
Request  : { contents: [{ parts: [{ text: prompt }] }], generationConfig: { temperature: 0.7, maxOutputTokens: 512 } }
Response : data.candidates[0].content.parts[0].text → JSON 파싱 → { headers: [...] }
```

**무료 티어 제한 (2026년 3월 기준)**
- gemini-2.0-flash: 분당 15회, 일 1,000회
- 개인 프로젝트 수준에서는 제한 없이 사용 가능

---

## 디자인 시스템

### 색상 (CSS 변수, 라이트/다크 자동 전환)
```css
--bg        : 메인 배경 (흰색/어두운 배경)
--bg2       : 보조 배경 (카드, 입력창 배경)
--bg3       : 페이지 배경
--text      : 주요 텍스트
--text2     : 보조 텍스트
--text3     : 힌트 텍스트
--border    : 기본 테두리 (rgba 10%)
--border2   : 강조 테두리 (rgba 18%)
```

### 헤더 색상 팔레트 (10종)
```js
const COLORS = [
  '#4A7DE8','#2E9E6B','#E8834A','#9B6DE8','#E84A6F',
  '#1AA3C4','#CBA020','#5D8A3C','#C45A8A','#777777'
];
```

### 레이아웃
- 좌측 패널: 340px 고정, 우측 패널: 나머지 공간
- 모바일(720px 이하): 단일 컬럼으로 전환

---

## Claude Code에서 이어서 개발하는 방법

```bash
# 1. 프로젝트 폴더로 이동
cd D:\IDR-AI교육\excel-form-builder

# 2. Claude Code 실행
claude

# 3. 아래 내용을 Claude Code에 전달
```

**Claude Code에 전달할 첫 메시지 예시:**
```
CLAUDE.md 파일을 읽어서 프로젝트 컨텍스트를 파악해줘.
그 다음 excel_form_maker.html 파일을 기반으로
[원하는 기능] 기능을 추가 개발해줘.
```

---

## 참고 링크

- Gemini API 문서: https://ai.google.dev/gemini-api/docs
- Gemini API 키 발급: https://aistudio.google.com
- SheetJS 문서: https://docs.sheetjs.com
- SheetJS CDN: https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js
