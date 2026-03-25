# GA4 이벤트 설계 및 연동 계획

측정 ID: `G-T9EN2FXVGX`

## 📊 GA4 추천 이벤트 항목 (총 6개)

### 1. `click_cta` — CTA 버튼 클릭
사용자가 앱 섹션으로 이동하는 버튼을 눌렀을 때 발생합니다.

| 매개변수 | 값 예시 | 설명 |
|---|---|---|
| `button_location` | `navbar`, `hero_banner`, `cta_section` | 클릭 위치 구분 |

> [!TIP]
> `navbar`(플로팅 바) 클릭 = 목적이 명확한 사용자 또는 재방문자,  
> `hero_banner` / `cta_section` 클릭 = 콘텐츠에 설득된 사용자.  
> 이 구분으로 **방문 의도와 전환 경로**를 비교 분석할 수 있습니다.

---

### 2. `section_view` — 섹션별 스크롤 도달
사용자가 각 주요 섹션에 도달(뷰포트 진입)했을 때 발생합니다. IntersectionObserver를 사용하여 섹션이 화면에 보이면 1회만 트리거합니다.

| 매개변수 | 값 예시 | 설명 |
|---|---|---|
| `section_name` | `pain_points`, `how_it_works`, `features`, `reviews`, `cta`, `app_form` | 도달한 섹션 이름 |

> [!TIP]
> 이 지표를 퍼널로 구성하면 **어떤 섹션에서 이탈이 많은지** 즉시 파악 가능합니다.  
> 예: Reviews 도달률 60% → CTA 도달률 30% = 리뷰 섹션 이후 이탈 집중

---

### 3. `start_size_analysis` — 분석 시작
사용자가 폼 입력 후 'AI 분석 시작' 버튼을 눌렀을 때 발생합니다.

| 매개변수 | 값 예시 | 설명 |
|---|---|---|
| `child_gender` | `boy`, `girl` | 성별 |
| `brand_type` | `kr`, `global` | 국내/글로벌 브랜드 |
| `fit_preference` | `exact`, `loose` | 핏 선호 |
| `clothing_type` | `set`, `top`, `bottom` | 의류 종류 |

---

### 4. `view_size_result` — 분석 결과 도출
로딩 완료 후 사이즈 결과가 실제 화면에 표시되었을 때 발생합니다.

| 매개변수 | 값 예시 | 설명 |
|---|---|---|
| `result_current_size` | `100` | 현재 추천 사이즈 |

---

### 5. [share](file:///Users/apple/GrowfitAI/index.html#1213-1220) — 결과 공유
결과 화면에서 카카오톡 또는 이메일로 공유할 때 발생합니다.

| 매개변수 | 값 예시 | 설명 |
|---|---|---|
| `method` | [Kakao](file:///Users/apple/GrowfitAI/index.html#1213-1220), [Email](file:///Users/apple/GrowfitAI/index.html#1221-1228) | 공유 수단 |
| `content_type` | `size_result` | 공유 콘텐츠 유형 |

---

### 6. `click_contact` — 하단 문의 메일 클릭
푸터의 `hello@growfit.ai` 링크를 클릭했을 때 발생합니다.

| 매개변수 | 없음 | |
|---|---|---|

---

## 🔄 사용자 여정 퍼널 요약

```
페이지 방문 (page_view, GA4 자동 수집)
  → 섹션 탐색 (section_view: pain_points → how_it_works → features → reviews → cta)
    → CTA 클릭 (click_cta: navbar / hero_banner / cta_section)
      → 분석 시작 (start_size_analysis)
        → 결과 확인 (view_size_result)
          → 공유 (share: Kakao / Email)
```

## Proposed Changes

### [MODIFY] [index.html](file:///Users/apple/GrowfitAI/index.html)

1. `<head>`에 GA4 gtag.js 초기화 스크립트 삽입
2. 각 CTA 링크(`a[href="#app"]`)에 `click_cta` 이벤트 리스너 부착 (위치별 `button_location` 구분)
3. 주요 `<section>`에 `data-section` 속성 부여 → IntersectionObserver로 `section_view` 발생
4. [calculateSize()](file:///Users/apple/GrowfitAI/index.html#1138-1212) 함수 내에 `start_size_analysis` 이벤트 삽입
5. 결과 표시 완료 시점에 `view_size_result` 이벤트 삽입
6. [shareKakao()](file:///Users/apple/GrowfitAI/index.html#1213-1220), [shareEmail()](file:///Users/apple/GrowfitAI/index.html#1221-1228) 내에 [share](file:///Users/apple/GrowfitAI/index.html#1213-1220) 이벤트 삽입
7. 푸터 메일 링크에 `click_contact` 이벤트 삽입
