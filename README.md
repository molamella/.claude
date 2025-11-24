# Git 쉽게 사용하기

**비개발자를 위한 버전 관리.** git 명령어를 배우지 않고도 GitHub에 작업물을 저장하세요.

디자이너, 작가, PM, 데이터 사이언티스트, 연구원 등 버전 관리가 필요하지만 git 전문가가 되고 싶지 않은 모든 분들에게 적합합니다.

## 이 스킬을 만든 이유

Git은 강력하지만 어렵습니다:
```bash
git add .
git commit -m "???"  # 뭐라고 써야 하지?
git push             # 어디로? 어떻게?
```

이 스킬은 이렇게 간단하게 만들어줍니다:
```bash
/push
# 끝! ✅
```

## 기능

- 🎯 **git 지식 불필요** - Claude Code에서 `/push`만 입력하세요
- 🚀 **GitHub 레포 자동 생성** - 처음이라면 자동으로 설정해드립니다
- 📝 **커밋 메시지 자동 생성** - 형식을 고민할 필요 없어요
- ⚡ **한 번의 명령으로** - 5개 이상의 git 명령어를 대체합니다
- 🤖 **Claude Code와 함께** - 자연어 또는 슬래시 명령어 사용 가능

## 빠른 시작 (비개발자용)

1. **초기 설정** (5분):
   ```bash
   # GitHub CLI 설치
   brew install gh

   # GitHub 로그인
   gh auth login

   # Claude Code에 이 스킬 설치
   mkdir -p ~/.claude/skills/git-pushing
   # 파일 복사 (설치 섹션 참고)
   ```

2. **작업물을 저장하고 싶을 때마다:**
   - 프로젝트에서 Claude Code 열기
   - `/push` 입력
   - 완료! 작업물이 GitHub에 저장되었습니다 ✅

이게 전부입니다! 외워야 할 git 명령어가 없어요.

## 슬래시 명령어

설치 후 Claude Code에서 다음 명령어를 사용할 수 있습니다:

- **`/push`** - 모든 변경사항을 저장하고 GitHub에 푸시
  - 처음이라면? 레포지토리가 자동으로 생성됩니다
  - 이미 설정되어 있다면? 변경사항만 저장합니다

- **`/new-repo`** - 원하는 이름으로 새 GitHub 레포지토리 생성
  - Claude가 이름을 물어봅니다
  - 모든 설정을 자동으로 해줍니다

## 설치

### 1. GitHub CLI 설치 (새 레포 생성에 필요)

```bash
# macOS
brew install gh

# GitHub 인증
gh auth login
```

안내에 따라 GitHub 계정으로 인증하세요.

### 2. Claude Code에 스킬 설치

**권장: 개인 설치 (모든 프로젝트에서 사용 가능)**

```bash
# skills 디렉토리 생성
mkdir -p ~/.claude/skills/git-pushing

# 모든 파일 복사
cp SKILL.md ~/.claude/skills/git-pushing/
cp -r scripts ~/.claude/skills/git-pushing/
cp -r .claude/commands ~/.claude/skills/git-pushing/

# 스크립트 실행 권한 부여
chmod +x ~/.claude/skills/git-pushing/scripts/*.sh
```

**대안: 프로젝트별 설치 (해당 프로젝트에서만 사용)**
```bash
mkdir -p .claude/skills/git-pushing
cp SKILL.md .claude/skills/git-pushing/
cp -r scripts .claude/skills/git-pushing/
cp -r .claude/commands .claude/skills/git-pushing/
chmod +x .claude/skills/git-pushing/scripts/*.sh
```

**설치되는 것들:**
- ✅ 스킬 (Claude에게 git 도움 시점 알려줌)
- ✅ 스크립트 (실제 git 작업 수행)
- ✅ 슬래시 명령어 (`/push` 와 `/new-repo`)

## 사용법

### 비개발자용 (가장 쉬움!)

Claude Code에서 슬래시 명령어만 사용하세요:

**작업 저장:**
```
/push
```

**새 레포지토리 생성:**
```
/new-repo
```

이게 전부입니다! git 명령어가 필요 없어요.

### 대안: Claude에게 말하기

자연스럽게 Claude에게 요청할 수도 있습니다:
- "내 변경사항 GitHub에 저장해줘"
- "이 변경사항 푸시해줘"
- "이 프로젝트에 새 레포지토리 만들어줘"
- "내 작업 백업해줘"

Claude가 자동으로 적절한 명령어를 실행합니다.

### 고급 사용자용: 직접 명령어 실행

직접 명령어를 실행하고 싶다면:

```bash
# 변경사항 저장 및 푸시
bash scripts/smart_commit.sh

# 원하는 이름으로 새 레포 생성
bash scripts/init_and_push.sh my-project-name
```

### 자동으로 처리되는 것들

스크립트가 프로젝트 상태를 자동으로 확인합니다:

1. **git이 초기화되지 않았다면?**
   - 표시: `⚠ No git repository found`
   - 표시: `▸ Creating new repository...`
   - `git init` 실행 및 GitHub 레포 생성

2. **remote가 설정되지 않았다면?**
   - 표시: `⚠ No remote repository configured`
   - 표시: `▸ Creating new repository...`
   - GitHub 레포 생성 및 remote 설정

3. **모든 것이 이미 설정되어 있다면?**
   - 표시: `→ Current branch: main`
   - 일반적으로 커밋 및 푸시

새 프로젝트든 기존 레포든 같은 명령어만 실행하면 됩니다!

### 커스텀 옵션

**커스텀 메시지 사용:**
```bash
bash scripts/smart_commit.sh "feat: add user authentication"
```

**커스텀 레포 이름 사용:**
```bash
bash scripts/init_and_push.sh my-awesome-project
```

**둘 다:**
```bash
bash scripts/init_and_push.sh my-project "feat: initial implementation"
```

## Conventional Commit 타입

스킬이 파일 변경사항을 기반으로 커밋 타입을 자동 감지합니다:

- `feat:` - 새 기능 (기본값)
- `fix:` - 버그 수정 (diff에서 "fix" 또는 "bug" 감지)
- `docs:` - 문서 (.md, .txt, .rst 파일)
- `test:` - 테스트 파일 (파일명에 "test" 포함)
- `chore:` - 의존성 (package.json, requirements.txt 등)
- `refactor:` - 리팩토링 (diff에서 "refactor" 감지)

## Scope 감지

파일 경로를 기반으로 scope를 자동 추가합니다:

- `plugin/` 내 파일 → `feat(plugin):`
- `skill/` 내 파일 → `feat(skill):`
- `agent/` 내 파일 → `feat(agent):`
- 첫 번째 디렉토리명 → `feat(dirname):`

## 예시

### 예시 1: 완전히 새 프로젝트 (자동 감지)

```bash
$ cd my-new-project
$ bash scripts/smart_commit.sh

⚠ No git repository found
▸ Creating new repository...
→ Git not initialized in this directory
→ Initializing git repository...
→ Git initialized!
▸ No remote repository configured
→ Creating GitHub repository: my-new-project
Make repository public or private? [public/private] (default: public): public
▸ Creating repository on GitHub...
→ Repository created successfully!
→ Remote set to: git@github.com:yourusername/my-new-project.git
→ Proceeding to initial commit and push...
→ Current branch: main
→ Staging all changes...
→ Generated commit message: feat: update 5 file(s)
→ Created commit: abc1234
→ Pushing to origin/main...
→ Successfully pushed new branch to origin/main

→ 🎉 Repository successfully created and pushed!
View at: https://github.com/yourusername/my-new-project
```

### 예시 2: 기존 레포 (자동 감지)

```bash
$ bash scripts/smart_commit.sh

→ Current branch: main
→ Staging all changes...
→ Generated commit message: feat(skill): update 2 file(s)
→ Created commit: def5678
→ Pushing to origin/main...
→ Successfully pushed to origin/main
 2 files changed, 47 insertions(+), 3 deletions(-)
```

### 예시 3: 커스텀 메시지 (모든 프로젝트 상태에서)

```bash
$ bash scripts/smart_commit.sh "fix: resolve authentication bug"

→ Current branch: feature-auth
→ Staging all changes...
→ Using provided message: fix: resolve authentication bug
→ Created commit: ghi9012
→ Pushing to origin/feature-auth...
→ Successfully pushed to origin/feature-auth
```

### 예시 4: 명시적 레포 이름 (고급)

```bash
$ bash scripts/init_and_push.sh cool-project-name

→ Git not initialized in this directory
→ Initializing git repository...
→ Git initialized!
→ Creating GitHub repository: cool-project-name
...
```

## 활성화 방법 (비개발자용)

스킬은 다음과 같은 경우에 자동 활성화됩니다:

**슬래시 명령어 사용:**
- `/push` - 작업 저장 및 업로드
- `/new-repo` - 새 레포지토리 생성

**또는 자연스럽게 요청:**
- "내 변경사항 GitHub에 저장해줘"
- "내 작업 백업해줘"
- "이 변경사항 푸시해줘"
- "새 레포지토리 만들어줘"
- "이거 GitHub에 어떻게 저장해?"
- "버전 관리가 필요해"

## 요구사항

- Git 설치됨
- GitHub CLI (`gh`) - 새 레포지토리 생성용
- GitHub 계정
- SSH 키 또는 HTTPS 인증 설정됨

## 문제 해결

### "GitHub CLI (gh) is not installed"
```bash
brew install gh
gh auth login
```

### "Not authenticated with GitHub"
```bash
gh auth login
```

### "Permission denied (publickey)"
SSH 키 설정:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
# 출력을 https://github.com/settings/keys 에 추가
```

### 스크립트 실행 불가
```bash
chmod +x scripts/*.sh
```

## 작동 원리

### smart_commit.sh (메인 진입점)

항상 호출하는 스마트 스크립트:

1. **git 상태 확인**
   - `.git` 폴더 없음? → `init_and_push.sh`에 위임
   - remote 미설정? → `init_and_push.sh`에 위임
   - 모든 준비 완료? → 커밋 진행

2. **커밋 및 푸시**
   - `git add .`로 모든 변경사항 스테이징
   - 파일과 diff를 분석하여 커밋 타입 결정
   - 디렉토리 구조에서 scope 감지
   - Claude Code footer가 포함된 conventional commit 생성
   - remote에 푸시 (새 브랜치는 `-u` 사용)
   - GitHub 레포의 경우 PR 링크 표시

### init_and_push.sh (필요시 자동 호출)

새 레포지토리 생성 시에만 호출:

1. `.git` 폴더 없으면 `git init` 실행
2. remote 없으면 `gh`로 GitHub 레포 생성
3. remote origin 설정
4. 작업 완료를 위해 `smart_commit.sh` 다시 호출

**직접 호출할 일은 거의 없습니다** - `smart_commit.sh`가 자동으로 호출합니다!

## 라이선스

MIT
