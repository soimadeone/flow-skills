[English](README.md) · 한국어

# flow-skills

기능을 아이디어에서 리뷰된 코드까지 가져가는 에이전트 스킬 모음입니다. 스펙을 쓰고, 작업을 계획하고, 구현하고, 다듬고, 리뷰합니다. 그 밖에 도식을 그리는 스킬과 여러 출처를 조사하는 스킬이 따로 있습니다.

평범한 `SKILL.md` 폴더라서 Claude Code, Codex 등 Agent Skills 형식을 지원하는 에이전트에서 똑같이 동작합니다.

## 설치

```bash
# 전체 설치
npx skills add soimadeone/flow-skills

# 일부만 설치
npx skills add soimadeone/flow-skills --skill flow-spec flow-plan flow-build

# 현재 프로젝트가 아니라 사용자 전체에 설치
npx skills add soimadeone/flow-skills -g
```

전체 설치를 권장합니다. 단계마다 다음 스킬로 넘기고, `flow-spec` 은 스펙 안 도식을 `flow-diagram` 으로 그립니다.

설치하지 않고 들어 있는 스킬만 보기:

```bash
npx skills add soimadeone/flow-skills --list
```

나중에 `npx skills update` 로 업데이트합니다.

## 흐름

```
flow-spec  →  flow-plan  →  flow-build  →  flow-review
                                 ↑               │
                                 └── 수정 ───────┘
```

단계가 끝날 때마다 에이전트가 계속할지, 고칠지, 멈출지 묻고, 계속하라고 하면 같은 세션에서 다음 스킬로 넘어갑니다.

| 스킬 | 하는 일 |
|---|---|
| `flow-spec` | 기능 요청을 범위와 완료 기준이 있는 스펙으로 만들고, 빠졌거나 예상 밖인 기능을 함께 점검하며, UI 가 있는 기능이면 검토용 목업을 만듭니다. |
| `flow-plan` | 스펙을 순서가 정해진 태스크로 나누고 인수 기준과 검증 방법을 붙입니다. 제품 코드는 건드리지 않습니다. |
| `flow-build` | 다음 태스크를 구현하고 필요한 검사를 돌립니다. `flow-build auto` 는 중간 확인 없이 계획 전체를 끝냅니다. |
| `flow-review` | diff, 커밋, 파일을 정확성·가독성·구조·보안·성능 기준으로 근거와 함께 리뷰합니다. 코드는 고치지 않습니다. |
| `flow-refine` | 동작은 그대로 두고 이름, 흐름, 책임, 중복을 정리합니다. |
| `flow-diagram` | 실제 코드와 설정을 근거로 아키텍처, 데이터 흐름, 시퀀스, 상태 도식을 HTML+SVG 파일 하나로 그립니다. |
| `flow-research` | 질문을 확인 가능한 주장으로 바꾸고, 주장마다 지지·반대 출처를 찾아 등급을 매긴 뒤, 판정마다 출처가 달린 보고서를 씁니다. |

## 사용법

스킬 이름을 부르고 원하는 것을 적습니다.

```text
# Claude Code
/flow-spec 이메일로 비밀번호 재설정
/flow-build auto

# Codex
$flow-plan
$flow-review HEAD~3..HEAD
```

자연어로 불러도 됩니다: `flow-diagram 로그인부터 세션 저장까지 데이터 흐름 그려줘`.

답과 문서는 사용자가 쓰는 언어를 따릅니다.

## 결과물 위치

| 스킬 | 기본 위치 |
|---|---|
| `flow-spec` | 프로젝트 루트의 `SPEC.md`, 또는 프로젝트에 이미 있는 스펙 위치 |
| `flow-plan` | `tasks/plan.md` 와 `tasks/todo.md` |
| `flow-diagram` | `docs/diagrams/<주제>.html` |
| `flow-research` | `research/<주제>/report.md`, 옆에 `brief.md`·`sources.md`·`claims.md` |

각 스킬은 저장소의 `AGENTS.md`(있으면 `CLAUDE.md` 도)를 먼저 읽고, 기본값보다 프로젝트 관례를 따릅니다.
