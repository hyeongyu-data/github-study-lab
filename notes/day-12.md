# Day 12

## 오늘의 주제

GitHub Actions matrix에 여러 축을 추가해서 job 조합이 늘어나는 흐름을 연습한다.

## 오늘의 목표

- matrix에 여러 축을 넣으면 조합이 어떻게 늘어나는지 이해한다.
- `note` 축과 `check` 축을 함께 사용한다.
- matrix 값에 따라 다른 검사를 실행한다.
- PR에서 여러 matrix 조합이 각각 job으로 표시되는지 확인한다.
- main push에서도 같은 matrix 검사가 성공하는지 확인한다.

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

## 2. 12일차 작업 Issue 만들기

```bash
gh issue create --title "12일차 Actions 다중 축 matrix 학습" --body "GitHub Actions matrix에 note와 check 두 축을 추가해 조합이 늘어나는 흐름을 연습합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 3. 작업 브랜치 만들기

Issue 번호가 `16`이라면 아래처럼 만든다.

```bash
git switch -c issue-16-day-12-actions-matrix-axes
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-12-actions-matrix-axes
```

## 4. 현재 matrix 확인하기

```bash
sed -n '1,260p' .github/workflows/study-check.yml
```

기존 matrix는 `note` 축 하나만 사용했다.

## 5. 다중 축 matrix 만들기

이번 실습에서는 matrix에 두 축을 둔다.

```yaml
matrix:
  note:
    - day-10.md
    - day-11.md
    - day-12.md
  check:
    - exists
    - has-title
```

조합 수는 `3개 note x 2개 check = 6개`다.

## 6. matrix 값으로 다른 검사 실행하기

`matrix.check` 값에 따라 다른 명령어를 실행한다.

```yaml
run: |
  case "${{ matrix.check }}" in
    exists)
      test -f "notes/${{ matrix.note }}"
      ;;
    has-title)
      grep -q "^# Day" "notes/${{ matrix.note }}"
      ;;
    *)
      echo "Unknown check: ${{ matrix.check }}"
      exit 1
      ;;
  esac
```

검사 의미:

- `exists`: 파일이 실제로 있는지 확인한다.
- `has-title`: 파일 안에 `# Day`로 시작하는 제목이 있는지 확인한다.

## 7. README 검사도 12일차로 업데이트하기

README가 `day-12.md`를 언급하는지 확인하도록 수정한다.

```yaml
run: grep -q "day-12.md" README.md
```

## 8. 변경사항 커밋하기

```bash
git status
git add notes/day-12.md .github/workflows/study-check.yml README.md
git commit -m "12일차 액션 다중 matrix 학습 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 9. 브랜치를 push하고 PR 만들기

Issue 번호가 `16`이라면 아래처럼 실행한다.

```bash
git push -u origin issue-16-day-12-actions-matrix-axes
gh pr create --base main --head issue-16-day-12-actions-matrix-axes --title "12일차 액션 다중 matrix 학습 노트 추가" --body "Closes #16"
```

Issue 번호가 다르다면 브랜치명과 PR 본문의 번호를 바꾼다.

## 10. PR 변경사항 확인하기

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

확인할 것:

- `notes/day-12.md`가 추가되었는가?
- README에 12일차 항목이 추가되었는가?
- workflow matrix에 `note`와 `check` 두 축이 있는가?

## 11. 다중 matrix Actions 실행 확인하기

```bash
gh run list --limit 5
```

실행 상세를 확인한다.

```bash
gh run view
```

확인할 것:

- `check-notes`가 6개 조합으로 실행되는가?
- `exists` 검사가 각 노트에서 성공하는가?
- `has-title` 검사가 각 노트에서 성공하는가?
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
gh issue comment 이슈번호 --body "Day 12 실습 완료: note와 check 두 축 matrix로 여러 검사 조합을 실행했습니다."
```

## 오늘 꼭 기억할 개념

- matrix 축이 여러 개면 모든 조합이 job으로 만들어진다.
- `3 x 2` matrix는 6개의 job 조합을 만든다.
- `matrix.note`, `matrix.check`처럼 각 축의 값을 사용할 수 있다.
- shell `case` 문을 쓰면 matrix 값에 따라 다른 검사를 실행할 수 있다.
- 조합이 많아질수록 자동 검사는 강력해지지만 실행 수도 늘어난다.

## 오늘 꼭 기억할 명령어

```bash
gh run list --limit 5
gh run view
gh run view --log
gh run watch
git add notes/day-12.md .github/workflows/study-check.yml README.md
git commit -m "12일차 액션 다중 matrix 학습 노트 추가"
git push
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `12일차 액션 다중 matrix 학습 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- matrix 조합 중 일부를 제외하는 `exclude`를 연습한다.
- 특정 조합만 추가하는 `include`를 연습한다.
