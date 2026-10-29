# AI Human 7기 3차 프로젝트: RAG와 LangGraph 에이전트 서비스

이 저장소는 **3차 프로젝트 조별 공간**입니다. 조원 전원이 write 권한을 갖습니다.
그 산업의 공개 문서를 코퍼스로 삼아, 근거를 붙여 답하고, 필요하면 되묻고 재검색하며, 사람의 승인이 필요한 행동 앞에서 멈추는 에이전트형 RAG 서비스를 만듭니다.

## 팀 정보 (킥오프에서 조장이 작성)

| 항목 | 내용 |
|---|---|
| 프로젝트 | proj3 |
| 조 | (조 이름) |
| 주제와 산업 | (주제서 요약 한 줄) |
| 배포 주소 | (Cloud Run 주소, D0에 확보) |

## 조원 소개: 첫 과제

**조원 각자가 자기 행을 브랜치 → PR → 리뷰 → merge로 직접 추가하세요.** (전원 필수)
이 첫 PR이 협업 흐름 연습입니다. 같은 표를 여러 명이 수정하면 충돌이 날 수 있는데, 그것까지가 연습입니다(아래 "충돌이 났을 때" 참고).

| 이름 | GitHub | 역할 | 맡는 축 | 한마디 |
|---|---|---|---|---|
| 홍길동(예시) | @octocat | 검색 담당 | ② RAG 품질 | 잘 부탁드립니다 |

## 일정

| 날짜 | 해야 할 일 |
|---|---|
| 10/29(목) | 주제서 1장 제출 (강사에게 별도 DM. 제출한 내용을 `docs/TOPIC.md`로 옮겨 둔다) |
| 10/30(금) 오후 | 킥오프(D0). 빈 FastAPI가 compose로 뜨고 배포 주소가 열린다 |
| 11/3(화) | 평가셋 v1 30건 사람 판정과 동결, 베이스라인 점수 |
| 11/6(금) | 평가셋 v2 50건 이상 동결, Supervisor와 서브에이전트 분리, 중간 점검 |
| 11/16(월) 24:00 | 코드와 설정 동결 |
| 11/17(화) 24:00 | 최종 제출 |
| 11/18(수) | 발표와 수료식 |

일별 목표는 3차 프로젝트 가이드의 일정 표를 따릅니다. 이 표는 이 저장소에서 꼭 지켜야 할 날짜만 뽑았습니다.

## 필수 게이트 (못 지키면 해당 점수만큼 고정 감점)

| 번호 | 조건 | 감점 |
|---|---|---:|
| G1 | 배포된 주소가 있고 평가용 계정으로 핵심 시나리오를 끝까지 볼 수 있다(또는 영상) | -5 |
| G2 | `docker compose up` 한 줄로 10분 안에 뜬다(키 없이 뜨는 데모 모드 포함) | -5 |
| G3 | 서로 다른 청크 1만 개 이상, 서로 다른 문서 50건 이상, 문서 파싱 1종 이상 | -10 |
| G4 | LangGraph 상태 그래프, 멀티에이전트 최소형(Supervisor와 서브에이전트 2개 이상), 체크포인트, 사람 승인(interrupt), 로그인과 사용자별 기억 | -10 |
| G5 | 평가셋 50건 이상으로 개선 전후를 두 번 이상 재서 회귀를 숫자로 보인다 | -10 |
| G6 | Langfuse(또는 동급)로 트레이스가 남고 평가 점수가 연결된다 | -5 |
| G7 | README, `.env.example`, 비밀값 커밋 없음, 3분 이내 시연 영상(정상 경로와 실패 경로) | -5 |

각 게이트를 이 저장소 어디에서 채우는지는 **[docs/GATES.md](./docs/GATES.md)**에 있습니다. 정의의 정본은 3차 프로젝트 가이드입니다.

## 협업 규칙 (필독)

1. **`main`에 직접 push 금지**: 모든 변경은 브랜치 → PR로만 합칩니다.
2. 브랜치명: `feat/{작업내용}` (예: `feat/hybrid-search`, 버그 수정은 `fix/{내용}`)
3. PR은 **조원 1명 이상의 approve를 받은 뒤** merge합니다. 리뷰 없는 merge는 감사 리포트에 잡힙니다.
4. **제출물 = 마감 시점의 `main`**: 마감 시각에 강사가 전체 조 저장소에 태그를 일괄 생성합니다. 태그 이후 커밋은 평가 대상이 아닙니다.
5. **커밋은 본인 계정으로**: 자기가 한 작업은 자기 계정으로 커밋해야 이 저장소가 본인 포트폴리오 증빙이 됩니다. 함께 작업했다면 커밋 메시지에 `Co-authored-by:`를 추가하세요.
6. 다른 조 저장소도 읽을 수 있습니다. 보고 배우는 것은 권장, 복사 제출은 금지입니다.
7. **동결한 평가셋은 고치지 않습니다.** v1과 v2를 동결한 뒤에 정답을 바꾸면 측정이 아니라 맞추기가 됩니다(`evals/README.md`).

## 폴더 구조

```
backend/            FastAPI 서버
  main.py             진입점 (/health)
  graph/              LangGraph: 상태, Supervisor, 서브에이전트        (G4)
  rag/                문서 파싱, 청킹, 인덱싱, 하이브리드 검색, 리랭킹  (G3)
  auth.py             로그인과 사용자 식별                              (G4)
  memory.py           사용자별 장기 기억 (Store namespace)              (G4)
  services/llm.py     LLM 호출은 여기 한 곳. 데모 모드 재생도 여기      (G2, G6)
data/               corpus/ 원문 스냅샷, manifest                       (G3)
evals/              평가셋 v1, v2, 동결 스크립트, 사전 등록              (G5)
tests/              회귀 테스트 (CI가 PR마다 실행)
db/init.sql         pgvector 확장 생성
docs/               주제서, 아키텍처, 게이트 점검, 제출물, Spec
EVAL_REPORT.md      개선 전후 지표 (제출물)
extras/mcp_skill/   MCP 서버와 Skill (가산 선택)
Dockerfile, docker-compose.yml   `docker compose up` 한 줄 실행
```

각 폴더가 무엇이고 무엇을 채우는지는 **[docs/SCAFFOLD.md](./docs/SCAFFOLD.md)**에 있습니다. 쓰지 않는 것은 지워도 됩니다.

## 시크릿 규칙 (위반 시 전원에게 노출됩니다)

- API 키와 비밀번호는 **`.env` 파일에만** 두세요. `.env`는 `.gitignore`에 이미 등록되어 커밋되지 않습니다.
- 코드나 문서에 키를 직접 적으면 안 됩니다. 이 org는 상호 공개라 **커밋된 키는 7기 전원이 볼 수 있습니다.**
- 평가용 계정의 비밀번호도 이 저장소에 적지 않습니다. 강사에게 별도 DM으로 제출합니다.
- 모든 push는 **Secret Scan**(GitHub Actions)이 자동 검사합니다. 검사가 실패하면 키가 커밋된 것입니다. 즉시 강사에게 알리고 **해당 키를 재발급**하세요. 히스토리에서 지워도 유출된 것으로 간주합니다.
- 필요한 키 목록은 `.env.example`에 값 없이 적어 공유하세요.
- LLM 키는 조당 하나이고 한도가 있습니다. 코드에서 무한 재시도하는 루프가 한도를 소진시키지 않게 상한을 두세요.

## 작업 흐름

```bash
# 1. 최신 main에서 작업 브랜치 생성
git switch main && git pull
git switch -c feat/hybrid-search

# 2. 작업 후 커밋과 push
git add .
git commit -m "feat: 하이브리드 검색 추가"
git push -u origin feat/hybrid-search

# 3. GitHub에서 PR 생성 → 조원 리뷰 → approve 후 merge
```

## 충돌(conflict)이 났을 때

PR 화면에 "This branch has conflicts"가 뜨면:

```bash
git switch main && git pull          # 최신 main 받기
git switch feat/내브랜치
git merge main                       # 충돌 발생 지점이 파일에 표시됨
# 파일 열어 <<<<<<< ======= >>>>>>> 사이에서 남길 내용 선택 후 저장
git add . && git commit              # 충돌 해결 커밋
git push                             # PR이 자동 갱신됨
```

당황하지 마세요. 충돌은 사고가 아니라 협업의 일상입니다. 막히면 조원 또는 강사를 부르세요.

## 질문

강사 확인이 필요한 질문은 이 저장소의 **Issues**에 남기고 강사를 멘션하세요.

## 공통 준비 자료

상세 요구조건·일정·수업 대응표는 [공통 안내](docs/project-guide/ROOT_PROJECT_GUIDE.md), 역할 확인은 [팀 운영](docs/TEAM.md)을 참고합니다. [개발·리뷰·CI 정책](docs/TEAM_POLICY.md)은 합의 전 초안입니다. 실제 게이트 상태와 증거는 기존 [docs/GATES.md](docs/GATES.md)에 기록합니다.

## 폴더별 진입점

최상위 README는 팀·실행·일정 개요를 유지합니다. 작업할 영역의 안내를 먼저 보고 필요한 파일만 읽습니다.

- [.github](.github/ROOT_GITHUB.md) — PR 양식과 GitHub Actions 설정
- [backend](backend/ROOT_BACKEND.md) — FastAPI 진입점·인증·기억과 그래프/검색/LLM 모듈 연결
- [data](data/ROOT_DATA.md) — 서비스 코퍼스와 문서/청크 manifest 보관
- [db](db/ROOT_DB.md) — 로컬 DB 초기화 SQL
- [docs](docs/ROOT_DOCS.md) — 주제·아키텍처·Spec·실제 게이트 상태와 공통 안내
- [evals](evals/ROOT_EVALS.md) — 평가셋·동결·실험 사전 등록
- [extras](extras/ROOT_EXTRAS.md) — 선택 가산 기능
- [tests](tests/ROOT_TESTS.md) — 동결 평가셋과 서비스 회귀 테스트
