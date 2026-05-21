# Day 03

## 오늘의 주제

Pull Request를 만들어서 브랜치 작업을 `main`에 합치는 흐름을 연습한다.

## 오늘의 목표

- Pull Request가 왜 필요한지 이해한다.
- 명령어로 Pull Request를 만든다.
- 명령어로 변경된 파일 내용을 확인한다.
- 명령어로 Pull Request를 merge한다.
- 로컬 `main` 브랜치를 최신 상태로 업데이트한다.
- 작업이 끝난 브랜치를 정리한다.

## 0. 먼저 현재 상태 확인하기

터미널에서 프로젝트 폴더로 이동한다.

```bash
cd /Users/buzz/Desktop/github-study-lab
git status
```

현재 폴더가 Git 저장소가 아니라는 오류가 나오면, 먼저 실제 저장소 폴더로 이동하거나 GitHub에서 다시 clone한다.

```bash
cd /Users/buzz/Desktop
git clone https://github.com/hyeongyu-data/github-study-lab.git
cd github-study-lab
```

## 1. 2일차 브랜치가 있는지 확인하기

로컬 브랜치 목록을 확인한다.

```bash
git branch
```

원격 브랜치까지 함께 확인한다.

```bash
git branch -a
```

`day-02-branch-practice` 브랜치가 보이면 2일차 실습이 잘 이어지고 있는 것이다.

## 2. 브랜치가 GitHub에 올라갔는지 확인하기

아래 명령어로 원격 저장소 주소를 확인한다.

```bash
git remote -v
```

원격 브랜치를 최신 상태로 받아온다.

```bash
git fetch origin
git branch -r
```

확인할 것:

- `origin/day-02-branch-practice` 브랜치가 있는가?
- `origin/main` 브랜치가 있는가?

## 3. GitHub CLI 로그인 상태 확인하기

Pull Request를 명령어로 만들려면 GitHub CLI인 `gh`가 필요하다.

```bash
gh auth status
```

로그인이 안 되어 있다면 아래 명령어로 로그인한다.

```bash
gh auth login
```

## 4. Pull Request 만들기

아래 명령어로 `day-02-branch-practice` 브랜치에서 `main` 브랜치로 들어가는 Pull Request를 만든다.

```bash
gh pr create --base main --head day-02-branch-practice --title "Add day 02 branch practice note" --body "Day 02 브랜치 실습 노트를 추가했습니다."
```

PR이 만들어졌는지 확인한다.

```bash
gh pr list
```

PR 번호를 포함해서 자세히 확인한다.

```bash
gh pr view
```

## 5. Pull Request 변경사항 확인하기

PR에 들어간 파일 목록을 확인한다.

```bash
gh pr diff --name-only
```

PR의 실제 변경 내용을 확인한다.

```bash
gh pr diff
```

확인할 것:

- `notes/day-02.md` 파일이 포함되어 있는가?
- 내가 의도한 내용만 들어갔는가?
- 불필요한 파일이 같이 들어가지 않았는가?

PR은 바로 합치기 전에 변경사항을 검토하는 공간이다. 혼자 공부할 때도 diff를 보는 습관을 들이면 좋다.

## 6. Pull Request merge하기

문제가 없다면 명령어로 PR을 merge한다.

```bash
gh pr merge --merge --delete-branch
```

이 명령어는 현재 브랜치와 연결된 PR을 merge하고, merge가 끝난 원격 브랜치를 삭제한다.

만약 현재 브랜치와 연결된 PR을 찾지 못하면, PR 번호를 확인한 뒤 번호를 넣어 merge한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

예시:

```bash
gh pr merge 1 --merge --delete-branch
```

## 7. 로컬 main 브랜치 최신화하기

GitHub에서 merge가 끝났다면, 로컬 터미널에서 `main` 브랜치를 최신 상태로 만든다.

```bash
git switch main
git pull
```

확인한다.

```bash
git log --oneline
ls notes
```

`notes/day-02.md` 파일이 `main` 브랜치에서도 보이면 성공이다.

## 8. 로컬 브랜치 정리하기

원격 브랜치는 `gh pr merge --delete-branch`로 삭제되었을 수 있다. 로컬 브랜치가 남아 있다면 삭제한다.

```bash
git branch
git branch -d day-02-branch-practice
```

이미 삭제되어 있다면 아래처럼 나올 수 있다.

```bash
error: branch 'day-02-branch-practice' not found
```

이 메시지는 이미 삭제되었다는 뜻이므로 괜찮다.

원격 브랜치 목록도 정리해서 확인한다.

```bash
git fetch --prune
git branch -a
```

## 9. 3일차 파일 커밋하기

오늘 만든 `notes/day-03.md`도 커밋한다.

```bash
git status
git add notes/day-03.md
git commit -m "Add day 03 pull request note"
git push
```

## 오늘 꼭 기억할 개념

- Pull Request는 브랜치의 변경사항을 `main`에 합치기 전에 확인하고 논의하는 공간이다.
- `base`는 변경사항이 들어갈 대상 브랜치다.
- `head`는 내가 작업한 브랜치다.
- merge는 브랜치의 변경사항을 다른 브랜치에 합치는 작업이다.
- GitHub에서 merge한 뒤에는 로컬에서도 `git pull`로 최신 상태를 받아와야 한다.
- `gh`는 GitHub 작업을 터미널 명령어로 실행하게 해주는 GitHub CLI다.

## 오늘 꼭 기억할 명령어

```bash
gh auth status
gh pr create --base main --head 브랜치명 --title "제목" --body "설명"
gh pr list
gh pr view
gh pr diff
gh pr diff --name-only
gh pr merge --merge --delete-branch
git switch main
git pull
git fetch --prune
git branch -a
```

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- GitHub 원격 저장소에서 clone, pull, push 흐름을 더 익힌다.
- 로컬과 GitHub의 상태가 다를 때 어떻게 맞추는지 연습한다.
