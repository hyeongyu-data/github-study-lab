# Day 09

## 오늘의 주제

GitHub Actions를 일부러 실패시킨 뒤 로그를 보고 원인을 찾아 수정하는 흐름을 연습한다.

## 오늘의 목표

- 실패하는 workflow를 만들고 PR에서 실패 상태를 확인한다.
- `gh run view --log`로 실패 로그를 읽는다.
- 실패 원인을 workflow 파일에서 찾는다.
- workflow를 수정하고 다시 push해서 Actions를 성공시킨다.
- 실패한 자동 검사를 merge 전에 고치는 흐름을 익힌다.

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

## 2. 9일차 작업 Issue 만들기

```bash
gh issue create --title "9일차 Actions 실패 로그 디버깅" --body "GitHub Actions를 일부러 실패시킨 뒤 로그를 확인하고 workflow를 수정해 성공시키는 흐름을 연습합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 3. 작업 브랜치 만들기

Issue 번호가 `10`이라면 아래처럼 만든다.

```bash
git switch -c issue-10-day-09-actions-debug
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-09-actions-debug
```

## 4. 일부러 실패하는 workflow 만들기

이번 실습의 첫 번째 목표는 실패를 직접 보는 것이다.

workflow의 마지막 검사를 일부러 존재하지 않는 파일로 바꾼다.

```yaml
      - name: Check intentionally missing note
        run: test -f notes/day-99.md
```

이 검사는 `notes/day-99.md`가 없기 때문에 실패한다.

## 5. 실패 버전 커밋하기

```bash
git status
git add notes/day-09.md .github/workflows/study-check.yml README.md
git commit -m "9일차 액션 실패 디버깅 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 6. 브랜치를 push하고 PR 만들기

Issue 번호가 `10`이라면 아래처럼 실행한다.

```bash
git push -u origin issue-10-day-09-actions-debug
gh pr create --base main --head issue-10-day-09-actions-debug --title "9일차 액션 실패 디버깅 노트 추가" --body "Closes #10"
```

Issue 번호가 다르다면 브랜치명과 PR 본문의 번호를 바꾼다.

## 7. 실패한 Actions 확인하기

PR을 만들면 Actions가 실행된다.

```bash
gh run list --limit 5
```

실패한 run을 자세히 확인한다.

```bash
gh run view
```

로그를 확인한다.

```bash
gh run view --log
```

확인할 것:

- 어떤 step에서 실패했는가?
- `test -f notes/day-99.md`가 실패 원인인가?
- 실패한 run의 conclusion이 `failure`인가?

## 8. workflow 고치기

실패 원인을 확인했으므로 실제 존재하는 9일차 노트를 검사하도록 수정한다.

```yaml
      - name: Check day 09 note exists
        run: test -f notes/day-09.md
```

수정 후 파일을 확인한다.

```bash
sed -n '1,180p' .github/workflows/study-check.yml
```

## 9. 수정 커밋하기

```bash
git status
git add .github/workflows/study-check.yml notes/day-09.md
git commit -m "액션 실패 검사 경로 수정"
```

다시 push한다.

```bash
git push
```

## 10. 다시 실행된 Actions 확인하기

```bash
gh run list --limit 5
```

진행 중이면 기다린다.

```bash
gh run watch
```

성공한 run을 확인한다.

```bash
gh run view
```

확인할 것:

- 새 run은 `success`인가?
- `Check day 09 note exists` step이 성공했는가?
- PR에서 더 이상 실패 상태가 아닌가?

## 11. PR merge하기

Actions가 성공하면 merge한다.

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
gh issue comment 이슈번호 --body "Day 09 실습 완료: Actions 실패 로그 확인, 원인 수정, 성공 run 확인까지 완료했습니다."
```

## 오늘 꼭 기억할 개념

- CI 실패는 merge 전에 문제를 발견하게 해주는 신호다.
- 실패 로그를 먼저 보고, 그 다음 workflow나 코드의 원인을 찾는다.
- `gh run view --log`는 실패한 step과 명령어 출력을 확인할 때 유용하다.
- 실패를 고친 뒤 다시 push하면 PR에서 Actions가 다시 실행된다.
- 성공한 run을 확인한 뒤 merge하는 습관을 들인다.

## 오늘 꼭 기억할 명령어

```bash
gh run list --limit 5
gh run view
gh run view --log
gh run watch
git add .github/workflows/study-check.yml notes/day-09.md README.md
git commit -m "9일차 액션 실패 디버깅 노트 추가"
git commit -m "액션 실패 검사 경로 수정"
git push
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `9일차 액션 실패 디버깅 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 첫 PR Actions run `26998415953`에서 `test -f notes/day-99.md`가 실패하는 것을 확인했다.
- workflow 검사 경로를 `notes/day-09.md`로 수정해서 다시 실행한다.

## 다음에 해볼 것

- workflow에 여러 job을 추가하고 job 간 실행 순서를 확인한다.
- 실패한 job과 성공한 job이 함께 있을 때 PR 상태가 어떻게 보이는지 확인한다.
