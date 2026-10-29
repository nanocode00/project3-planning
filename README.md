# 3차 프로젝트 공통 문서 이관 준비

**이 저장소 루트에서 프로젝트에 옮길 문서 구조를 바로 검토할 수 있습니다.** 실제 대상은 [2조 프로젝트 저장소](https://github.com/aihuman-7th/proj3-b)이며, 해당 저장소에 적용하거나 PR을 생성하지 않았습니다. 아이디어·개별 기획안·API 실험·비밀값은 제외했습니다.

기준 구조는 강사님이 제공한 `proj3-b`의 `f06acf2221f48e448c54c7254e8b9b4b76612170`입니다. 이곳에는 이관 후보 문서만 두며, 기존 제품 코드·평가셋·workflow를 복제하지 않습니다.

## 어디부터 볼까

- [AGENTS.md](AGENTS.md): 최상위 규칙과 22개 폴더의 `ROOT_*.md` 탐색 경로
- [공통 안내](docs/project-guide/ROOT_PROJECT_GUIDE.md): 요구조건·일정·평가·수업자료 대응
- [팀 운영 정책](docs/TEAM_POLICY.md): 개발·리뷰·CI 정책 합의 전 초안
- [이관 계획과 전체 파일 목록](MIGRATION.md): 무엇을 어디로 옮기는지

## 루트에서 보는 구조

| 폴더 | 역할·하위 문서 안내 |
|---|---|
| `.github/` | [ROOT_GITHUB.md](.github/ROOT_GITHUB.md) |
| `backend/` | [ROOT_BACKEND.md](backend/ROOT_BACKEND.md) |
| `data/` | [ROOT_DATA.md](data/ROOT_DATA.md) |
| `db/` | [ROOT_DB.md](db/ROOT_DB.md) |
| `docs/` | [ROOT_DOCS.md](docs/ROOT_DOCS.md) |
| `evals/` | [ROOT_EVALS.md](evals/ROOT_EVALS.md) |
| `extras/` | [ROOT_EXTRAS.md](extras/ROOT_EXTRAS.md) |
| `tests/` | [ROOT_TESTS.md](tests/ROOT_TESTS.md) |

폴더별 안내 → 필요한 하위 안내 → 관련 문서/코드 순서로 탐색합니다. 문서의 역할·숫자·계약을 여러 파일에 중복 관리하지 않습니다. 실제 게이트 상태는 프로젝트의 `docs/GATES.md`에서 관리하고, [상세 조건](docs/project-guide/GATE_REQUIREMENTS.md)은 참고표로 사용합니다.

이 저장소에 없는 기존 프로젝트 파일은 기준 커밋의 원격 파일로 연결했습니다. 링크의 원격 경로는 실제 이관 시 프로젝트 내부 상대 경로로 유지하면 됩니다. 루트 README는 이 준비 저장소 설명이며, 프로젝트 README 후보는 로컬 준비본에 별도로 보존했습니다.
