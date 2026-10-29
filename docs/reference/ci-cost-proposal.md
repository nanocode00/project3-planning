# GitHub Actions CI 사용량 절감 정책 추가안

> 기존 `AGENTS.md`의 **GitHub Actions 자원 보호** 정책을 보완한다.\
> 목적은 테스트 품질을 낮추는 것이 아니라, **변경과 관계없는 CI 실행·중복 실행·불필요한 dependency 설치를 제거하여 GitHub Actions 사용량을 최소화하는 것**이다.

---

## 1. 기본 원칙

CI 비용을 줄이기 위해 테스트 자체를 임의로 삭제하거나 회귀 범위를 축소하지 않는다.

대신 다음 순서로 최적화한다.

1. **변경과 무관한 workflow/job을 실행하지 않는다.**
2. **동일 작업의 중복 실행을 취소한다.**
3. **dependency 다운로드와 설치를 cache한다.**
4. **일반 PR CI와 heavy validation을 분리한다.**
5. **필요한 테스트만 필요한 환경에서 실행한다.**
6. 그래도 실행 시간이 과도한 경우에만 테스트 구조·병렬화를 검토한다.

---

## 2. 변경 범위 기반 CI 실행

### 2.1 모든 PR에서 모든 job을 실행하지 않는다

backend와 frontend처럼 독립적으로 검증 가능한 영역은 변경 파일을 기준으로 job 실행 여부를 분리한다.

예:

- `backend/**` 변경 → backend CI 실행
- `frontend/**` 변경 → frontend CI 실행
- 공통 schema/config/API 계약 변경 → 관련 backend/frontend CI 모두 실행
- CI workflow 또는 dependency 파일 변경 → 영향받는 CI 실행
- 문서만 변경 → Product CI 생략

단, 변경 범위를 잘못 분류하면 회귀 검증이 누락될 수 있으므로 **공통 영향 파일은 보수적으로 양쪽 테스트를 실행한다.**

---

## 3. Docs-only 변경 최적화

다음과 같이 제품 동작에 영향을 주지 않는 변경만 포함된 PR에서는 backend 전체 테스트, frontend build 등 무거운 Product CI를 실행하지 않는 것을 원칙으로 한다.

예:

```text
docs/**
*.md
LICENSE
.github/ISSUE_TEMPLATE/**
```

단, 아래 파일은 문서 형식이더라도 CI 또는 제품 동작에 영향을 줄 수 있으므로 단순 docs-only로 취급하지 않는다.

```text
.github/workflows/**
AGENTS.md
requirements*.txt
package.json
package-lock.json
pnpm-lock.yaml
Dockerfile*
docker-compose*.yml
pyproject.toml
```

PR 정책 검사, secret scan 등 **저비용 저장소 보호용 check는 docs-only PR에서도 유지할 수 있다.**

---

## 4. Job 단위 조건 분리

workflow 하나가 backend와 frontend를 모두 포함하더라도 각 job에는 독립적인 실행 조건을 둘 수 있어야 한다.

다음과 같은 구조를 권장한다.

```yaml
jobs:
  changes:
    # 변경 파일 영역 판정

  backend:
    if: backend 변경 또는 공통 영향 파일 변경
    # backend 검증

  frontend:
    if: frontend 변경 또는 공통 영향 파일 변경
    # frontend 검증
```

전체 workflow를 `paths`만으로 제한하면 required check 처리나 복합 변경에서 문제가 생길 수 있으므로, 프로젝트 구조에 따라 **workflow-level filter와 job-level condition 중 적절한 방식을 선택한다.**

---

## 5. 동일 PR 중복 실행 방지

PR에 새 commit이 push되면 이전 commit 기준의 CI는 일반적으로 더 이상 의미가 없다.

따라서 PR 기반 workflow에는 특별한 이유가 없는 한 다음 설정을 사용한다.

```yaml
concurrency:
  group: <workflow-name>-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true
```

주의:

- 서로 다른 PR은 서로 취소하지 않는다.
- HEAD별 결과 보존이 필요한 정책 검사처럼 특수한 경우에는 PR 번호와 HEAD SHA를 함께 사용한다.
- concurrency key는 workflow 목적에 맞게 설계한다.

---

## 6. `push`와 `pull_request` 중복 실행 금지

동일 commit에 대해 `push`와 `pull_request` workflow가 모두 실행되어 같은 테스트가 두 번 수행되지 않는지 확인한다.

예를 들어 feature branch의 PR 검증이 목적이라면 다음과 같이 구성할 수 있다.

```yaml
on:
  pull_request:
```

`push`가 반드시 필요한 경우에는 `main` 등 필요한 branch만 명시한다.

```yaml
on:
  push:
    branches:
      - main

  pull_request:
```

다음과 같은 광범위한 설정은 특별한 이유가 없다면 피한다.

```yaml
on:
  push:
  pull_request:
```

CI workflow 추가·수정 시 **동일 SHA가 몇 번 실행되는지 반드시 확인한다.**

---

## 7. 모든 Job에 Timeout 설정

모든 GitHub-hosted runner job에는 합리적인 `timeout-minutes`를 명시한다.

예:

```yaml
jobs:
  policy-check:
    timeout-minutes: 5

  backend-tests:
    timeout-minutes: 15

  frontend-tests:
    timeout-minutes: 15
```

timeout을 설정하지 않은 job이 외부 서비스 장애, dependency 다운로드 장애, 잘못된 polling 등으로 장시간 runner를 점유하지 않도록 한다.

특히 다음 작업에는 반드시 timeout을 검토한다.

- polling
- 네트워크 요청
- package installation
- Docker build
- integration test
- 외부 API 호출

---

## 8. Dependency 설치 최소화

일반 PR CI에서는 제품 전체 실행 환경을 그대로 설치하지 않는다.

목적에 따라 CI dependency를 분리한다.

예:

```text
requirements-ci.txt
requirements-test.txt
requirements-stt.txt
requirements-tts.txt
requirements-training.txt
```

일반 PR CI에는 다음 dependency를 넣지 않는 것을 원칙으로 한다.

- CUDA runtime
- PyTorch GPU package
- STT/TTS 모델 runtime
- fine-tuning library
- 대형 ML framework
- 사용하지 않는 database/client SDK
- 실제 모델 inference에만 필요한 패키지

단위 테스트에서는 mock, fake, dependency injection을 우선 사용한다.

---

## 9. Dependency Cache 사용

반복 설치가 발생하는 package manager에는 cache 사용을 우선 적용한다.

예:

```yaml
- uses: actions/setup-python@v5
  with:
    cache: pip
    cache-dependency-path: backend/requirements-ci.txt
```

```yaml
- uses: actions/setup-node@v4
  with:
    cache: npm
    cache-dependency-path: frontend/package-lock.json
```

cache key에는 requirements 또는 lockfile 변경이 반영되어야 한다.

단, cache 적중률이 낮거나 cache 저장/복원 시간이 설치 시간보다 긴 경우에는 실제 실행 시간을 측정해 유지 여부를 판단한다.

---

## 10. Heavy Validation 분리

다음 작업은 일반 PR CI에서 자동 실행하지 않는다.

- 실제 STT/TTS inference
- GPU test
- full benchmark
- 모델 다운로드가 필요한 E2E
- fine-tuning
- 대규모 evaluation
- 장시간 Docker image build
- 외부 유료 API를 대량 호출하는 평가

대신 필요에 따라 다음 방식으로 수행한다.

```text
workflow_dispatch
scheduled workflow
dedicated runner
local reproducible experiment
release validation
```

실행 결과는 Worklog 또는 실험 문서에 기록한다.

---

## 11. Workflow 수정 시 비용 영향 검토

다음 파일이 변경된 PR에서는 기능 검토뿐 아니라 **CI 비용 영향도 함께 검토한다.**

```text
.github/workflows/**
requirements*.txt
pyproject.toml
package.json
package-lock.json
pnpm-lock.yaml
Dockerfile*
docker-compose*.yml
```

리뷰 시 최소한 다음을 확인한다.

- 새 dependency가 모든 PR에서 설치되는가?
- 무거운 ML/runtime dependency가 일반 CI로 유입됐는가?
- 새로운 job이 모든 PR에서 실행되는가?
- 기존 job과 검증 내용이 중복되는가?
- `push`와 `pull_request` 양쪽에서 중복 실행되는가?
- cache가 가능한가?
- timeout이 설정되어 있는가?
- concurrency가 필요한가?
- docs-only PR에도 무거운 job이 실행되는가?

---

## 12. Rerun 정책

실패한 CI를 원인 확인 없이 반복 rerun하지 않는다.

### 허용

- GitHub runner 일시 장애
- package registry 일시 장애
- 외부 서비스의 명확한 transient error
- flaky test 여부를 확인하기 위한 제한된 재실행

### 금지

```text
실패
→ 원인 확인 없음
→ rerun
→ 다시 실패
→ rerun
→ 반복
```

dependency 설치나 특정 test가 지속적으로 실패하면 workflow 또는 코드 원인을 수정한 뒤 다시 실행한다.

같은 commit에 대한 반복 rerun은 최소화한다.

---

## 13. Artifact 보존 최소화

CI artifact는 실제 후속 검토에 필요한 경우에만 업로드한다.

대용량 artifact를 기본적으로 장기간 보존하지 않는다.

예:

```yaml
- uses: actions/upload-artifact@v4
  with:
    retention-days: 3
```

다음은 일반 PR artifact로 업로드하지 않는 것을 원칙으로 한다.

- model checkpoint
- training dataset
- 전체 build cache
- 불필요한 `node_modules`
- Python virtualenv
- 대형 benchmark output

---

## 14. Matrix Job 제한

OS, Python, Node, DB 버전을 조합한 matrix는 실행 수를 급격하게 증가시킬 수 있다.

예를 들어

```text
3 OS × 3 Python version × 2 DB
= 18 jobs
```

가 된다.

일반 PR에서는 대표 환경 하나를 기본 gate로 사용하고, 전체 compatibility matrix가 필요한 경우 scheduled/release validation으로 분리하는 것을 우선 검토한다.

---

## 15. PR CI와 Release CI 구분

모든 검증을 PR 단계에서 수행할 필요는 없다.

### PR CI

빠른 피드백을 우선한다.

```text
lint
unit test
schema test
contract test
lightweight integration test
build smoke test
PR policy
secret scan
```

### Release / Heavy CI

실제 배포 직전 또는 별도 실행한다.

```text
full E2E
실제 외부 provider
GPU inference
전체 benchmark
compatibility matrix
대용량 Docker build
full security scan
```

---

## 16. CI 추가 시 완료 조건

새 workflow 또는 job을 추가할 때 다음 조건을 만족해야 한다.

- [ ] 이 CI가 어떤 변경에 필요한지 정의했다.
- [ ] 관련 없는 변경에서는 실행되지 않는다.
- [ ] docs-only 변경 처리 방식을 확인했다.
- [ ] 동일 commit의 중복 trigger가 없다.
- [ ] `concurrency` 필요 여부를 검토했다.
- [ ] `cancel-in-progress` 필요 여부를 검토했다.
- [ ] `timeout-minutes`를 설정했다.
- [ ] dependency cache를 검토했다.
- [ ] 무거운 ML/runtime dependency를 설치하지 않는다.
- [ ] 실제 모델/GPU/full benchmark를 일반 PR gate에 넣지 않았다.
- [ ] artifact 업로드가 필요한 경우 보존 기간을 최소화했다.
- [ ] 기존 CI와 동일한 테스트를 중복 실행하지 않는지 확인했다.
- [ ] workflow 변경 후 실제 Actions 실행 시간과 job 구성을 확인했다.

---

## 17. 권장 우선순위

CI 사용량이 높아졌을 때는 다음 순서로 개선한다.

1. **path/change filtering**
2. **docs-only skip**
3. **backend/frontend 등 독립 job 선택 실행**
4. **중복 `push` / `pull_request` 제거**
5. **`concurrency` + `cancel-in-progress`**
6. **dependency cache**
7. **CI 전용 lightweight requirements**
8. **timeout 설정**
9. **heavy validation 분리**
10. **artifact / matrix 최소화**
11. 테스트 구조·병렬화 최적화

테스트를 단순히 삭제하는 것은 마지막 수단으로도 권장하지 않는다.\
CI 최적화의 목표는 **검증 품질을 유지하면서 불필요한 runner 사용을 제거하는 것**이다.
