# Day 05

## 오늘의 주제

GitHub Issue를 명령어로 만들고, 브랜치와 Pull Request에 연결하는 흐름을 연습한다.

## 오늘의 목표

- Issue가 무엇인지 이해한다.
- `gh issue` 명령어로 Issue를 만든다.
- Issue 번호를 작업 브랜치, 커밋, PR에 연결한다.
- PR merge로 Issue가 자동 종료되는 흐름을 확인한다.
- 명령어만으로 Issue 상태를 조회한다.

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

## 1. GitHub CLI 로그인 상태 확인하기

Issue를 명령어로 다루려면 `gh` 로그인이 필요하다.

```bash
gh auth status
```

로그인이 안 되어 있다면 아래 명령어로 로그인한다.

```bash
gh auth login
```

## 2. 현재 Issue 목록 확인하기

열려 있는 Issue 목록을 확인한다.

```bash
gh issue list
```

열린 Issue와 닫힌 Issue를 모두 보고 싶다면 아래 명령어를 사용한다.

```bash
gh issue list --state all
```

## 3. 5일차 작업 Issue 만들기

아래 명령어로 5일차 실습용 Issue를 만든다.

```bash
gh issue create --title "Add day 05 issue workflow note" --body "Day 05에서 GitHub Issue를 명령어로 만들고 PR에 연결하는 흐름을 연습합니다."
```

생성된 Issue 목록을 다시 확인한다.

```bash
gh issue list
```

Issue 번호를 확인한다. 예를 들어 `#3`처럼 보이면 Issue 번호는 `3`이다.

## 4. Issue 자세히 보기

아래 명령어에서 `이슈번호`는 방금 만든 Issue 번호로 바꾼다.

```bash
gh issue view 이슈번호
```

예시:

```bash
gh issue view 3
```

확인할 것:

- Issue 제목이 맞는가?
- Issue 본문이 맞는가?
- Issue 상태가 `OPEN`인가?

## 5. Issue 작업용 브랜치 만들기

Issue 번호가 `3`이라면 브랜치 이름에 번호를 넣어 작업 맥락을 남긴다.

```bash
git switch -c issue-3-day-05-note
```

번호가 다르다면 브랜치 이름도 자신의 Issue 번호에 맞춘다.

```bash
git switch -c issue-이슈번호-day-05-note
```

현재 브랜치를 확인한다.

```bash
git branch
```

## 6. 5일차 노트 커밋하기

오늘 만든 `notes/day-05.md` 파일을 커밋한다.

```bash
git status
git add notes/day-05.md
git commit -m "5일차 이슈 작업 흐름 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 7. 브랜치를 GitHub에 push하기

아래 명령어에서 브랜치 이름은 자신이 만든 브랜치 이름으로 맞춘다.

```bash
git push -u origin issue-3-day-05-note
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git push -u origin issue-이슈번호-day-05-note
```

원격 브랜치를 확인한다.

```bash
git branch -a
git branch -vv
```

## 8. Issue를 닫는 Pull Request 만들기

PR 본문에 `Closes #이슈번호`를 넣으면 PR이 merge될 때 해당 Issue가 자동으로 닫힌다.

Issue 번호가 `3`이라면 아래처럼 실행한다.

```bash
gh pr create --base main --head issue-3-day-05-note --title "Add day 05 issue workflow note" --body "Closes #3"
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
gh pr create --base main --head issue-이슈번호-day-05-note --title "Add day 05 issue workflow note" --body "Closes #이슈번호"
```

PR 상태를 확인한다.

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

확인할 것:

- `notes/day-05.md`만 추가되었는가?
- PR 본문에 `Closes #이슈번호`가 들어갔는가?

## 9. PR merge하기

변경 내용이 맞다면 PR을 merge하고 원격 브랜치도 삭제한다.

```bash
gh pr merge --merge --delete-branch
```

현재 PR을 찾지 못하면 PR 번호를 확인한 뒤 merge한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

## 10. main 최신화하기

PR merge 후 로컬 `main`을 최신 상태로 맞춘다.

```bash
git switch main
git pull origin main
git fetch --prune origin
```

확인한다.

```bash
git status
ls notes
git log --oneline --decorate --all -10
```

`notes/day-05.md`가 보이면 성공이다.

## 11. Issue가 자동으로 닫혔는지 확인하기

Issue 번호를 넣어서 상태를 확인한다.

```bash
gh issue view 이슈번호
```

닫힌 Issue 목록도 확인한다.

```bash
gh issue list --state closed
```

확인할 것:

- 방금 만든 Issue가 `CLOSED` 상태인가?
- PR merge 때문에 자동으로 닫혔는가?

## 12. Issue에 댓글 남기기

닫힌 Issue에도 댓글을 남길 수 있다.

```bash
gh issue comment 이슈번호 --body "Day 05 실습 완료: Issue 생성, PR 연결, merge 후 자동 종료까지 확인했습니다."
```

댓글이 들어갔는지 확인한다.

```bash
gh issue view 이슈번호 --comments
```

## 오늘 꼭 기억할 개념

- Issue는 해야 할 일, 버그, 아이디어를 기록하는 작업 단위다.
- Issue 번호는 `#3`처럼 표시된다.
- 브랜치 이름에 Issue 번호를 넣으면 작업 맥락을 추적하기 쉽다.
- PR 본문에 `Closes #이슈번호`를 쓰면 merge될 때 Issue가 자동으로 닫힌다.
- `gh issue` 명령어로 Issue 생성, 조회, 댓글 작성을 할 수 있다.

## 오늘 꼭 기억할 명령어

```bash
gh issue list
gh issue list --state all
gh issue create --title "제목" --body "내용"
gh issue view 이슈번호
gh issue view 이슈번호 --comments
gh issue comment 이슈번호 --body "댓글"
git switch -c issue-이슈번호-작업이름
gh pr create --base main --head 브랜치명 --title "제목" --body "Closes #이슈번호"
gh pr merge --merge --delete-branch
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `5일차 이슈 작업 흐름 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- GitHub Project 또는 Milestone으로 여러 Issue를 묶어서 관리하는 흐름을 익힌다.
- 작업 우선순위와 상태를 GitHub에서 관리하는 방법을 연습한다.
