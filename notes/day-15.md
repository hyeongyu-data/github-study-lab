# Day 15

## 오늘의 주제

다른 브랜치에서 특정 파일 하나만 가져오기 전에 차이를 확인하고, 필요한 파일만 `main`에 반영하는 흐름을 연습한다.

## 오늘의 목표

- 다른 브랜치의 특정 파일 차이를 먼저 확인한다.
- 전체 브랜치 merge 없이 파일 하나만 가져온다.
- `git diff 브랜치 -- 파일경로`와 `git checkout 브랜치 -- 파일경로`를 함께 사용한다.
- 가져온 파일을 커밋하고 원격 `main`에 push한다.

## 0. 현재 상태 확인하기

프로젝트 폴더에서 시작한다.

```bash
cd /Users/buzz/Desktop/github-study-lab
git status
git branch
```

확인할 것:

- 현재 작업 중인 변경사항이 없는가?
- 현재 브랜치가 `main`인가?
- 가져오려는 대상 브랜치가 존재하는가?

## 1. main 브랜치로 이동하기

```bash
git checkout main
```

최신 상태로 맞춘다.

```bash
git pull origin main
```

상태를 확인한다.

```bash
git status
```

## 2. 가져올 파일 후보 확인하기

예시로 `equipment` 브랜치의 특정 파일을 가져온다고 가정한다.

```bash
git ls-tree -r equipment --name-only
```

`agents/equipment_cost` 폴더 안의 파일만 보고 싶다면 아래처럼 확인한다.

```bash
git ls-tree -r equipment --name-only agents/equipment_cost
```

## 3. 가져오기 전에 차이 확인하기

가져오려는 파일 경로를 먼저 정한다.

예시:

```bash
agents/equipment_cost/README.md
```

현재 `main`과 `equipment` 브랜치의 해당 파일 차이를 확인한다.

```bash
git diff equipment -- agents/equipment_cost/README.md
```

주의:

- 이 명령어는 파일을 가져오지 않는다.
- 현재 브랜치와 `equipment` 브랜치의 차이만 보여준다.
- diff를 보고 정말 가져올지 판단한다.

## 4. 특정 파일 하나만 가져오기

차이를 확인한 뒤 필요한 파일만 가져온다.

```bash
git checkout equipment -- agents/equipment_cost/README.md
```

가져온 뒤 상태를 확인한다.

```bash
git status
git diff
```

## 5. 가져온 파일 스테이징하기

```bash
git add agents/equipment_cost/README.md
```

스테이징 상태를 확인한다.

```bash
git status
git diff --staged
```

## 6. 한글 커밋 메시지로 커밋하기

```bash
git commit -m "equipment 브랜치의 equipment_cost README 파일 반영"
```

커밋을 확인한다.

```bash
git log --oneline --decorate -5
```

## 7. 원격 main에 push하기

```bash
git push origin main
```

push 후 상태를 확인한다.

```bash
git status
git log --oneline --decorate -5
```

## 오늘 실행할 전체 흐름

```bash
git checkout main
git pull origin main
git ls-tree -r equipment --name-only agents/equipment_cost
git diff equipment -- agents/equipment_cost/README.md
git checkout equipment -- agents/equipment_cost/README.md
git status
git diff
git add agents/equipment_cost/README.md
git diff --staged
git commit -m "equipment 브랜치의 equipment_cost README 파일 반영"
git push origin main
```

## 최신 명령어로 쓰는 대안

`checkout` 대신 `restore`를 사용할 수도 있다.

```bash
git switch main
git pull origin main
git diff equipment -- agents/equipment_cost/README.md
git restore --source equipment -- agents/equipment_cost/README.md
git add agents/equipment_cost/README.md
git commit -m "equipment 브랜치의 equipment_cost README 파일 반영"
git push origin main
```

## 14일차와의 차이

- 14일차는 `agents/equipment_cost` 폴더 전체를 가져왔다.
- 15일차는 폴더 전체가 아니라 파일 하나만 가져온다.
- 15일차에서는 가져오기 전에 `git diff`로 차이를 먼저 확인한다.

## 주의할 점

- 파일 경로가 `equipment` 브랜치에 실제로 있어야 한다.
- `git diff equipment -- 파일경로`는 차이를 보여줄 뿐 파일을 가져오지 않는다.
- `git checkout equipment -- 파일경로`를 실행해야 실제로 현재 브랜치에 파일이 반영된다.
- 가져온 파일은 자동 커밋되지 않는다.

## 오늘 꼭 기억할 개념

- 다른 브랜치에서 파일 하나만 가져올 수 있다.
- 가져오기 전에 diff를 확인하면 불필요한 변경을 줄일 수 있다.
- 경로 단위 checkout은 전체 merge보다 범위가 작고 명확하다.
- 작은 단위로 가져오고 작은 단위로 커밋하는 습관이 좋다.

## 오늘 꼭 기억할 명령어

```bash
git ls-tree -r 브랜치명 --name-only 경로
git diff 브랜치명 -- 파일경로
git checkout 브랜치명 -- 파일경로
git add 파일경로
git diff --staged
git commit -m "한글 커밋 메시지"
git push origin main
```

## 커밋 메시지 규칙

- 커밋 메시지는 한글로 작성한다.
- 무엇을 했는지 짧고 명확하게 적는다.
- 예시: `equipment 브랜치의 equipment_cost README 파일 반영`

## 헷갈린 것

- 

## 실습 기록

- 

## 다음에 해볼 것

- 브랜치 전체 merge와 파일 단위 checkout의 결과 차이를 비교한다.
- 같은 파일이 양쪽 브랜치에서 수정되었을 때 충돌이 어떻게 보이는지 확인한다.
