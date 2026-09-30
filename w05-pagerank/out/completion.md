# Week 5 필수 과제 완료 보고

## 수정·생성 파일

기존 파일 중 수정한 것은 아래 3개뿐이다. 첨부 원본과 비교하여 `bench.py`, `test_tasks.py`, README, Task 1–4 명세, 선택 과제 코드가 그대로임을 SHA-256으로 확인했다. 저장소의 `check.py`도 수정하지 않았다.

| 수정 파일 | 구현 내용 |
|---|---|
| `task1_pagerank.py` | 균등 초기 분포, damping, dead-end 균등 재분배, L1 조기 종료, 실제 반복 횟수·종료 delta 기록. 별도의 broken 구현은 재분배와 teleportation을 생략하여 두 실패를 재현한다. |
| `task2_convergence.py` | 기존 누적 JSON 형식을 유지하며 실제 수렴 여부와 delta, 반복 상한, CPU·RAM·실행 중 프로그램 정보를 기록한다. UTF-8 및 파일 context manager를 사용한다. |
| `task3_sparse.py` | 실제 간선의 정수 목적지 인덱스, 역 out-degree와 rank 벡터를 사용한다. DenseMatrix 기준 구현은 그대로 유지한다. |

`w05-pagerank/out/`에 생성한 필수 제출 파일:

- `convergence.json`: beta 5개, 크기 1,200/20,000, tolerance 1e-3/1e-6/1e-10에 대한 실제 실행 17개. 최종 재실행도 보존한다.
- `convergence.md`: beta·크기·tolerance 표, 관찰 해석, top-10 순위 변화, 실행 환경.
- `bench.txt`: 최종 `bench.py --yours` stdout 원문.
- `observation.md`: Task마다 실제 결과를 바탕으로 3줄씩 기록.

추가 제출 참고 파일은 `verification.txt`(실행 명령·출력·종료 코드)와 이 `completion.md`다.
`w05-pagerank-submission.zip`에는 완성한 `w05-pagerank/`와 원본 `check.py`를 포함한다. 임시 분석 스크립트와 Python 캐시는 ZIP에서 제외한다.

## 실제 검증 결과

아래 다섯 명령의 인수를 지정된 순서대로 실행했다. Windows에 `python3` 명령이 없어 제공된 Python 3.12.14 실행 파일로 각 스크립트를 실행했고, 로그에는 실제 실행 파일의 절대 경로를 기록했다.

| 명령 | 결과 |
|---|---|
| `python3 task1_pagerank.py --verify` | 7개 체크 모두 통과, 종료 코드 0 |
| `python3 task2_convergence.py --betas 0.5,0.7,0.85,0.95,0.99` | 5개 beta 모두 tolerance 도달, 종료 코드 0 |
| `python3 bench.py --yours` | 최대 노드 오차 4.16e-17, 속도 75.5배, float 수 398.2배 개선, strong 판정, 종료 코드 0 |
| `python3 test_tasks.py` | 8 passed, 0 failed, 1 skipped, 종료 코드 0 |
| `python3 ../check.py w05` | 필수 제출 파일과 형식 확인 통과, 종료 코드 0 |

추가로 단일 노드, 전체 dead end, 순환 그래프, trap, 고정 seed의 작은 임의 그래프를 beta 0/0.5/0.85/0.99에서 dense와 비교했다. 44개 그래프·beta 조합의 최대 오차는 1.22e-15였고, 질량 보존·조기 종료·반복 상한·재실행 상태 초기화도 확인했다.
결과 문서를 최종 측정값으로 갱신한 뒤 제출 형식 검사도 다시 실행했다.

## 오류와 남은 사항

구현 검증에서 실패한 항목은 없으며, 미해결 필수 오류도 없다.
초기 환경 문제는 `python3` 대신 제공된 Python 런타임 사용, Git HTTPS helper 대신 저장소 ZIP 다운로드, 접근이 거부된 CIM 조회 대신 Python 표준 라이브러리의 Windows 메모리 API·레지스트리 조회로 해결했다.
`test_tasks.py`의 1개 SKIP은 Task 3의 teleport 설명을 사람이 평가하도록 원래 설정된 항목이다. 해당 설명은 `observation.md`에 작성했다.
메모리 비교는 과제 harness가 정의한 **보유 float 수**이며 Python 객체·정수 간선·컨테이너 오버헤드를 포함한 전체 RAM 비율은 아니다. 실행 시간은 이 머신의 실제 관찰값이며 재실행 시 달라질 수 있다.
선택 Task 4 Spark는 수행하지 않았다. 필수 과제가 완료됐으므로 별도 선택 실습으로 진행할 수 있다.
