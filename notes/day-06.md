# Day 06

## 오늘의 주제

GitHub Issue를 라벨과 마일스톤으로 분류하고 관리하는 흐름을 연습한다.

## 오늘의 목표

- 라벨이 무엇인지 이해한다.
- 마일스톤이 무엇인지 이해한다.
- `gh label` 명령어로 라벨을 조회하고 만든다.
- `gh api`와 `gh issue edit`으로 마일스톤을 만들고 Issue에 연결한다.
- 라벨과 마일스톤이 붙은 Issue를 PR로 닫는 흐름을 복습한다.

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
- `main`이 원격 저장소와 같은 최신 상태인가?

## 1. GitHub CLI 인증 확인하기

```bash
gh auth status
```

인증이 풀려 있다면 다시 로그인한다.

```bash
gh auth login -h github.com
```

## 2. 현재 라벨 목록 확인하기

```bash
gh label list
```

라벨은 Issue와 PR을 분류하기 위한 이름표다.

예시:

- `bug`: 버그
- `documentation`: 문서 작업
- `enhancement`: 기능 개선
- `학습`: 학습용 작업

## 3. 학습용 라벨 만들기

`학습` 라벨을 만든다.

```bash
gh label create "학습" --color "2EA043" --description "GitHub 학습용 작업"
```

이미 같은 이름의 라벨이 있어서 실패하면 새로 만들 필요는 없다. 대신 아래 명령어로 기존 라벨을 확인한다.

```bash
gh label list
```

라벨 설명이나 색을 바꾸고 싶다면 아래 명령어를 사용한다.

```bash
gh label edit "학습" --color "2EA043" --description "GitHub 학습용 작업"
```

## 4. 마일스톤 목록 확인하기

`gh`에는 기본 `milestone` 명령어가 없으므로 GitHub API를 사용한다.

```bash
gh api repos/:owner/:repo/milestones
```

보기 쉽게 제목과 번호만 확인한다.

```bash
gh api repos/:owner/:repo/milestones --jq '.[] | {number, title, state}'
```

마일스톤은 여러 Issue와 PR을 하나의 목표나 기간으로 묶는 단위다.

## 5. 1주차 학습 마일스톤 만들기

아래 명령어로 `GitHub Study Week 1` 마일스톤을 만든다.

```bash
gh api --method POST repos/:owner/:repo/milestones -f title="GitHub Study Week 1" -f description="GitHub 기초 학습 1주차"
```

이미 같은 이름의 마일스톤이 있다면 새로 만들 필요는 없다. 아래 명령어로 확인한다.

```bash
gh api repos/:owner/:repo/milestones --jq '.[] | {number, title, state}'
```

## 6. 6일차 작업 Issue 만들기

라벨을 붙여서 Issue를 만든다.

```bash
gh issue create --title "6일차 라벨과 마일스톤 학습" --body "GitHub Issue에 라벨과 마일스톤을 붙여 작업을 분류하는 흐름을 연습합니다." --label "학습"
```

Issue 목록에서 번호를 확인한다.

```bash
gh issue list
```

아래부터 `이슈번호`는 방금 만든 Issue 번호로 바꿔서 실행한다.

## 7. Issue에 마일스톤 연결하기

```bash
gh issue edit 이슈번호 --milestone "GitHub Study Week 1"
```

Issue 상태를 확인한다.

```bash
gh issue view 이슈번호
```

확인할 것:

- `학습` 라벨이 붙어 있는가?
- `GitHub Study Week 1` 마일스톤이 연결되어 있는가?
- Issue 상태가 `OPEN`인가?

## 8. 라벨로 Issue 검색하기

```bash
gh issue list --label "학습"
```

마일스톤 기준으로는 API를 사용해서 확인한다.

```bash
gh api repos/:owner/:repo/issues --jq '.[] | select(.milestone.title=="GitHub Study Week 1") | {number, title, state}'
```

## 9. 6일차 작업 브랜치 만들기

Issue 번호가 `4`라면 아래처럼 만든다.

```bash
git switch -c issue-4-day-06-label-milestone
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git switch -c issue-이슈번호-day-06-label-milestone
```

현재 브랜치를 확인한다.

```bash
git branch
```

## 10. 6일차 노트 커밋하기

```bash
git status
git add notes/day-06.md
git commit -m "6일차 라벨과 마일스톤 학습 노트 추가"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 11. 브랜치를 GitHub에 push하기

Issue 번호가 `4`라면 아래처럼 실행한다.

```bash
git push -u origin issue-4-day-06-label-milestone
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
git push -u origin issue-이슈번호-day-06-label-milestone
```

## 12. Issue를 닫는 PR 만들기

Issue 번호가 `4`라면 아래처럼 실행한다.

```bash
gh pr create --base main --head issue-4-day-06-label-milestone --title "6일차 라벨과 마일스톤 학습 노트 추가" --body "Closes #4"
```

Issue 번호가 다르다면 아래처럼 바꾼다.

```bash
gh pr create --base main --head issue-이슈번호-day-06-label-milestone --title "6일차 라벨과 마일스톤 학습 노트 추가" --body "Closes #이슈번호"
```

PR 변경사항을 확인한다.

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

확인할 것:

- `notes/day-06.md`만 추가되었는가?
- PR 본문에 `Closes #이슈번호`가 들어갔는가?

## 13. PR merge하기

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
git branch -a
ls notes
```

`notes/day-06.md`가 보이면 성공이다.

## 15. Issue 종료 확인하기

```bash
gh issue view 이슈번호
gh issue list --state closed
```

Issue가 `CLOSED` 상태이면 PR merge로 자동 종료된 것이다.

마무리 댓글을 남긴다.

```bash
gh issue comment 이슈번호 --body "Day 06 실습 완료: 라벨과 마일스톤을 만들고 Issue에 연결한 뒤 PR merge로 종료까지 확인했습니다."
```

## 오늘 꼭 기억할 개념

- 라벨은 Issue와 PR을 성격별로 분류하는 이름표다.
- 마일스톤은 여러 Issue와 PR을 하나의 목표나 기간으로 묶는 단위다.
- `gh label`로 라벨을 만들고 수정할 수 있다.
- `gh api repos/:owner/:repo/milestones`로 마일스톤을 다룰 수 있다.
- `gh issue edit`으로 기존 Issue에 라벨이나 마일스톤을 붙일 수 있다.

## 오늘 꼭 기억할 명령어

```bash
gh label list
gh label create "학습" --color "2EA043" --description "GitHub 학습용 작업"
gh label edit "학습" --color "2EA043" --description "GitHub 학습용 작업"
gh api repos/:owner/:repo/milestones
gh api --method POST repos/:owner/:repo/milestones -f title="GitHub Study Week 1" -f description="GitHub 기초 학습 1주차"
gh issue create --title "제목" --body "내용" --label "학습"
gh issue edit 이슈번호 --milestone "GitHub Study Week 1"
gh issue list --label "학습"
git commit -m "6일차 라벨과 마일스톤 학습 노트 추가"
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `6일차 라벨과 마일스톤 학습 노트 추가`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- GitHub Actions로 자동 검사를 실행하는 기본 흐름을 익힌다.
- PR이 열릴 때 자동으로 명령어가 실행되는 구조를 배운다.
