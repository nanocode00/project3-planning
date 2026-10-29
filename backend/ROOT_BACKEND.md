# backend 탐색 안내

## 폴더 탐색

상위: [상위 안내](../README.md) · 전체 규칙: [AGENTS.md](../AGENTS.md)

역할: FastAPI 진입점·인증·기억과 그래프/검색/LLM 모듈 연결.

| 하위 폴더 | 진입 문서 |
|---|---|
| `graph/` | [그래프 상태·Supervisor·서브에이전트·승인/재개 설계 자리](graph/ROOT_BACKEND_GRAPH.md) |
| `rag/` | [코퍼스 적재·검색·리랭킹 설계 자리](rag/ROOT_BACKEND_RAG.md) |
| `services/` | [LLM 호출·데모 응답·관측을 모으는 서비스 경계](services/ROOT_BACKEND_SERVICES.md) |

| 직접 확인할 파일 | 용도 |
|---|---|
| [auth.py](https://github.com/aihuman-7th/proj3-b/blob/f06acf2221f48e448c54c7254e8b9b4b76612170/backend/auth.py) | 사용자 식별 구현 자리 |
| [main.py](https://github.com/aihuman-7th/proj3-b/blob/f06acf2221f48e448c54c7254e8b9b4b76612170/backend/main.py) | FastAPI 진입점과 /health |
| [memory.py](https://github.com/aihuman-7th/proj3-b/blob/f06acf2221f48e448c54c7254e8b9b4b76612170/backend/memory.py) | 사용자별 기억 namespace |
| [requirements-dev.txt](https://github.com/aihuman-7th/proj3-b/blob/f06acf2221f48e448c54c7254e8b9b4b76612170/backend/requirements-dev.txt) | 이 폴더의 자료·설정 |
| [requirements.txt](https://github.com/aihuman-7th/proj3-b/blob/f06acf2221f48e448c54c7254e8b9b4b76612170/backend/requirements.txt) | 이 폴더의 자료·설정 |

이 안내에서 필요한 하위 경로만 선택해 읽습니다. 폴더/파일의 존재는 구현·검증 완료를 뜻하지 않습니다.
