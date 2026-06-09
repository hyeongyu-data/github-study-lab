# Day 10

## 오늘의 주제

GitHub Actions workflow에 여러 job을 추가하고 `needs`로 실행 순서를 제어하는 흐름을 연습한다.

## 오늘의 목표

- Actions job이 무엇인지 이해한다.
- 하나의 workflow에 여러 job을 만든다.
- 서로 독립적인 job과 순서가 필요한 job을 구분한다.
- `needs`로 job 실행 순서를 제어한다.
- PR에서 여러 job이 모두 성공해야 안전하게 merge할 수 있음을 확인한다.

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

## 2. 10일차 작업 Issue 만들기

```bash
gh issue create --title "10일차 Actions 여러 job과 needs 학습" --body "GitHub Actions workflow에 여러 job을 추가하고 needs로 실행 순서를 제어하는 흐름을 연습합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 3. 작업 브랜치 만들기

Issue 번호가 `12`라면 아래처럼 만든다.

```bash
git switch -c issue-12-day-10-actions-needs
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-10-actions-needs
```

## 4. 현재 workflow 확인하기

```bash
sed -n '1,220p' .github/workflows/study-check.yml
```

기존 workflow는 하나의 job에서 노트 파일을 확인했다.

## 5. 여러 job으로 workflow 나누기

이번 실습에서는 workflow를 세 개의 job으로 나눈다.

- `check-notes`: `notes/day-10.md`가 있는지 확인한다.
- `check-readme`: README가 있고 `day-10.md`를 언급하는지 확인한다.
- `summarize-study`: 앞의 두 job이 모두 성공한 뒤 실행된다.

핵심 구조:

```yaml
jobs:
  check-notes:
    runs-on: ubuntu-latest

  check-readme:
    runs-on: ubuntu-latest

  summarize-study:
    runs-on: ubuntu-latest
    needs:
      - check-notes
      - check-readme
```

## 6. needs 의미 이해하기

`needs`는 어떤 job이 먼저 성공해야 현재 job이 실행될 수 있는지 정한다.

이번 workflow에서는:

- `check-notes`와 `check-readme`는 서로 독립적으로 실행될 수 있다.
- `summarize-study`는 두 job이 모두 성공해야 실행된다.
- 둘 중 하나라도 실패하면 `summarize-study`는 실행되지 않는다.

## 7. 변경사항 커밋하기

```bash
git status
git add notes/day-10.md .github/workflows/study-check.yml README.md
git commit -m "10일차 액션 여러 job 학습 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 8. 브랜치를 push하고 PR 만들기

Issue 번호가 `12`라면 아래처럼 실행한다.

```bash
git push -u origin issue-12-day-10-actions-needs
gh pr create --base main --head issue-12-day-10-actions-needs --title "10일차 액션 여러 job 학습 노트 추가" --body "Closes #12"
```

Issue 번호가 다르다면 브랜치명과 PR 본문의 번호를 바꾼다.

## 9. PR 변경사항 확인하기

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

확인할 것:

- `notes/day-10.md`가 추가되었는가?
- README에 10일차 항목이 추가되었는가?
- workflow에 여러 job과 `needs`가 추가되었는가?

## 10. Actions 여러 job 실행 확인하기

```bash
gh run list --limit 5
```

실행 상세를 확인한다.

```bash
gh run view
```

확인할 것:

- `check-notes` job이 성공했는가?
- `check-readme` job이 성공했는가?
- `summarize-study` job이 두 job 뒤에 성공했는가?

진행 중이면 기다린다.

```bash
gh run watch
```

실패하면 로그를 확인한다.

```bash
gh run view --log
```

## 11. PR merge하기

모든 job이 성공하면 merge한다.

```bash
gh pr merge --merge --delete-branch
```

현재 PR을 찾지 못하면 PR 번호를 확인한 뒤 merge한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

## 12. main 최신화와 정리

```bash
git switch main
git pull origin main
git fetch --prune origin
git status
ls notes
```

main push 이벤트로 실행된 workflow도 확인한다.

```bash
gh run list --limit 5
gh run view
```

## 13. Issue 종료 확인하기

```bash
gh issue view 이슈번호
gh issue list --state closed
```

마무리 댓글을 남긴다.

```bash
gh issue comment 이슈번호 --body "Day 10 실습 완료: 여러 job과 needs 실행 순서 확인까지 완료했습니다."
```

## 오늘 꼭 기억할 개념

- workflow는 하나 이상의 job으로 구성된다.
- job은 기본적으로 서로 독립적으로 실행될 수 있다.
- `needs`를 쓰면 특정 job이 끝난 뒤 다음 job이 실행되도록 만들 수 있다.
- 여러 job 중 하나라도 실패하면 전체 workflow는 실패로 판단된다.
- PR에서는 여러 job 상태를 보고 merge해도 되는지 판단한다.

## 오늘 꼭 기억할 명령어

```bash
gh run list --limit 5
gh run view
gh run view --log
gh run watch
git add notes/day-10.md .github/workflows/study-check.yml README.md
git commit -m "10일차 액션 여러 job 학습 노트 추가"
git push
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `10일차 액션 여러 job 학습 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- workflow job 중 하나만 실패하게 만들고, 다른 job과 dependent job이 어떻게 표시되는지 확인한다.
- matrix 전략으로 여러 환경에서 같은 job을 반복 실행하는 방법을 익힌다.
