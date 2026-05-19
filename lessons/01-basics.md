# 01. Git과 GitHub 기본기

## 핵심 개념

- Git: 내 컴퓨터에서 코드 변경 이력을 관리하는 도구
- GitHub: Git 저장소를 온라인에 올려 협업하는 서비스
- Repository: 프로젝트와 변경 이력이 들어 있는 저장소
- Commit: 의미 있는 변경사항을 저장한 기록
- Branch: 독립적으로 작업하기 위한 흐름
- Remote: GitHub 같은 외부 저장소 주소

## 오늘의 실습 흐름

1. `git init`으로 저장소를 만든다.
2. `git status`로 현재 상태를 확인한다.
3. 파일을 수정한다.
4. `git add`로 커밋 후보에 올린다.
5. `git commit`으로 변경 이력을 저장한다.

## 꼭 기억할 명령어

```bash
git status
git add README.md
git commit -m "Add study README"
git log --oneline
```

## 체크포인트

- 커밋은 저장 버튼이 아니라 변경 이력의 스냅샷입니다.
- 커밋 메시지는 나중의 나와 협업자를 위한 설명입니다.
- GitHub에 올리기 전에 로컬 Git 흐름이 먼저 익숙해져야 합니다.

