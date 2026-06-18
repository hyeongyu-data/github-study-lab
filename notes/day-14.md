# Day 14

## 오늘의 주제

다른 브랜치에 있는 특정 폴더만 `main` 브랜치로 가져와 커밋하는 흐름을 연습한다.

## 오늘의 목표

- 특정 브랜치 전체를 merge하지 않고 필요한 폴더만 가져온다.
- `git checkout 브랜치 -- 경로` 명령어의 의미를 이해한다.
- 가져온 폴더를 `main`에서 커밋하고 push한다.
- 브랜치 간 특정 파일/폴더 단위 병합 흐름을 익힌다.

## 0. 현재 상태 확인하기

프로젝트 폴더에서 시작한다.

```bash
cd /Users/buzz/Desktop/github-study-lab
git status
git branch
```

확인할 것:

- 현재 작업 중인 변경사항이 없는가?
- `equipment` 브랜치가 존재하는가?
- `equipment` 브랜치에 `agents/equipment_cost` 폴더가 있는가?

## 1. main 브랜치로 이동하기

```bash
git checkout main
```

확인한다.

```bash
git branch
git status
```

현재 브랜치가 `main`이어야 한다.

## 2. equipment 브랜치의 특정 폴더만 가져오기

```bash
git checkout equipment -- agents/equipment_cost
```

이 명령어의 의미:

- 현재 브랜치는 `main`이다.
- `equipment` 브랜치 전체를 merge하지 않는다.
- `equipment` 브랜치에 있는 `agents/equipment_cost` 폴더만 현재 작업 트리로 가져온다.

가져온 뒤 상태를 확인한다.

```bash
git status
ls agents
ls agents/equipment_cost
```

## 3. 변경사항 스테이징하기

```bash
git add agents/equipment_cost
```

스테이징 상태를 확인한다.

```bash
git status
```

## 4. 한글 커밋 메시지로 커밋하기

```bash
git commit -m "equipment 브랜치의 equipment_cost 에이전트 폴더 병합"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 5. main 브랜치를 원격 저장소에 push하기

```bash
git push origin main
```

push 후 상태를 확인한다.

```bash
git status
git log --oneline --decorate -5
```

## 오늘 실행할 전체 명령어

```bash
git checkout main
git checkout equipment -- agents/equipment_cost
git add agents/equipment_cost
git commit -m "equipment 브랜치의 equipment_cost 에이전트 폴더 병합"
git push origin main
```

## 최신 명령어로 쓰는 대안

요즘 Git에서는 `checkout` 대신 `switch`와 `restore`를 나누어 쓰기도 한다.

```bash
git switch main
git restore --source equipment -- agents/equipment_cost
git add agents/equipment_cost
git commit -m "equipment 브랜치의 equipment_cost 에이전트 폴더 병합"
git push origin main
```

둘 다 같은 목적이다. 오늘은 사용자가 제시한 `git checkout equipment -- agents/equipment_cost` 흐름을 기준으로 연습한다.

## 주의할 점

- `equipment` 브랜치가 로컬에 있어야 한다.
- `agents/equipment_cost` 경로가 `equipment` 브랜치에 실제로 있어야 한다.
- 이 방식은 브랜치 전체 병합이 아니라 특정 경로만 가져오는 방식이다.
- 가져온 폴더는 현재 브랜치인 `main`의 새 변경사항으로 커밋된다.

## 오늘 꼭 기억할 개념

- `git checkout 브랜치 -- 경로`는 특정 브랜치의 특정 파일이나 폴더를 현재 브랜치로 가져온다.
- 전체 merge가 부담스러울 때 필요한 파일이나 폴더만 선택적으로 가져올 수 있다.
- 가져온 내용은 자동 커밋되지 않으므로 `git add`, `git commit`이 필요하다.
- 커밋 후 원격 `main`에 반영하려면 `git push origin main`을 실행한다.

## 오늘 꼭 기억할 명령어

```bash
git checkout main
git checkout equipment -- agents/equipment_cost
git add agents/equipment_cost
git commit -m "equipment 브랜치의 equipment_cost 에이전트 폴더 병합"
git push origin main
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `equipment 브랜치의 equipment_cost 에이전트 폴더 병합`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- 특정 파일 하나만 다른 브랜치에서 가져오는 흐름을 연습한다.
- 브랜치 전체 merge와 경로 단위 checkout의 차이를 비교한다.
