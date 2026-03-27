# Claude Code 스터디 대시보드 프로젝트

## 프로젝트 개요
Claude Code 스터디 팀의 주차별 미션 산출물을 모아서 보여주는 대시보드.
팀원들이 Obsidian으로 MD 파일을 작성하고 GitHub Desktop으로 push하면 대시보드에 자동 반영되는 구조.

## GitHub 정보
- repo: https://github.com/tomost-dada/selfish-aaa-1
- 대시보드 파일: index.html (루트에 위치)
- 배포: Netlify 예정 (Private repo라 GitHub Pages 사용 불가)

## 팀 정보
- 팀원 수: 10명 미만
- 기술 수준: 전원 비개발자
- 툴: Obsidian(MD 작성) + GitHub Desktop(push)

## 폴더 구조 규칙
```
selfish-aaa-1/
├── index.html              ← 대시보드 (절대 수정 금지 - Claude Code가 관리)
├── CLAUDE.md               ← 이 파일
├── README.md
├── week-01/
│   ├── 홍길동.md
│   ├── 김영희.md
│   └── screenshots/
│       └── 홍길동-result.png
├── week-02/
│   └── (동일 구조)
└── week-N/
    └── (동일 구조)
```

## MD 파일 작성 양식
팀원들이 매주 아래 양식으로 작성:
```markdown
# N주차 미션 회고

## 이번 주 한 것
자유롭게 작성

## 결과물 스크린샷
![결과물](screenshots/이름-result.png)

## 참고 링크
- [링크 설명](https://...)
```

## 대시보드 기능 (index.html)
1. **주차별 타임라인** - 주차별 산출물 카드 형태로 표시
2. **팀원별 제출 현황** - 멤버 카드 + 진행률 바 + 주차별 제출 여부 도트
3. **회고글 읽기** - MD 파일을 HTML로 렌더링 (marked.js 사용)
4. **스크린샷 갤러리** - 이미지 masonry 그리드 + 라이트박스

## 대시보드 데이터 로딩 방식
- GitHub API로 repo의 MD 파일 목록을 읽어옴
- `USE_DEMO_DATA = true` 이면 하드코딩된 데모 데이터 사용
- `USE_DEMO_DATA = false` + GITHUB_CONFIG 설정하면 실제 데이터 로드
- Private repo라서 GitHub Token 필요

## GITHUB_CONFIG 설정 위치
index.html 상단:
```javascript
const GITHUB_CONFIG = {
  owner: "tomost-dada",
  repo: "selfish-aaa-1",
  branch: "main",
  token: "",  // GitHub Personal Access Token (repo 권한)
};
const USE_DEMO_DATA = false;
```

## GitHub Token 발급 방법
1. GitHub → Settings → Developer settings
2. Personal access tokens → Tokens (classic)
3. Generate new token → repo 권한 체크 → 생성
4. 발급된 토큰을 GITHUB_CONFIG.token 에 입력

## 배포 방법 (Netlify)
```
netlify.com 가입
→ Add new site → Import from Git → GitHub 연결
→ selfish-aaa-1 repo 선택
→ Deploy
```
배포 후 Push할 때마다 자동 반영됨.

## Claude Code 주요 작업 목록
앞으로 Claude Code에게 시킬 수 있는 작업들:

### 즉시 가능
- `index.html` 디자인 수정
- 새로운 탭/기능 추가 (예: 팀 통계 차트, 댓글 기능)
- 주차별 폴더 자동 생성
- README.md 업데이트
- git add . && git commit -m "메시지" && git push origin main

### 팀원 관리
- 팀원 목록 추가/제거 시 대시보드 자동 반영 (GitHub API 기반이라 별도 설정 불필요)

### 자동화 가능
- 매주 새 week-N 폴더 생성 + .gitkeep 추가
- 미제출 팀원 알림 (GitHub API로 파일 존재 여부 확인)

## 자주 쓰는 Git 명령어
```bash
# 현재 상태 확인
git status

# 전체 변경사항 커밋 후 푸시
git add .
git commit -m "커밋 메시지"
git push origin main

# 최신 내용 가져오기
git pull origin main

# 새 주차 폴더 생성
mkdir -p week-04/screenshots && touch week-04/screenshots/.gitkeep
```

## 참고 사항
- 팀원들은 터미널/코드 전혀 모름 → GitHub Desktop GUI만 사용
- 파일명 규칙: 한글 이름 OK, 스크린샷은 영어+하이픈 권장
- 같은 파일 동시 수정 시 conflict 발생 → 각자 본인 이름 파일만 수정하도록 안내
- Obsidian iCloud Sync, Obsidian Sync 사용 금지 (GitHub Desktop과 충돌)
