# 2조 프로젝트 저장소 결합 준비안

준비일: 2026-10-29 (Asia/Seoul). **준비만 완료했으며 대상 저장소에 적용·커밋·push·PR 생성하지 않았습니다.**

대상: https://github.com/aihuman-7th/proj3-b (`main`)
기준 커밋: `f06acf2221f48e448c54c7254e8b9b4b76612170`
원본: https://github.com/nanocode00/project3-planning (`d1624fba21693808268107bf15a8a41d294dbe34` 및 현재 로컬 공통 문서 변경)

## 현재 구조에서 확인한 것

- 제품 코드는 `backend/`, 코퍼스는 `data/`, 평가셋·사전 등록은 `evals/`, 제출 보고서는 `EVAL_REPORT.md`에 있습니다.
- 기존 `docs/GATES.md`는 구현 위치·실제 진행 상태·증거·변이 확인을 기록하는 공식 점검표입니다. 준비 저장소의 상세 체크리스트로 덮어쓰지 않습니다.
- 기존 `docs/TOPIC.md`, `docs/ARCHITECTURE.md`, `docs/specs/_example.md`, `docs/deliverables/`가 있습니다. 같은 역할의 빈 주제서와 실험 등록 양식을 새 정본으로 만들지 않습니다.
- README는 조원 각자가 자기 소개 행을 PR로 추가하도록 지시합니다. 이번 준비본은 팀원 소개 행을 대신 작성하지 않습니다.
- `AGENTS.md`는 Spec-first, LLM 호출 경계, 동결 평가셋, 실험 사전 등록, 비밀값·코퍼스 경계·루프 상한을 이미 규정합니다. 기존 본문은 보존합니다.
- 현재 CI는 PR마다 Python 3.12 환경에서 개발 의존성을 설치하고 pytest를 실행합니다. Secret Scan은 PR과 main push에서 실행합니다. Actions 실행 결과나 branch protection 설정은 이번에 검증하지 않았습니다.

## 배치 결정

| 대상 경로 | 내용/이유 |
|---|---|
| `docs/project-guide/ROOT_PROJECT_GUIDE.md` | 공통 자료 진입점과 문서별 책임 |
| `docs/project-guide/GATE_REQUIREMENTS.md` | 기존 상세 GATES를 이름 변경. 조건 설명용이며 상태는 기존 `docs/GATES.md`에 기록 |
| `docs/project-guide/EVALUATION.md`, `SCHEDULE.md`, `DELIVERABLES.md` | 평가·날짜별 실행 제안·제출 요구조건 |
| `docs/project-guide/COURSE_*.md` | 수업 적용법·101개 조건 대응·원본 파일/셀 위치 |
| `docs/project-guide/CHECKPOINT.md`, `NOTION.md`, `PDF.md` | 10/26 출처별 누락 확인. 현재 편성과 과거 기록 구분 |
| `docs/project-guide/sources/` | 기존 수집 원문·해시 manifest·강의 catalog. 서비스 코퍼스인 `data/`와 구분 |
| `docs/project-guide/EVIDENCE_TEMPLATE.md` | 빈 증거 기록 형식. 사전 등록·결과의 정본은 기존 `evals/`·`EVAL_REPORT.md` |
| `docs/TEAM.md` | 공식 편성 참고·역할 합의·운영 체크리스트 |
| `docs/TEAM_POLICY.md` | 공통 개발·리뷰·CI 정책의 합의 전 초안 |
| `docs/reference/ci-cost-proposal.md` | 사용자 CI 추가안의 제안 이력. 현재 설정으로 오인하지 않음 |
| 루트 README·AGENTS·기존 docs/GATES | README·AGENTS에 탐색 경로, GATES에 안내 링크 추가. 기존 본문·상태 표·실험 기록 보존 |
| `.gitattributes` | 안내 스냅샷 경로에만 원문 바이트 보존 규칙 추가 |

준비본은 총 **50개 파일**입니다. 기존 3개 파일에 탐색 안내를 추가하고 신규 47개 파일을 준비했습니다. 제품 코드·평가셋·workflow는 변경하지 않습니다.

## 폴더별 진입 문서 (10/29 팀 협의 반영)

- 최상위 `README.md`는 개요·팀·실행·일정을 유지하고 주요 폴더 진입 문서를 연결합니다.
- 뼈대의 22개 폴더에는 이름이 겹치지 않는 `ROOT_*.md`를 둡니다. 예: `backend/ROOT_BACKEND.md`, `backend/graph/ROOT_BACKEND_GRAPH.md`, `docs/ROOT_DOCS.md`.
- 공통 안내는 `docs/project-guide/ROOT_PROJECT_GUIDE.md`, 출처 안내는 `sources/ROOT_GUIDE_SOURCES.md`로 이름을 바꿉니다. 기존 data·evals·MCP의 README는 상세 안내로 유지합니다.
- 각 진입 문서는 역할·상위 링크·직접 하위 폴더의 진입점·직접 확인할 파일만 안내합니다. 파일별로 필요한 경로를 따라 내려가며 규칙·숫자·계약을 복제하지 않습니다.
- `AGENTS.md`에 모든 폴더의 진입 문서 위치와 “영역 진입점 → 필요한 하위 진입점 → 관련 Spec/코드” 탐색 규칙을 명시합니다. 모든 문서를 일괄 로드하지 않습니다.
- corpus·평가 결과 폴더의 탐색 문서는 코퍼스 집계나 평가 결과로 취급하지 않습니다.
- 전체 경로는 `manifest.json`의 `folder_entries`와 준비본 AGENTS에 기록합니다. 이 탐색 구조도 대상 저장소에는 아직 적용하지 않았습니다.

## 이관 전 정리한 내용

- StartFit 명칭을 제거하고 2조 공통 정책 초안으로 정리했습니다.
- 운영 중인 문서의 3인 담당·포트폴리오 총3장 표현을 현재 4인 구성 또는 전원/역할 기준으로 바꿨습니다. PDF·과거 편성 원문의 역사적 3인 설명은 변경하지 않았습니다.
- A/B/C 담당 자리표시자는 검색·에이전트·운영 등 역할명으로 바꿨습니다. 평가 계정 A/B와 실험 A/B는 유지했습니다.
- 실제 게이트 상태, 조건 상세, 실험 등록, 결과, 팀 소개의 정본을 구분했습니다.
- 문서 간 상대 링크를 이관 위치에 맞게 다시 연결했습니다.
- 강의 링크 93개를 공개 강의 저장소의 확인된 커밋 `0097f0e2d3cd1f4fab1b53ec80e45b9dfdf62bad`으로 고정했습니다. 강의 원문은 포함하지 않습니다. 이 배포 참고본이 조사 당시 파일/셀 지문과 일치한다고 단정하지 않습니다.
- 강의 catalog의 개인 로컬 절대 경로를 상대 경로/`${COURSE_ROOT}`로 바꾸고 조사일·기존 파일/셀 해시는 보존했습니다.
- 과거 공식 안내 스냅샷 8개는 원본 바이트를 보존했습니다. 후속 공식 공지 전체를 새로 대조하거나 외부 재배포 조건을 새로 확인한 것은 아닙니다.

## 제외한 것

- 전자부품·StartFit·금융·AI 백엔드 등 아이디어 및 버전별 주제 기획안 전체
- `docs/OpsMate-주제서-v3.md`, `docs/SPEC.md` (개별 제품 기획/설계)
- 중복 빈 주제서 `templates/TOPIC.md` (기존 공식 `docs/TOPIC.md`로 연결)
- `.env`, API 키·계정 정보, `private/`의 Mouser 실험·Postman 자료
- 준비 저장소의 Git 이력·ignore 설정·원본 README 전체
- 기존 코드·평가셋·주제서·아키텍처·CI·보호 브랜치 설정 변경

## CI 후속 작업 경계

| 항목 | 현재 확인 | 이후 실제 적용 작업 |
|---|---|---|
| 중복 trigger | 테스트는 PR만, Secret Scan은 PR+main push | 현재 이미 줄인 중복을 다시 변경하지 않음 |
| 변경 범위 선택 | 테스트 job은 변경 파일에 관계없이 실행 | 실제 `backend/`, `tests/`, `evals/`, `extras/mcp_skill/`, 공통 설정·실행 Markdown의 영향 매핑 |
| 캐시 | pip cache와 `backend/requirements-dev.txt` 키 설정 | 실제 lock/requirements 포함 범위·효과 확인 |
| concurrency·timeout | 두 workflow에 해당 설정 없음 | 검증 취소 그룹과 job timeout 설계 |
| 필수 집계 | `ci-required` job 없음 | 실패·취소·예상 밖 skip을 통과시키지 않는 집계와 분류 검증 |
| Secret Scan | 별도 workflow에서 gitleaks 실행 | 문서 전용 변경에서도 유지, 결과 연동 방안 결정 |
| heavy 검증·관리자 설정 | 실행/설정 확인 범위 밖 | 책임자·시점·증거·팀원 승인·보호 브랜치 별도 확인 |

이번 패치는 문서 준비 범위입니다. 정책 문서를 추가해도 이 설정들이 자동 적용되지는 않습니다. 실제 CI 구현은 별도 작업으로 검증합니다.

## 준비 산출물과 적용 전 확인

- `payload/`: 이관할 파일만 대상 경로대로 배치한 후보
- `manifest.json`: 출발/도착 경로, 원본·준비본 해시, 대상 기준 SHA
- `diff-stat.txt`, `validation.json`: 변경 규모와 실제 정적 검증 결과

실제 적용 지시를 받으면 최신 원격 main과 충돌을 다시 확인하고, 사용자/팀의 미커밋 작업을 보존한 작업 브랜치에서 반영합니다. 이 미리보기의 후보 파일을 자동 적용하거나 프로젝트 main에 직접 push하지 않습니다. 정책 확정은 팀 리뷰로 진행하며, 원문 외부 재배포는 출처의 기존 주의사항을 유지합니다.

정적 검증은 문서·링크·해시·제외 범위·패치 적용 가능성에 한정됩니다. 제품 테스트·외부 API·배포·GitHub Actions는 실행하지 않았습니다.
