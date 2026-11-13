# waggle-web

와글와글(대구·경북 대학생 팀 매칭 플랫폼) 프론트엔드. React 18 + Vite 7, JavaScript(TypeScript 아님), React Router 6, Axios, Zustand, Emotion, MUI 7, react-quill-new + DOMPurify.

## 원격과 PR
- `origin` = `ming920/waggle-web` (fork, 작업 대상), `upstream` = `Wagglex2/waggle-web` (원본, push 차단됨)
- push는 `origin`에만. PR은 fork의 `main`으로: `gh pr create -R ming920/waggle-web --base main`
- 원본 레포(`Wagglex2/*`)에는 push·PR·이슈·코멘트를 만들지 않음

## 작업 단위
- 작업은 fork의 GitHub Issues로 관리. 전체 목록은 `ming920/waggle-was#25`, 백엔드 짝 이슈는 각 이슈 본문에 링크
- 이슈 하나 = 브랜치 하나 = PR 하나. PR 본문에 `Closes #<번호>`
- 이슈 범위 밖 문제를 발견하면 고치지 말고 PR 본문이나 새 이슈로 남김
- `needs-decision` 라벨 이슈는 이슈에 정책이 확정되기 전까지 구현하지 않음
- 테스트 프레임워크가 없음 → 버그 수정 PR에는 재현·확인 절차(화면, 입력값, 기대 결과)를 적음
- 성능 이슈: 수정 전후를 같은 방법(`npm run build` 산출물 크기, Lighthouse 등)으로 측정

## 명령
- 설치: `npm ci` (Node 20, 배포 워크플로 기준)
- 개발 서버: `npm run dev`
- 검증: `npm run lint` && `npm run build`
- 포맷: 파일 전체 재포맷 금지, 바꾼 부분만 `.prettierrc` 스타일에 맞춤 (포맷 통일은 #12)

## 브랜치·커밋
- 브랜치: `fix/<이슈번호>-<요약>`, `feat/...`, `refactor/...`, `perf/...`
- 커밋: `feat:`, `fix:`, `refactor:`, `perf:`, `style:`, `docs:`, `ci:` + 한국어 요약
- `.omc/`, `.DS_Store`, `.env*`는 커밋하지 않음

## 구조
- `@/` = `src/` (vite alias)
- `src/api/api.js`: axios 인스턴스와 토큰 갱신 인터셉터. 새 API 호출은 raw axios 대신 이 인스턴스 사용
- `src/stores/`: zustand store (`useAuthStore`가 인증 상태)
- `src/routes/`: `ProtectedRoute`, `PublicRoute`
- 환경변수 `VITE_API_KEY` = 백엔드 base URL (이름과 달리 키가 아님, #13)
- 백엔드 API 스펙은 `../waggle-was`의 컨트롤러·DTO 기준

## 알아둘 것
- `frontend-deploy.yml`은 main push 시 S3 배포. fork에는 시크릿이 없고, 원본 레포에서만 돌게 하는 작업은 #1
- 사용자 입력 HTML은 DOMPurify로 정화한 뒤 렌더링(`Details.jsx`). `dangerouslySetInnerHTML`을 새로 쓰지 않음
- `body { min-width: 1100px }`로 데스크톱 전용 상태 (#14)
