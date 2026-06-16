# Day 13

## 오늘의 주제

GitHub Actions matrix에서 `exclude`와 `include`로 실행 조합을 조절하는 흐름을 연습한다.

## 오늘의 목표

- matrix 조합이 자동으로 만들어지는 방식을 복습한다.
- `exclude`로 특정 조합을 제외한다.
- `include`로 기본 조합에 없는 특수 조합을 추가한다.
- PR에서 제외된 조합과 추가된 조합이 어떻게 보이는지 확인한다.
- matrix 조합을 의도적으로 설계하는 감각을 익힌다.

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

## 2. 13일차 작업 Issue 만들기

```bash
gh issue create --title "13일차 Actions matrix include exclude 학습" --body "GitHub Actions matrix에서 exclude로 조합을 제외하고 include로 특수 조합을 추가하는 흐름을 연습합니다." --label "학습"
```

Issue 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 3. 작업 브랜치 만들기

Issue 번호가 `18`이라면 아래처럼 만든다.

```bash
git switch -c issue-18-day-13-actions-matrix-include-exclude
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-13-actions-matrix-include-exclude
```

## 4. 현재 matrix 확인하기

```bash
sed -n '1,320p' .github/workflows/study-check.yml
```

기존 workflow는 `note`와 `check` 두 축을 조합해서 matrix를 만들었다.

## 5. exclude로 조합 제외하기

아래 조합을 제외한다.

```yaml
exclude:
  - note: day-11.md
    check: has-title
```

의미:

- 기본 조합에는 `day-11.md + has-title`이 포함된다.
- 하지만 `exclude`가 있으므로 해당 조합은 실행되지 않는다.

## 6. include로 특수 조합 추가하기

기본 `check` 축에는 없던 `mentions-actions` 검사를 추가한다.

```yaml
include:
  - note: day-13.md
    check: mentions-actions
```

이 조합은 기본 matrix 조합과 별개로 추가된다.

## 7. 새 check 처리 추가하기

`mentions-actions`는 노트 안에 `Actions`라는 단어가 있는지 검사한다.

```bash
grep -q "Actions" "notes/${{ matrix.note }}"
```

workflow의 `case` 문에는 아래 분기를 추가한다.

```yaml
mentions-actions)
  grep -q "Actions" "notes/${{ matrix.note }}"
  ;;
```

## 8. 예상 조합 계산하기

기본 조합:

- note 3개: `day-11.md`, `day-12.md`, `day-13.md`
- check 2개: `exists`, `has-title`
- 기본 조합 수: `3 x 2 = 6`

조정:

- `exclude`로 1개 제거
- `include`로 1개 추가
- 최종 조합 수: `6 - 1 + 1 = 6`

## 9. 변경사항 커밋하기

```bash
git status
git add notes/day-13.md .github/workflows/study-check.yml README.md
git commit -m "13일차 액션 matrix include exclude 학습 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 10. 브랜치를 push하고 PR 만들기

Issue 번호가 `18`이라면 아래처럼 실행한다.

```bash
git push -u origin issue-18-day-13-actions-matrix-include-exclude
gh pr create --base main --head issue-18-day-13-actions-matrix-include-exclude --title "13일차 액션 matrix include exclude 학습 노트 추가" --body "Closes #18"
```

Issue 번호가 다르다면 브랜치명과 PR 본문의 번호를 바꾼다.

## 11. PR 변경사항 확인하기

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

확인할 것:

- `notes/day-13.md`가 추가되었는가?
- README에 13일차 항목이 추가되었는가?
- workflow에 `exclude`와 `include`가 추가되었는가?

## 12. matrix 실행 조합 확인하기

```bash
gh run list --limit 5
```

실행 상세를 확인한다.

```bash
gh run view
```

확인할 것:

- `day-11.md, has-title` 조합이 빠졌는가?
- `day-13.md, mentions-actions` 조합이 추가되었는가?
- 전체 matrix 조합이 성공했는가?
- `summarize-study`가 마지막에 성공했는가?

진행 중이면 기다린다.

```bash
gh run watch
```

실패하면 로그를 확인한다.

```bash
gh run view --log
```

## 13. PR merge하기

모든 job이 성공하면 merge한다.

```bash
gh pr merge --merge --delete-branch
```

현재 PR을 찾지 못하면 PR 번호를 확인한 뒤 merge한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

## 14. main 최신화와 정리

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

## 15. Issue 종료 확인하기

```bash
gh issue view 이슈번호
gh issue list --state closed
```

마무리 댓글을 남긴다.

```bash
gh issue comment 이슈번호 --body "Day 13 실습 완료: matrix exclude와 include로 실행 조합을 조절했습니다."
```

## 오늘 꼭 기억할 개념

- `exclude`는 기본 matrix 조합 중 특정 조합을 실행하지 않게 한다.
- `include`는 기본 matrix 조합에 없는 조합을 추가한다.
- matrix 조합 수는 축의 곱으로 계산한 뒤 include/exclude로 조정된다.
- 특수 check를 추가할 때는 workflow의 처리 로직도 함께 추가해야 한다.
- PR에서 실제 job 목록을 확인하면 matrix 설계가 맞는지 검증할 수 있다.

## 오늘 꼭 기억할 명령어

```bash
gh run list --limit 5
gh run view
gh run view --log
gh run watch
git add notes/day-13.md .github/workflows/study-check.yml README.md
git commit -m "13일차 액션 matrix include exclude 학습 노트 추가"
git push
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `13일차 액션 matrix include exclude 학습 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- Actions에서 artifact를 업로드하고 다운로드하는 흐름을 연습한다.
- 자동 검사 결과물을 파일로 남기는 방법을 익힌다.
