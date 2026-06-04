# Day 08

## 오늘의 주제

GitHub Actions 실행 로그를 확인하고 workflow 경고를 줄이는 개선 흐름을 연습한다.

## 오늘의 목표

- `gh run` 명령어로 Actions 실행 기록을 확인한다.
- Actions 실행 상세와 로그를 읽는다.
- workflow에 `workflow_dispatch`를 추가해서 수동 실행을 가능하게 만든다.
- Node.js 20 deprecation 경고를 줄이기 위해 Node 24 opt-in 환경변수를 설정한다.
- workflow 수정 후 PR에서 자동 검사가 성공하는지 확인한다.

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
- 작업 중인 변경사항이 없는가?
- `main`이 최신 상태인가?

## 1. GitHub CLI 인증 확인하기

```bash
gh auth status
```

인증이 풀려 있다면 다시 로그인한다.

```bash
gh auth login -h github.com
```

## 2. 최근 Actions 실행 기록 보기

```bash
gh run list --limit 5
```

가장 최근 실행을 자세히 본다.

```bash
gh run view
```

로그까지 확인한다.

```bash
gh run view --log
```

확인할 것:

- workflow 이름은 무엇인가?
- 실행 이벤트가 `pull_request`인가, `push`인가?
- 성공했는가, 실패했는가?
- 경고나 annotation이 있는가?

## 3. 8일차 작업 Issue 만들기

```bash
gh issue create --title "8일차 GitHub Actions 로그와 워크플로 개선" --body "GitHub Actions 실행 로그를 확인하고 Node.js 20 deprecation 경고를 줄이도록 workflow를 개선합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 4. 작업 브랜치 만들기

Issue 번호가 `8`이라면 아래처럼 만든다.

```bash
git switch -c issue-8-day-08-actions-log
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-08-actions-log
```

## 5. workflow 파일 확인하기

```bash
sed -n '1,160p' .github/workflows/study-check.yml
```

현재 workflow는 PR과 main push 때 실행된다.

## 6. workflow 개선하기

이번 실습에서는 workflow에 세 가지를 반영한다.

- `workflow_dispatch`: 명령어로 수동 실행 가능하게 만들기
- `FORCE_JAVASCRIPT_ACTIONS_TO_NODE24`: Node.js 24 opt-in
- `notes/day-08.md` 존재 여부 확인

수정 후 확인한다.

```bash
sed -n '1,180p' .github/workflows/study-check.yml
```

예상 핵심 내용:

```yaml
on:
  workflow_dispatch:
  pull_request:
  push:
    branches:
      - main

env:
  FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: "true"
```

## 7. 8일차 변경사항 커밋하기

```bash
git status
git add notes/day-08.md .github/workflows/study-check.yml README.md
git commit -m "8일차 액션 로그와 워크플로 개선 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 8. 브랜치를 GitHub에 push하기

Issue 번호가 `8`이라면 아래처럼 실행한다.

```bash
git push -u origin issue-8-day-08-actions-log
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git push -u origin issue-이슈번호-day-08-actions-log
```

## 9. PR 만들기

Issue 번호가 `8`이라면 아래처럼 실행한다.

```bash
gh pr create --base main --head issue-8-day-08-actions-log --title "8일차 액션 로그와 워크플로 개선 노트 추가" --body "Closes #8"
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
gh pr create --base main --head issue-이슈번호-day-08-actions-log --title "8일차 액션 로그와 워크플로 개선 노트 추가" --body "Closes #이슈번호"
```

PR 변경사항을 확인한다.

```bash
gh pr view
gh pr diff --name-only
```

## 10. PR Actions 실행 확인하기

```bash
gh run list --limit 5
```

실행이 진행 중이면 기다린다.

```bash
gh run watch
```

실행 상세를 확인한다.

```bash
gh run view
```

로그도 확인한다.

```bash
gh run view --log
```

확인할 것:

- `Study Check`가 성공했는가?
- `Check day 08 note exists` 단계가 통과했는가?
- Node.js 20 deprecation 경고가 줄었는가?

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

## 12. main 최신화와 push Actions 확인하기

```bash
git switch main
git pull origin main
git fetch --prune origin
git status
```

main push 이벤트로 실행된 workflow를 확인한다.

```bash
gh run list --limit 5
gh run view
```

## 13. workflow 수동 실행하기

`workflow_dispatch`를 추가했기 때문에 명령어로 직접 실행할 수 있다.

```bash
gh workflow run "Study Check" --ref main
```

실행 목록을 확인한다.

```bash
gh run list --limit 5
gh run watch
```

## 14. Issue 종료 확인하기

```bash
gh issue view 이슈번호
gh issue list --state closed
```

마무리 댓글을 남긴다.

```bash
gh issue comment 이슈번호 --body "Day 08 실습 완료: Actions 로그 확인, workflow_dispatch 추가, Node 24 opt-in 설정까지 완료했습니다."
```

## 오늘 꼭 기억할 개념

- Actions 로그는 자동화가 왜 성공하거나 실패했는지 보여준다.
- `workflow_dispatch`는 workflow를 수동 실행할 수 있게 하는 이벤트다.
- `gh run view --log`로 실패 원인과 경고를 자세히 볼 수 있다.
- workflow 파일을 고치면 PR에서 자동 검사를 통해 변경이 안전한지 확인할 수 있다.
- 경고는 실패가 아니어도 나중에 문제가 될 수 있으므로 기록하고 개선한다.

## 오늘 꼭 기억할 명령어

```bash
gh run list --limit 5
gh run view
gh run view --log
gh run watch
gh workflow run "Study Check" --ref main
git add notes/day-08.md .github/workflows/study-check.yml README.md
git commit -m "8일차 액션 로그와 워크플로 개선 노트 추가"
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `8일차 액션 로그와 워크플로 개선 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- Actions workflow를 일부러 실패시킨 뒤 로그를 보고 수정하는 흐름을 연습한다.
- PR 자동 검사 실패를 merge 전에 발견하고 고치는 흐름을 익힌다.
