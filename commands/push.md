---
allowed-tools: Bash(bash scripts/*)
---

모든 변경사항을 스테이징하고, 커밋 메시지를 생성한 후, GitHub에 푸시합니다.

git 레포지토리가 없으면 초기화하고 새 GitHub 레포지토리를 생성합니다.

스마트 커밋 스크립트 실행:

```bash
bash scripts/smart_commit.sh
```

스크립트가 수행하는 작업:
1. git 초기화 확인 (없으면 설정)
2. remote 레포지토리 확인 (없으면 GitHub에 생성)
3. 모든 변경사항 스테이징
4. 파일 분석하여 의미 있는 커밋 메시지 생성
5. conventional 형식으로 커밋
6. GitHub에 푸시

git 명령어를 알 필요 없습니다!
