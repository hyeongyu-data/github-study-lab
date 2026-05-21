# Day 02

## 오늘의 주제

브랜치를 만들어서 `main`과 분리된 작업 흐름을 연습한다.

## 오늘의 목표

- 현재 폴더가 Git 저장소인지 확인한다.
- 브랜치가 왜 필요한지 이해한다.
- 새 브랜치를 만들고 이동한다.
- 새 파일을 수정하고 커밋한다.
- 만든 브랜치를 GitHub에 push한다.

## 0. 먼저 현재 위치 확인하기

터미널에서 프로젝트 폴더로 이동한다.

```bash
cd /Users/buzz/Desktop/github-study-lab
pwd
```

현재 위치가 아래처럼 나오면 맞다.

```bash
/Users/buzz/Desktop/github-study-lab
```

## 1. Git 저장소인지 확인하기

```bash
git status
```

정상이라면 현재 브랜치 이름과 변경사항 상태가 나온다.

예시:

```bash
On branch main
nothing to commit, working tree clean
```

만약 아래처럼 나오면 현재 폴더는 Git 저장소가 아니다.

```bash
fatal: not a git repository (or any of the parent directories): .git
```

이 경우에는 GitHub에서 다시 clone하거나, 실제 `.git`이 있는 저장소 폴더로 이동해야 한다.

```bash
cd /Users/buzz/Desktop
git clone https://github.com/hyeongyu-data/github-study-lab.git
cd github-study-lab
```

## 2. 현재 브랜치 확인하기

```bash
git branch
```

현재 브랜치에는 `*` 표시가 붙는다.

예시:

```bash
* main
```

브랜치는 독립적인 작업 공간이다. 보통 `main`은 안정적인 기본 흐름으로 두고, 새로운 작업은 별도 브랜치에서 진행한다.

## 3. 새 브랜치 만들고 이동하기

```bash
git switch -c day-02-branch-practice
```

확인한다.

```bash
git branch
```

예시:

```bash
* day-02-branch-practice
  main
```

## 4. 파일 수정하기

이 파일 아래의 실습 기록 부분에 직접 내용을 추가한다.

예시:

```md