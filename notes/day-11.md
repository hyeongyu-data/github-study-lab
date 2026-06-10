# Day 11

## 오늘의 주제

GitHub Actions matrix 전략으로 같은 job을 여러 입력값에 대해 반복 실행하는 흐름을 연습한다.

## 오늘의 목표

- matrix 전략이 무엇인지 이해한다.
- 하나의 job을 여러 입력값으로 반복 실행한다.
- `matrix.note` 값을 사용해 여러 노트 파일을 검사한다.
- `fail-fast: false`가 어떤 역할을 하는지 이해한다.
- PR에서 matrix job들이 어떻게 표시되는지 확인한다.

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

## 2. 11일차 작업 Issue 만들기

```bash
gh issue create --title "11일차 Actions matrix 전략 학습" --body "GitHub Actions matrix 전략으로 같은 job을 여러 입력값에 대해 반복 실행하는 흐름을 연습합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 3. 작업 브랜치 만들기

Issue 번호가 `14`라면 아래처럼 만든다.

```bash
git switch -c issue-14-day-11-actions-matrix
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-11-actions-matrix
```

## 4. 현재 workflow 확인하기

```bash
sed -n '1,260p' .github/workflows/study-check.yml
```

기존 workflow는 `check-notes`, `check-readme`, `summarize-study` job으로 나뉘어 있다.

## 5. matrix 전략 추가하기

`check-notes` job에 matrix를 추가한다.

```yaml
strategy:
  fail-fast: false
  matrix:
    note:
      - day-09.md
      - day-10.md
      - day-11.md
```

검사 명령어는 matrix 값을 사용한다.

```yaml
run: test -f "notes/${{ matrix.note }}"
```

## 6. fail-fast 의미 이해하기

`fail-fast`는 matrix 작업 중 하나가 실패했을 때 나머지 작업을 취소할지 정한다.

- `fail-fast: true`: 하나가 실패하면 진행 중이거나 대기 중인 다른 matrix 작업을 취소할 수 있다.
- `fail-fast: false`: 하나가 실패해도 나머지 matrix 작업을 계속 실행한다.

학습할 때는 실패 결과를 모두 보고 싶으므로 `false`가 더 좋다.

## 7. README 검사도 11일차로 업데이트하기

README가 `day-11.md`를 언급하는지 확인하도록 수정한다.

```yaml
run: grep -q "day-11.md" README.md
```

## 8. 변경사항 커밋하기

```bash
git status
git add notes/day-11.md .github/workflows/study-check.yml README.md
git commit -m "11일차 액션 matrix 전략 학습 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 9. 브랜치를 push하고 PR 만들기

Issue 번호가 `14`라면 아래처럼 실행한다.

```bash
git push -u origin issue-14-day-11-actions-matrix
gh pr create --base main --head issue-14-day-11-actions-matrix --title "11일차 액션 matrix 전략 학습 노트 추가" --body "Closes #14"
```

Issue 번호가 다르다면 브랜치명과 PR 본문의 번호를 바꾼다.

## 10. PR 변경사항 확인하기

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

확인할 것:

- `notes/day-11.md`가 추가되었는가?
- README에 11일차 항목이 추가되었는가?
- workflow에 `strategy.matrix`가 추가되었는가?

## 11. matrix Actions 실행 확인하기

```bash
gh run list --limit 5
```

실행 상세를 확인한다.

```bash
gh run view
```

확인할 것:

- `check-notes`가 matrix 값별로 여러 번 실행되는가?
- `day-09.md`, `day-10.md`, `day-11.md` 검사가 각각 성공하는가?
- `check-readme`가 성공하는가?
- `summarize-study`가 마지막에 성공하는가?

진행 중이면 기다린다.

```bash
gh run watch
```

실패하면 로그를 확인한다.

```bash
gh run view --log
```

## 12. PR merge하기

모든 job이 성공하면 merge한다.

```bash
gh pr merge --merge --delete-branch
```

현재 PR을 찾지 못하면 PR 번호를 확인한 뒤 merge한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

## 13. main 최신화와 정리

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

## 14. Issue 종료 확인하기

```bash
gh issue view 이슈번호
gh issue list --state closed
```

마무리 댓글을 남긴다.

```bash
gh issue comment 이슈번호 --body "Day 11 실습 완료: matrix 전략으로 여러 노트 검사를 반복 실행하고 결과를 확인했습니다."
```

## 오늘 꼭 기억할 개념

- matrix는 하나의 job을 여러 입력값으로 반복 실행하는 방법이다.
- `matrix.note`처럼 matrix 값을 workflow 안에서 사용할 수 있다.
- matrix job은 PR 상태에서 여러 실행 항목으로 보인다.
- `fail-fast: false`를 쓰면 하나가 실패해도 다른 matrix 실행 결과까지 확인할 수 있다.
- 반복되는 검사를 matrix로 만들면 workflow 중복을 줄일 수 있다.

## 오늘 꼭 기억할 명령어

```bash
gh run list --limit 5
gh run view
gh run view --log
gh run watch
git add notes/day-11.md .github/workflows/study-check.yml README.md
git commit -m "11일차 액션 matrix 전략 학습 노트 추가"
git push
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `11일차 액션 matrix 전략 학습 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- matrix 값을 일부러 하나 실패하게 만들어 `fail-fast: false`의 효과를 확인한다.
- matrix에 여러 축을 추가해서 조합이 어떻게 늘어나는지 실험한다.
