# Day 01

## 배운 것

- Git은 로컬에서 변경 이력을 관리하고, GitHub는 그 저장소를 온라인에 올려 공유하는 서비스다.
- `git init`, `git add`, `git commit`으로 첫 커밋을 만들었다.
- `gh repo create ... --source=. --remote=origin --push`로 GitHub 저장소 생성, 원격 연결, push를 한 번에 진행할 수 있다.
- 현재 원격 저장소는 `https://github.com/hyeongyu-data/github-study-lab.git`이다.
```
cd /github-study-lab
git status
git add 파일명 # 원하는 파일만 add
git add -A # 전체 add
git commit -m "메세지" # 커밋 메세지 입력
git push
```
- push 한거 되돌리기
```
git reset --soft HEAD~1
git push --force-with-lease
```


## 헷갈린 것

- `gh`는 기본 Git 명령어가 아니라 GitHub CLI라서 따로 설치해야 한다.
- 순수 `git`만으로는 GitHub에 새 저장소를 만들 수 없고, 이미 존재하는 원격 저장소에 연결해서 push할 수 있다.

## 다음에 해볼 것

- 브랜치를 만들어서 별도 작업 흐름을 연습한다.
- 수정사항을 커밋한 뒤 GitHub에 다시 push한다.
- Pull Request 흐름을 익힌다.
