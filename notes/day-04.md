# Day 04

## 오늘의 주제

로컬 저장소와 GitHub 원격 저장소의 상태를 확인하고 동기화하는 흐름을 연습한다.

## 오늘의 목표

- `origin`, 로컬 브랜치, 원격 브랜치의 관계를 이해한다.
- `git fetch`와 `git pull`의 차이를 익힌다.
- 명령어만으로 새 브랜치를 push하고 PR을 merge한다.
- merge 후 로컬과 원격 브랜치 정보를 정리한다.
- `git clone`으로 새 작업 환경을 내려받는 흐름을 확인한다.

## 0. 현재 상태 확인하기

프로젝트 폴더에서 시작한다.

```bash
cd /Users/buzz/Desktop/github-study-lab
git status
git branch -a
git log --oneline --decorate --all -10
```

확인할 것:

- 현재 브랜치가 `main`인가?
- 작업 중인 변경사항이 있는가?
- `origin/main`은 어떤 커밋을 가리키고 있는가?

## 1. 로컬 main을 원격 main과 맞추기

먼저 `main`으로 이동하고 GitHub의 최신 내용을 가져온다.

```bash
git switch main
git pull origin main
```

현재 상태를 확인한다.

```bash
git status
git log --oneline --decorate -5
```

`git pull`은 원격의 새 커밋을 가져오고 현재 브랜치에 반영한다.

## 2. fetch와 원격 브랜치 정리 연습하기

원격 저장소 정보를 받아오면서, GitHub에서 이미 삭제된 브랜치 기록도 정리한다.

```bash
git fetch --prune origin
git branch -a
```

기억할 것:

- `git fetch`는 원격 상태를 받아오지만 현재 파일을 자동으로 바꾸지는 않는다.
- `git pull`은 fetch 후 현재 브랜치에 변경을 반영한다.
- `--prune`은 원격에서 삭제된 브랜치의 로컬 추적 기록을 정리한다.

## 3. 4일차 작업 브랜치 만들기

이 파일을 `main`에 바로 올리지 않고 새 브랜치에서 커밋한다.

```bash
git switch -c day-04-remote-sync
git branch
git status
```

현재 브랜치에 `*` 표시가 붙었는지 확인한다.

```bash
* day-04-remote-sync
  main
```

## 4. 4일차 노트 커밋하기

```bash
git add notes/day-04.md
git commit -m "Add day 04 remote sync note"
git log --oneline --decorate -5
```

확인할 것:

- 가장 위 커밋이 `Add day 04 remote sync note`인가?
- 현재 커밋 옆에 `day-04-remote-sync`가 표시되는가?

## 5. 브랜치를 GitHub에 push하기

```bash
git push -u origin day-04-remote-sync
```

원격 브랜치가 등록되었는지 명령어로 확인한다.

```bash
git branch -a
git branch -vv
```

`day-04-remote-sync`가 `origin/day-04-remote-sync`를 추적하고 있으면 성공이다.

## 6. main과 작업 브랜치 차이 확인하기

PR을 만들기 전에 어떤 커밋과 파일이 들어갈지 확인한다.

```bash
git log --oneline main..day-04-remote-sync
git diff --stat main...day-04-remote-sync
git diff main...day-04-remote-sync
```

확인할 것:

- 새 커밋이 한 개 보이는가?
- `notes/day-04.md`만 추가되는가?

## 7. 명령어로 Pull Request 만들기

GitHub CLI 로그인 상태를 확인한다.

```bash
gh auth status
```

PR을 만든다.

```bash
gh pr create --base main --head day-04-remote-sync --title "Add day 04 remote sync note" --body "Day 04 원격 저장소 동기화 실습 노트를 추가했습니다."
```

PR 상태와 변경 파일을 확인한다.

```bash
gh pr view
gh pr diff --name-only
gh pr diff
```

## 8. 명령어로 Pull Request merge하기

변경 내용이 맞다면 merge하고 원격 작업 브랜치도 삭제한다.

```bash
gh pr merge --merge --delete-branch
```

PR 번호를 요구하거나 현재 PR을 찾지 못하면 아래처럼 진행한다.

```bash
gh pr list
gh pr merge PR번호 --merge --delete-branch
```

## 9. merge 결과를 로컬 main에 반영하기

```bash
git switch main
git pull origin main
git fetch --prune origin
git branch -a
ls notes
```

확인할 것:

- `notes/day-04.md`가 보이는가?
- `day-04-remote-sync` 원격 브랜치가 정리되었는가?
- `main`과 `origin/main`이 같은 최신 커밋을 가리키는가?

```bash
git log --oneline --decorate --all -10
```

## 10. clone으로 새 저장소 내려받아 보기

`clone`은 GitHub 저장소 전체를 새 폴더에 내려받는 명령어다. 기존 작업 폴더와 헷갈리지 않도록 임시 폴더에 실습한다.

```bash
git clone https://github.com/hyeongyu-data/github-study-lab.git /tmp/github-study-lab-day04-clone
cd /tmp/github-study-lab-day04-clone
git status
git remote -v
git log --oneline --decorate -5
ls notes
```

확인할 것:

- 새로 clone한 폴더도 Git 저장소인가?
- `origin` 주소가 자동으로 등록되어 있는가?
- `day-04.md`까지 내려받아졌는가?

실습을 마치고 원래 프로젝트 폴더로 돌아온다.

```bash
cd /Users/buzz/Desktop/github-study-lab
```

## 오늘 꼭 기억할 개념

- 로컬 저장소는 내 컴퓨터의 작업 공간이고, 원격 저장소는 GitHub에 있는 저장소다.
- `origin`은 보통 GitHub 원격 저장소에 붙인 이름이다.
- `fetch`는 원격 상태를 확인하기 위해 받아오는 작업이다.
- `pull`은 원격 변경을 현재 로컬 브랜치에 반영하는 작업이다.
- `push`는 로컬 커밋을 원격 저장소로 올리는 작업이다.
- `clone`은 원격 저장소를 새 로컬 저장소로 내려받는 작업이다.

## 오늘 꼭 기억할 명령어

```bash
git pull origin main
git fetch --prune origin
git branch -a
git switch -c 브랜치명
git push -u origin 브랜치명
git diff main...브랜치명
gh pr create --base main --head 브랜치명 --title "제목" --body "설명"
gh pr diff
gh pr merge --merge --delete-branch
git clone 원격주소 새폴더
```

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- GitHub Issue를 명령어로 만들고 작업 브랜치와 연결하는 흐름을 연습한다.
- Issue 번호를 커밋과 Pull Request에 연결하는 방법을 익힌다.
