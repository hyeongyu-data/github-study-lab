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
6. GitHub에 원격 저장소를 만들고 `origin`으로 연결한다.
7. `git push`로 로컬 커밋을 GitHub에 업로드한다.

## 꼭 기억할 명령어

```bash
git status
git add README.md
git commit -m "Add study README"
git log --oneline
git remote -v
git push -u origin main
```

## GitHub 업로드 흐름

명령어만으로 GitHub에 올릴 때는 GitHub CLI인 `gh`를 사용할 수 있습니다.

```bash
brew install gh
gh auth login
gh repo create github-study-lab --public --source=. --remote=origin --push
```

업로드가 끝난 뒤에는 아래 명령어로 연결 상태를 확인합니다.

```bash
git remote -v
git status
```

## 체크포인트

- 커밋은 저장 버튼이 아니라 변경 이력의 스냅샷입니다.
- 커밋 메시지는 나중의 나와 협업자를 위한 설명입니다.
- `origin`은 보통 내 GitHub 원격 저장소를 가리키는 이름입니다.
- `git push -u origin main`은 현재 `main` 브랜치를 GitHub에 올리고 이후 기본 업로드 대상으로 기억하게 합니다.
