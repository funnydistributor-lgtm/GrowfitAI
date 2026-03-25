# GA4 연동 완료 Walkthrough

## 변경 파일
- [index.html](file:///Users/apple/GrowfitAI/index.html)

## 적용된 이벤트 요약

| # | 이벤트명 | 트리거 시점 | 주요 매개변수 |
|---|---|---|---|
| 1 | `click_cta` | CTA 버튼 클릭 | `button_location` (navbar / hero_banner / cta_section) |
| 2 | `section_view` | 섹션 스크롤 도달 | `section_name` (pain_points, how_it_works, features, reviews, cta, app_form) |
| 3 | `start_size_analysis` | AI 분석 시작 클릭 | `child_gender`, `brand_type`, `fit_preference`, `clothing_type` |
| 4 | `view_size_result` | 결과 화면 표시 | `result_current_size` |
| 5 | [share](file:///Users/apple/GrowfitAI/index.html#1236-1245) | 카카오/이메일 공유 | `method` (Kakao / Email) |
| 6 | `click_contact` | 푸터 문의 메일 클릭 | — |

## 구현 방식
- `<head>`에 `G-T9EN2FXVGX` 측정 ID로 gtag.js 초기화
- CTA 링크에 `data-cta` 속성, 섹션에 `data-section` 속성을 부여하여 리스너 자동 등록
- `section_view`는 `IntersectionObserver`(threshold 30%)로 **1회만** 발생

## 검증 결과

![브라우저 확인 스크린샷](/Users/apple/.gemini/antigravity/brain/5eb5a846-454b-44af-85da-1ddd38bcdc28/full_page_view_1774417390020.png)

- ✅ 페이지 정상 로드 (JS 에러 없음)
- ✅ 네비게이션 CTA 클릭 → `#app` 이동 정상
- ✅ GA4 실시간 데이터는 [GA4 실시간 보고서](https://analytics.google.com/)에서 확인 가능
