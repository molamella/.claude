---
allowed-tools: Bash(bash scripts/*)
---

새 GitHub 레포지토리를 생성하고 현재 프로젝트를 푸시합니다.

사용자에게 레포지토리 이름을 물어본 후 실행:

```bash
bash scripts/init_and_push.sh [repo-name]
```

수행 작업:
1. 필요시 git 초기화
2. 지정된 이름으로 새 GitHub 레포지토리 생성
3. remote 연결 설정
4. 모든 파일 커밋
5. GitHub에 푸시

명령어 실행 전에 사용자에게 레포지토리 이름을 물어보세요.
