# 전체 기능 목록

이 스킬이 할 수 있는 모든 것을 한 곳에 모았습니다.

## 핵심 자동화

### 1. 한 번의 명령으로 Git 워크플로우
```bash
bash scripts/smart_commit.sh
```
다음을 대체합니다:
```bash
git add .
git commit -m "feat: update files"
git push
```

### 2. 모든 것을 자동 감지
- ✅ git 없음? → 초기화합니다
- ✅ remote 없음? → GitHub 레포를 생성합니다
- ✅ 새 브랜치? → `-u` 플래그로 푸시합니다
- ✅ 기존 브랜치? → 그냥 푸시합니다
- ✅ 변경사항 없음? → 정상적으로 종료합니다

### 3. 브랜치 인식 푸시
**현재 있는 브랜치로 푸시합니다:**
- `main`에 있음 → `origin/main`으로 푸시
- `feature-x`에 있음 → `origin/feature-x`로 푸시
- `bugfix-login`에 있음 → `origin/bugfix-login`으로 푸시

**작동 방식:**
```bash
git rev-parse --abbrev-ref HEAD  # 현재 브랜치 가져오기
git push origin $CURRENT_BRANCH   # 해당 브랜치로 푸시
```

**스마트 업스트림 트래킹:**
- 브랜치가 remote에 존재 → `git push`
- 새 브랜치 → `git push -u origin $BRANCH` (업스트림 설정)

## 커밋 메시지 생성

### 4. 자동 생성 Conventional Commits

변경사항을 분석하여 적절한 conventional commit 메시지를 생성합니다.

**커밋 타입 (자동 감지):**

| 타입 | 트리거 | 예시 |
|------|--------|------|
| `feat:` | 코드 변경의 기본값 | `feat: update 3 file(s)` |
| `fix:` | diff에 "fix" 또는 "bug" 포함 | `fix: resolve login issue` |
| `docs:` | .md, .txt, .rst 파일 | `docs: update 2 file(s)` |
| `test:` | 파일명에 "test" 포함 | `test: update 1 file(s)` |
| `chore:` | package.json, requirements.txt | `chore: update dependencies` |
| `refactor:` | diff에 "refactor" 포함 | `refactor: update 1 file(s)` |

**Scope 감지 (디렉토리에서 자동 추출):**

| 디렉토리 | 결과 |
|----------|------|
| `plugin/file.js` | `feat(plugin): update 1 file(s)` |
| `skill/docs.md` | `docs(skill): update 1 file(s)` |
| `agent/test.py` | `test(agent): update 1 file(s)` |
| `auth/login.ts` | `feat(auth): update 1 file(s)` |
| 루트 파일 | `feat: update 2 file(s)` |

### 5. 커스텀 커밋 메시지

자동 생성 덮어쓰기:
```bash
bash scripts/smart_commit.sh "fix: resolve authentication bug"
bash scripts/smart_commit.sh "feat(api): add new endpoint"
```

### 6. Claude Code 표시

모든 커밋에 포함됩니다:
```
🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

## 레포지토리 관리

### 7. Git 레포지토리 초기화
```bash
# .git 폴더 없음? 자동으로 실행:
git init
```

### 8. GitHub 레포지토리 생성

GitHub CLI를 사용하여 레포 생성:
```bash
gh repo create [name] --public/--private --source=. --remote=origin
```

**기능:**
- ✅ public/private 선택 프롬프트
- ✅ 기본값으로 디렉토리명 사용
- ✅ 커스텀 이름 지정 가능
- ✅ 자동으로 remote 설정

**사용법:**
```bash
# 자동 이름 지정 (디렉토리명 사용)
bash scripts/init_and_push.sh

# 커스텀 이름
bash scripts/init_and_push.sh my-awesome-project
```

### 9. Remote 설정
- ✅ `origin` remote 존재 여부 확인
- ✅ 없으면 GitHub 레포 생성
- ✅ 자동으로 remote URL 설정

## 스마트 동작

### 10. 변경사항 없음 감지
```bash
⚠ No changes to commit
# 정상적으로 종료, 에러 없음
```

### 11. 새 브랜치 처리
새 브랜치 첫 푸시:
```bash
git push -u origin feature-new
⚠ Create PR: https://github.com/user/repo/pull/new/feature-new
```

GitHub 레포의 경우 PR 생성 링크를 표시합니다!

### 12. Diff 통계
성공적인 푸시 후:
```
 2 files changed, 47 insertions(+), 3 deletions(-)
```

### 13. 색상 출력
- 🟢 녹색 → 정보/성공
- 🟡 노란색 → 경고
- 🔴 빨간색 → 에러
- 🔵 파란색 → 단계 표시

### 14. 에러 처리
- `set -e` - 명령어 실패 시 종료
- 명확한 에러 메시지
- 실패 시 0이 아닌 종료 코드

## Claude Code 통합

### 15. 슬래시 명령어

**`/push`** - 변경사항 저장 및 푸시
```
/push
→ Current branch: main
→ Staging all changes...
→ Generated commit message: feat: update 2 file(s)
✅ 완료!
```

**`/new-repo`** - 새 레포지토리 생성
```
/new-repo
What should we name the repository? my-project
→ Creating GitHub repository: my-project
✅ 완료!
```

### 16. 자연어 트리거

다음과 같이 말하면 스킬이 자동 활성화됩니다:
- "변경사항 푸시해줘"
- "github에 저장해줘"
- "커밋하고 푸시해줘"
- "새 레포지토리 만들어줘"
- "작업 백업해줘"
- "이거 푸시해줘"
- "변경사항 저장해줘"

### 17. 스킬 자동 활성화

`.claude/skills/`에 설치되면 Claude가 자동으로:
- 푸시 의도를 감지
- 적절한 스크립트 실행
- 모든 것을 처음부터 끝까지 처리

## 파일 분석

### 18. 스테이징된 파일 분석
```bash
git diff --cached --name-only  # 변경된 파일 목록
git diff --cached --stat       # 통계 표시
```

용도:
- 커밋 타입 결정
- Scope 추출
- 설명 생성
- 변경된 파일 수 계산

### 19. 패턴 매칭

확인 항목:
- 파일 확장자 (`.md`, `.txt`, `.py` 등)
- 디렉토리명 (`plugin/`, `test/` 등)
- Diff 내용 (`fix`, `refactor` 등)
- 의존성 파일 (`package.json` 등)

## 포함되지 않은 것들

투명성을 위해 포함되지 않은 기능:

❌ **브랜치 전환** - 현재 브랜치만 사용
❌ **Pull/fetch** - 푸시 전 동기화 안 함
❌ **Merge 충돌** - 해결 안 함
❌ **대화형 스테이징** - 항상 `git add .`
❌ **커밋 수정** - 새 커밋만
❌ **Rebase/squash** - 선형 히스토리
❌ **다중 remote** - `origin`만
❌ **서브모듈** - 미처리
❌ **Git hooks** - 우회함
❌ **강제 푸시** - `-f` 사용 안 함

## 사용자 유형별 사용 사례

### 엔지니어
- ⚡ **속도:** 세 개의 명령어 대신 하나
- 📝 **편리한 메시지:** 자동 생성 conventional commits
- 🔄 **일관성:** 매번 같은 형식
- 🚀 **빠른 프로토타이핑:** feature 브랜치에서 빠른 반복

### 비개발자
- 🎯 **단순함:** 배워야 할 git 명령어 없음
- 💬 **자연스러움:** "변경사항 저장해줘"가 바로 작동
- 🆘 **안전함:** 설정을 자동 감지하고 수정
- 📚 **교육적:** git이 뒤에서 뭘 하는지 볼 수 있음

### 팀
- 📐 **표준:** 강제된 conventional commits
- 🎓 **온보딩:** 새 멤버는 스크립트 하나만 실행
- 🤖 **CI/CD:** 자동화에서 같은 스크립트 사용
- 📊 **변경 로그:** 히스토리에서 쉽게 자동 생성

### 솔로 개발자
- 🏃 **더 빠르게:** 반복 작업 시간 절약
- 🧠 **덜 생각:** 자동 커밋 메시지
- 📦 **이식성:** 워크플로우를 코드로 공유
- 🔧 **커스터마이즈:** 필요에 따라 스크립트 수정

## 요구사항

### 필수
- ✅ Git 설치됨
- ✅ GitHub CLI (`gh`) - 레포 생성용
- ✅ GitHub 계정
- ✅ 인증 설정됨 (`gh auth login`)

### 선택
- Claude Code (스킬 통합용)
- 기존 git 레포지토리 (없으면 생성)
- Remote 설정 (없으면 생성)

## 요약

**총 기능: 19개**
- 9개 핵심 자동화 기능
- 4개 커밋 메시지 기능
- 3개 레포지토리 관리 기능
- 3개 Claude Code 통합 기능

**하나의 스크립트. 완전한 워크플로우. 모든 브랜치. 엔지니어와 비개발자 모두를 위해.**
