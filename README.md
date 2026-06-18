# GitHub Study Lab

Git과 GitHub를 직접 손으로 익히기 위한 실습 저장소입니다.

## Study Goal

Git과 GitHub를 하루하루 명령어 중심으로 연습하면서, 실제 협업 흐름을 작은 단위로 익힙니다.

## Study Roadmap

1. Git 기본기: 저장소, 변경사항, 커밋
2. 브랜치와 병합: 실험 공간 만들기
3. GitHub 원격 저장소: push, pull, clone
4. Pull Request: 리뷰와 협업 흐름
5. Issues: 작업 생성과 PR 연결
6. Labels와 Milestones: 작업 분류와 목표 관리
7. Actions 기초: 자동화와 CI
8. Actions 로그와 workflow 개선
9. Actions 실패 디버깅
10. Actions 여러 job과 needs
11. Actions matrix 전략
12. Actions 다중 축 matrix
13. Actions matrix include/exclude
14. 다른 브랜치의 특정 폴더 가져오기

## Study Rhythm

- 하루에 하나씩 진도를 나갑니다.
- 학습 노트는 `notes/day-01.md`, `notes/day-02.md`처럼 날짜 순서가 보이도록 기록합니다.
- 각 날짜 파일에는 배운 것, 헷갈린 것, 다음에 해볼 것을 정리합니다.
- 가능한 모든 작업은 GitHub 웹 화면이 아니라 명령어로 진행합니다.
- 커밋 메시지는 한글로 작성합니다.

## Study Notes

| Day | Topic | Note |
| --- | --- | --- |
| 01 | Git 기본기와 첫 push | `notes/day-01.md` |
| 02 | 브랜치 생성과 원격 push | `notes/day-02.md` |
| 03 | Pull Request 생성과 merge | `notes/day-03.md` |
| 04 | fetch, pull, clone으로 원격 저장소 동기화 | `notes/day-04.md` |
| 05 | Issue 생성과 PR 자동 종료 | `notes/day-05.md` |
| 06 | 라벨과 마일스톤으로 Issue 관리 | `notes/day-06.md` |
| 07 | GitHub Actions 기초 자동 검사 | `notes/day-07.md` |
| 08 | Actions 로그 확인과 workflow 개선 | `notes/day-08.md` |
| 09 | Actions 실패 로그 디버깅 | `notes/day-09.md` |
| 10 | Actions 여러 job과 needs 실행 순서 | `notes/day-10.md` |
| 11 | Actions matrix 전략 | `notes/day-11.md` |
| 12 | Actions 다중 축 matrix | `notes/day-12.md` |
| 13 | Actions matrix include/exclude | `notes/day-13.md` |
| 14 | 다른 브랜치의 특정 폴더 가져오기 | `notes/day-14.md` |

## Progress

- Day 01: 로컬 저장소 생성, 첫 커밋, GitHub 원격 저장소 push 완료
- Day 02: 작업 브랜치 생성, 커밋, 원격 브랜치 push 완료
- Day 03: GitHub CLI로 Pull Request 생성, diff 확인, merge 완료
- Day 04: `fetch`, `pull`, `clone`, 원격 브랜치 정리 흐름 학습 완료
- Day 05: Issue 생성, `Closes #이슈번호`로 PR과 연결, merge 후 Issue 자동 종료 확인 완료
- Day 06: 라벨과 마일스톤을 만들고 Issue에 연결하는 흐름 정리
- Day 07: GitHub Actions workflow를 만들고 PR과 push에서 자동 검사가 실행되는 흐름 정리
- Day 08: Actions 실행 로그를 확인하고 workflow 수동 실행과 Node 24 opt-in 흐름 정리
- Day 09: Actions를 일부러 실패시킨 뒤 로그를 보고 workflow를 수정하는 흐름 정리
- Day 10: Actions workflow를 여러 job으로 나누고 `needs`로 실행 순서를 제어하는 흐름 정리
- Day 11: Actions matrix 전략으로 같은 job을 여러 입력값에 대해 반복 실행하는 흐름 정리
- Day 12: Actions matrix에 여러 축을 추가해 검사 조합이 늘어나는 흐름 정리
- Day 13: Actions matrix의 `exclude`와 `include`로 실행 조합을 조절하는 흐름 정리
- Day 14: `equipment` 브랜치의 `agents/equipment_cost` 폴더만 `main`으로 가져오는 흐름 정리

## Command Rules

자주 쓰는 기본 흐름:

```bash
git status
git switch -c 브랜치명
git add 파일명
git commit -m "한글 커밋 메시지"
git push -u origin 브랜치명
gh pr create --base main --head 브랜치명 --title "PR 제목" --body "Closes #이슈번호"
gh pr merge --merge --delete-branch
git switch main
git pull origin main
git fetch --prune origin
```

Issue 관리:

```bash
gh issue list
gh issue create --title "제목" --body "내용"
gh issue view 이슈번호
gh issue edit 이슈번호 --milestone "마일스톤명"
gh issue comment 이슈번호 --body "댓글"
```

라벨과 마일스톤 관리:

```bash
gh label list
gh label create "학습" --color "2EA043" --description "GitHub 학습용 작업"
gh api repos/:owner/:repo/milestones
```

Actions 확인:

```bash
gh workflow list
gh run list
gh run view
gh run watch
gh run view --log
gh workflow run "Study Check" --ref main
```

## Remote Repository

- GitHub: https://github.com/hyeongyu-data/github-study-lab.git
