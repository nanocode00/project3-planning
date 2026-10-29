# 프로젝트 저장소 이관 예정 파일

**아래 파일을 2조 프로젝트 저장소에 옮길 예정입니다. 현재는 검토용 미리보기이며 실제 이관·프로젝트 PR 생성은 하지 않았습니다.**

- 대상: [aihuman-7th/proj3-b](https://github.com/aihuman-7th/proj3-b)
- 구조 확인 기준: `f06acf2221f48e448c54c7254e8b9b4b76612170` (2026-10-29)
- [배치 이유·기존 구조·CI 후속 작업](PLAN.md) · [검증 기록](validation.json) · [파일/해시 목록](manifest.json)

## 무엇이 달라지나

1. 강사님이 잡아둔 제품 폴더 구조를 유지합니다.
2. 최상위 README는 개요로 두고, 22개 폴더의 고유한 `ROOT_*.md`로 상위에서 하위까지 탐색합니다. AGENTS에는 전체 진입 경로를 명시합니다.
3. 공통 요구조건·일정·수업 대응은 `docs/project-guide/`, 팀 역할·정책은 `docs/TEAM.md`·`docs/TEAM_POLICY.md`로 옮깁니다. 정책은 합의 전 초안입니다.
4. 실제 게이트 상태는 기존 `docs/GATES.md`에 계속 기록합니다. 기존 코드·평가셋·workflow는 바꾸지 않습니다.

아이디어·개별 제품 기획안·API 테스트·비밀값·로컬 Git 사본은 포함하지 않습니다.

## 확인할 문서

- [이관 후 최상위 README](payload/README.md)
- [이관 후 AGENTS와 폴더 진입점 목록](payload/AGENTS.md)
- [공통 자료 진입점](payload/docs/project-guide/ROOT_PROJECT_GUIDE.md)
- [팀 운영 정책 초안](payload/docs/TEAM_POLICY.md)

`payload/` 아래에는 **옮길 후보 50개 파일만** 넣었습니다. 아래 도착 경로는 프로젝트 저장소 루트 기준입니다. `수정` 3개는 기존 내용을 보존한 후 안내를 추가하는 전체 파일 후보이며, `신규`는 추가할 파일입니다.

후보 본문의 상대 링크는 이관 후 프로젝트 구조를 기준으로 합니다. 이 미리보기에 없는 기존 코드·평가 문서는 [기준 커밋의 원래 저장소](https://github.com/aihuman-7th/proj3-b/tree/f06acf2221f48e448c54c7254e8b9b4b76612170)에서 확인할 수 있습니다. 실제 적용 전에는 최신 main과 다시 대조합니다.

## 옮길 파일 전체

| 구분 | 프로젝트 도착 경로 / 후보 내용 |
|---|---|
| 신규 | [.gitattributes](payload/.gitattributes) |
| 신규 | [.github/ROOT_GITHUB.md](payload/.github/ROOT_GITHUB.md) |
| 신규 | [.github/workflows/ROOT_GITHUB_WORKFLOWS.md](payload/.github/workflows/ROOT_GITHUB_WORKFLOWS.md) |
| 수정 | [AGENTS.md](payload/AGENTS.md) |
| 수정 | [README.md](payload/README.md) |
| 신규 | [backend/ROOT_BACKEND.md](payload/backend/ROOT_BACKEND.md) |
| 신규 | [backend/graph/ROOT_BACKEND_GRAPH.md](payload/backend/graph/ROOT_BACKEND_GRAPH.md) |
| 신규 | [backend/rag/ROOT_BACKEND_RAG.md](payload/backend/rag/ROOT_BACKEND_RAG.md) |
| 신규 | [backend/services/ROOT_BACKEND_SERVICES.md](payload/backend/services/ROOT_BACKEND_SERVICES.md) |
| 신규 | [data/ROOT_DATA.md](payload/data/ROOT_DATA.md) |
| 신규 | [data/corpus/ROOT_DATA_CORPUS.md](payload/data/corpus/ROOT_DATA_CORPUS.md) |
| 신규 | [db/ROOT_DB.md](payload/db/ROOT_DB.md) |
| 수정 | [docs/GATES.md](payload/docs/GATES.md) |
| 신규 | [docs/ROOT_DOCS.md](payload/docs/ROOT_DOCS.md) |
| 신규 | [docs/TEAM.md](payload/docs/TEAM.md) |
| 신규 | [docs/TEAM_POLICY.md](payload/docs/TEAM_POLICY.md) |
| 신규 | [docs/deliverables/ROOT_DOCS_DELIVERABLES.md](payload/docs/deliverables/ROOT_DOCS_DELIVERABLES.md) |
| 신규 | [docs/project-guide/CHECKPOINT.md](payload/docs/project-guide/CHECKPOINT.md) |
| 신규 | [docs/project-guide/COURSE_COVERAGE.md](payload/docs/project-guide/COURSE_COVERAGE.md) |
| 신규 | [docs/project-guide/COURSE_GUIDE.md](payload/docs/project-guide/COURSE_GUIDE.md) |
| 신규 | [docs/project-guide/COURSE_SOURCES.md](payload/docs/project-guide/COURSE_SOURCES.md) |
| 신규 | [docs/project-guide/DELIVERABLES.md](payload/docs/project-guide/DELIVERABLES.md) |
| 신규 | [docs/project-guide/EVALUATION.md](payload/docs/project-guide/EVALUATION.md) |
| 신규 | [docs/project-guide/EVIDENCE_TEMPLATE.md](payload/docs/project-guide/EVIDENCE_TEMPLATE.md) |
| 신규 | [docs/project-guide/GATE_REQUIREMENTS.md](payload/docs/project-guide/GATE_REQUIREMENTS.md) |
| 신규 | [docs/project-guide/NOTION.md](payload/docs/project-guide/NOTION.md) |
| 신규 | [docs/project-guide/PDF.md](payload/docs/project-guide/PDF.md) |
| 신규 | [docs/project-guide/ROOT_PROJECT_GUIDE.md](payload/docs/project-guide/ROOT_PROJECT_GUIDE.md) |
| 신규 | [docs/project-guide/SCHEDULE.md](payload/docs/project-guide/SCHEDULE.md) |
| 신규 | [docs/project-guide/sources/ROOT_GUIDE_SOURCES.md](payload/docs/project-guide/sources/ROOT_GUIDE_SOURCES.md) |
| 신규 | [docs/project-guide/sources/checkpoint-2026-10-26.html](payload/docs/project-guide/sources/checkpoint-2026-10-26.html) |
| 신규 | [docs/project-guide/sources/checkpoint-guide.txt](payload/docs/project-guide/sources/checkpoint-guide.txt) |
| 신규 | [docs/project-guide/sources/checkpoint-schedule.txt](payload/docs/project-guide/sources/checkpoint-schedule.txt) |
| 신규 | [docs/project-guide/sources/checkpoint-start.txt](payload/docs/project-guide/sources/checkpoint-start.txt) |
| 신규 | [docs/project-guide/sources/checkpoint-teams.txt](payload/docs/project-guide/sources/checkpoint-teams.txt) |
| 신규 | [docs/project-guide/sources/course-materials.json](payload/docs/project-guide/sources/course-materials.json) |
| 신규 | [docs/project-guide/sources/guide-v5.pdf](payload/docs/project-guide/sources/guide-v5.pdf) |
| 신규 | [docs/project-guide/sources/guide-v5.txt](payload/docs/project-guide/sources/guide-v5.txt) |
| 신규 | [docs/project-guide/sources/manifest.json](payload/docs/project-guide/sources/manifest.json) |
| 신규 | [docs/project-guide/sources/notion-2026-10-26.md](payload/docs/project-guide/sources/notion-2026-10-26.md) |
| 신규 | [docs/reference/ROOT_DOCS_REFERENCE.md](payload/docs/reference/ROOT_DOCS_REFERENCE.md) |
| 신규 | [docs/reference/ci-cost-proposal.md](payload/docs/reference/ci-cost-proposal.md) |
| 신규 | [docs/specs/ROOT_DOCS_SPECS.md](payload/docs/specs/ROOT_DOCS_SPECS.md) |
| 신규 | [evals/ROOT_EVALS.md](payload/evals/ROOT_EVALS.md) |
| 신규 | [evals/results/ROOT_EVALS_RESULTS.md](payload/evals/results/ROOT_EVALS_RESULTS.md) |
| 신규 | [extras/ROOT_EXTRAS.md](payload/extras/ROOT_EXTRAS.md) |
| 신규 | [extras/mcp_skill/ROOT_EXTRAS_MCP_SKILL.md](payload/extras/mcp_skill/ROOT_EXTRAS_MCP_SKILL.md) |
| 신규 | [extras/mcp_skill/mcp_server/ROOT_EXTRAS_MCP_SKILL_MCP_SERVER.md](payload/extras/mcp_skill/mcp_server/ROOT_EXTRAS_MCP_SKILL_MCP_SERVER.md) |
| 신규 | [extras/mcp_skill/skills/ROOT_EXTRAS_MCP_SKILL_SKILLS.md](payload/extras/mcp_skill/skills/ROOT_EXTRAS_MCP_SKILL_SKILLS.md) |
| 신규 | [tests/ROOT_TESTS.md](payload/tests/ROOT_TESTS.md) |
