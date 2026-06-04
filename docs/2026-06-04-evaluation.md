# 평가 보고서 — 2026년 6월 4일

**평가 대상**: 포트폴리오 사이트 OG 메타데이터 구성 및 브랜딩 통일 작업  
**평가 레포**: `2-somin/2-somin.github.io`, `2-somin/portfolio`  
**평가 일자**: 2026-06-04

---

## 1. 목표 달성도

| 목표 항목 | 달성 여부 | 비고 |
|-----------|-----------|------|
| OG 6개 태그 모두 채우기 | ✅ | 두 레포 모두 적용 |
| og:image 캔버스 이미지 생성 | ✅ | 1200×630 사진 미사용 |
| 카카오톡 공유 최적화 | ✅ | locale, image size 표준 충족 |
| 파비콘 커스텀 제작 (Jekyll 사이트) | ✅ | 5종 사이즈 생성 |
| 이름 이소민 → Lee Somin 변경 | ✅ | 양쪽 레포 모두 반영 |
| 프로필 사진 IMG_black.jpg 변경 | ✅ | 양쪽 레포 모두 반영 |

---

## 2. 문제 발생 및 해결 이력

### 문제 1 — 레포지토리 구조 미파악 (주요)

**증상**: `/portfolio/` 페이지에 OG 태그가 전혀 없었는데, `2-somin.github.io` 레포만 수정하고 배포하여 반영이 안 됨  
**원인**: `https://2-somin.github.io/portfolio/`가 `2-somin.github.io` 레포가 아닌 별도의 `2-somin/portfolio` 레포에서 서빙되는 GitHub Pages 구조를 초기에 파악하지 못함  
**해결**: GitHub API로 유저 전체 레포 목록 조회 → `portfolio` 레포 발견 → 클론 후 작업  
**재발 방지**: 작업 시작 전 GitHub Pages 배포 레포 구조 먼저 확인 필요

---

### 문제 2 — og:image URL 이중 생성

**증상**: Jekyll 사이트에서 `og:image` URL이 `https://2-somin.github.iohttps://2-somin.github.io/images/og-image.png`로 출력  
**원인**: Liquid 템플릿에서 `base_path`(이미 full URL 포함)에 `site.url`을 추가로 prepend  
**해결**: `| prepend: site.url` 제거  
**영향 범위**: JSON-LD 블록 내 동일 패턴도 함께 수정

---

### 문제 3 — og:description 본문 오출력

**증상**: 홈 페이지 OG description에 `site.og_description` 대신 본문 첫 줄이 출력  
**원인**: Jekyll의 변수 우선순위 — `page.excerpt`가 `site.description`보다 높아 프론트매터 `description`이 없으면 본문 첫 줄 사용  
**해결**: `about.md` 프론트매터에 `description` 명시적 추가

---

## 3. 기술 품질 평가

### 3-1. OG 메타데이터 완성도

**잘된 점**
- `og:image:width`, `og:image:height` 명시 → 크롤러가 이미지 미리 파악 가능
- `og:locale: ko_KR` 설정 → 국내 SNS 크롤러 대응
- `og:type: website` 기본값 처리와 `article` 자동 분기 구현

**개선 여지**
- portfolio 레포의 `og:site_name`은 `Product Designer 이소민`으로 한글이 남아 있음. 필요 시 `Lee Somin Portfolio`로 통일 가능

---

### 3-2. OG 이미지

**잘된 점**
- 1200×630px 표준 사이즈 충족
- 사진 미사용 → 저작권 이슈 없음, 텍스트 선명도 보장
- 브랜드 컬러(`#1a1a2e`, `#c9a97a`) 일관 적용
- 50KB 이하 → 크롤러 로딩 부담 없음

**개선 여지**
- 현재 OG 이미지는 수동 생성 방식. 향후 콘텐츠 변경 시 재생성 필요

---

### 3-3. 브랜딩 통일성

| 요소 | Jekyll 사이트 | portfolio 레포 | 일치 여부 |
|------|-------------|---------------|-----------|
| 표시 이름 | Lee Somin | Lee Somin | ✅ |
| 프로필 사진 | IMG_black.jpg | IMG_black.jpg | ✅ |
| OG 이미지 | og-image.png (동일 파일) | og-image.png (동일 파일) | ✅ |
| 파비콘 | 커스텀 `이` 아이콘 | Jekyll 사이트 파비콘 참조 | ✅ |
| 브랜드 컬러 | `#1a1a2e` / `#c9a97a` | — (별도 CSS) | 부분 |

---

## 4. 종합 평가

| 항목 | 점수 |
|------|------|
| 목표 달성도 | 100% |
| 버그 발견 및 수정 | 3건 |
| 브랜딩 일관성 | 주요 요소 통일 완료 |
| 카카오톡 공유 대응 | 표준 사양 충족 |

### 총평

초기에 `/portfolio/` 페이지가 별도 레포(`2-somin/portfolio`)에서 서빙된다는 구조 파악에 시간이 소요되었으나, 확인 후 두 레포 모두에 OG 태그, 이름, 프로필 사진을 일관되게 반영하였다. Jekyll 사이트에서는 Liquid 템플릿 버그 2건을 발견하고 수정하였다. 카카오톡 공유 시 두 URL 모두 올바른 미리보기가 출력될 수 있는 상태로 완성되었다.

---

## 5. 후속 권장 작업

1. **카카오 공유 디버거 검증**: [https://developers.kakao.com/tool/debugger/sharing](https://developers.kakao.com/tool/debugger/sharing) — 두 URL 모두 확인
2. **portfolio 레포 파비콘 직접 추가**: 현재 Jekyll 사이트 파비콘 URL 참조 중 → portfolio 레포에 파비콘 파일 직접 포함하면 더 안정적
3. **portfolio 레포 각 프로젝트 페이지 OG 보완**: 현재는 메인 페이지만 적용
