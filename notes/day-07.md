# Day 07

## 오늘의 주제

GitHub Actions로 PR과 push 때 자동 검사를 실행하는 흐름을 연습한다.

## 오늘의 목표

- GitHub Actions가 무엇인지 이해한다.
- `.github/workflows` 폴더의 역할을 이해한다.
- 명령어로 workflow 파일을 만든다.
- PR을 만들고 Actions 실행 결과를 확인한다.
- 실패하거나 성공한 workflow 로그를 명령어로 조회한다.

## 0. 현재 상태 확인하기

프로젝트 폴더에서 시작한다.

```bash
cd /Users/buzz/Desktop/github-study-lab
git status
git branch
git pull origin main
```

확인할 것:

- 현재 브랜치가 `main`인가?
- 작업 중인 변경사항이 있는가?
- `main`이 최신 상태인가?

## 1. GitHub CLI 인증 확인하기

```bash
gh auth status
```

인증이 풀려 있다면 다시 로그인한다.

```bash
gh auth login -h github.com
```

## 2. Actions 상태 확인하기

현재 저장소의 workflow 목록을 확인한다.

```bash
gh workflow list
```

아직 workflow가 없다면 비어 있거나 안내 메시지가 나온다.

최근 Actions 실행 기록을 확인한다.

```bash
gh run list
```

## 3. 7일차 작업 Issue 만들기

```bash
gh issue create --title "7일차 GitHub Actions 기초 학습" --body "GitHub Actions workflow를 만들고 PR에서 자동 검사가 실행되는 흐름을 연습합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 4. 작업 브랜치 만들기

Issue 번호가 `5`라면 아래처럼 만든다.

```bash
git switch -c issue-5-day-07-actions
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-07-actions
```

현재 브랜치를 확인한다.

```bash
git branch
```

## 5. workflow 폴더 만들기

GitHub Actions workflow 파일은 `.github/workflows` 폴더 안에 둔다.

```bash
mkdir -p .github/workflows
```

폴더가 만들어졌는지 확인한다.

```bash
ls -la .github
ls -la .github/workflows
```

## 6. 첫 workflow 파일 만들기

아래 명령어로 `study-check.yml` 파일을 만든다.

```bash
cat > .github/workflows/study-check.yml <<'YAML'
name: Study Check

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  check-notes:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: List study notes
        run: ls -la notes

      - name: Check day 07 note exists
        run: test -f notes/day-07.md
YAML
```

파일 내용을 확인한다.

```bash
sed -n '1,120p' .github/workflows/study-check.yml
```

## 7. 7일차 노트와 workflow 커밋하기

```bash
git status
git add notes/day-07.md .github/workflows/study-check.yml README.md
git commit -m "7일차 깃허브 액션 학습 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 8. 브랜치를 GitHub에 push하기

Issue 번호가 `5`라면 아래처럼 실행한다.

```bash
git push -u origin issue-5-day-07-actions
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git push -u origin issue-이슈번호-day-07-actions
```

## 9. PR 만들기

Issue 번호가 `5`라면 아래처럼 실행한다.

```bash
gh pr create --base main --head issue-5-day-07-actions --title "7일차 깃허브 액션 학습 노트 추가" --body "Closes #5"
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
gh pr create --base main --head issue-이슈번호-day-07-actions --title "7일차 깃허브 액션 학습 노트 추가" --body "Closes #이슈번호"
```

PR을 확인한다.

```bash
gh pr view
gh pr diff --name-only
```

확인할 것:

- `notes/day-07.md`가 포함되어 있는가?
- `.github/workflows/study-check.yml`이 포함되어 있는가?
- `README.md`가 포함되어 있는가?

## 10. Actions 실행 확인하기

PR을 만들면 `pull_request` 이벤트로 workflow가 실행된다.

```bash
gh run list
```

가장 최근 실행을 자세히 확인한다.

```bash
gh run view
```

실행이 진행 중이면 완료될 때까지 기다린다.

```bash
gh run watch
```

실패했을 때는 로그를 확인한다.

```bash
gh run view --log
```

## 11. PR merge하기

Actions가 성공하면 PR을 merge한다.

```bash
gh pr merge --merge --delete-branch
```

현재 PR을 찾지 못하면 PR 번호를 확인한 뒤 merge한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

## 12. main 최신화와 Actions 재확인

```bash
git switch main
git pull origin main
git fetch --prune origin
git status
ls notes
```

`push` 이벤트로 `main`에서도 workflow가 다시 실행될 수 있다.

```bash
gh run list
gh run view
```

## 13. Issue 종료 확인하기

```bash
gh issue view 이슈번호
gh issue list --state closed
```

Issue가 `CLOSED` 상태이면 PR merge로 자동 종료된 것이다.

마무리 댓글을 남긴다.

```bash
gh issue comment 이슈번호 --body "Day 07 실습 완료: GitHub Actions workflow 생성, PR 자동 실행, 로그 확인까지 완료했습니다."
```

## 오늘 꼭 기억할 개념

- GitHub Actions는 GitHub 저장소에서 자동으로 실행되는 작업이다.
- workflow 파일은 `.github/workflows` 폴더 안에 둔다.
- `on`은 workflow가 언제 실행될지 정한다.
- `jobs`는 실행할 작업 묶음이다.
- `steps`는 job 안에서 순서대로 실행되는 명령이다.
- `gh run list`, `gh run view`, `gh run watch`로 Actions 실행 상태를 명령어로 확인할 수 있다.

## 오늘 꼭 기억할 명령어

```bash
gh workflow list
gh run list
gh run view
gh run watch
gh run view --log
mkdir -p .github/workflows
git add notes/day-07.md .github/workflows/study-check.yml README.md
git commit -m "7일차 깃허브 액션 학습 노트 추가"
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `7일차 깃허브 액션 학습 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- Actions workflow를 일부러 실패시킨 뒤 로그를 보고 고치는 흐름을 연습한다.
- PR에서 자동 검사 결과를 보고 merge 여부를 판단하는 연습을 한다.
