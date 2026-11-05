<h1 align="center">$\bf{\large{\color{#6580DD} Clonamic }}$</h1>

**Claude Code, Codex, Cursor, Grok, Antigravity에서 쓰는 AI 에이전트 스킬·플러그인 모음입니다.**

에이전트에게 일을 맡길 때 반복되는 문제(시키지 않은 일, 확인 없는 완료 보고, 번역투 한국어)를 줄이려고 만들었습니다. 지금 쓰는 에이전트 도구에 설치해 채팅이나 슬래시 명령으로 부르면 됩니다. 모든 플러그인의 바탕은 [clonamic-harness](https://github.com/clonamic/plugin/tree/main/clonamic-harness)입니다.

![Claude Code](https://img.shields.io/badge/Claude_Code-D97757?style=for-the-badge&logo=claudecode&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-000000?style=for-the-badge)
![Cursor](https://img.shields.io/badge/Cursor-000000?style=for-the-badge&logo=cursor&logoColor=white)
![Grok](https://img.shields.io/badge/Grok-000000?style=for-the-badge&logo=x&logoColor=white)
![Antigravity](https://img.shields.io/badge/Antigravity-4285F4?style=for-the-badge&logo=google&logoColor=white)

<hr>

## 핵심: clonamic-harness

모든 플러그인은 이 하네스에서 출발했습니다. AI 에이전트에게 일을 맡기면 혼자 판단해 과하게 만들고, 끝나지 않은 일을 끝났다고 하고, 범위 밖의 사용자 환경을 바꾼 채 되돌리지 않는 일이 생깁니다. 하네스는 AI의 성능은 그대로 두고, 넘으면 안 되는 선만 정합니다.

### 작업 흐름

| 단계 | 하는 일 |
|---|---|
| 이해 | 요청을 항목별로 나누고, 지금 상황에 맞는 의도로 다시 씁니다. 시키지 않은 일과 과한 설계를 미리 걸러 냅니다 |
| 승인 | 읽기는 자유, 쓰기는 승인 후. 요청이 애매하면 작업명세서, 바꾸기 전에는 개발명세서로 승인받습니다(최대 두 번) |
| 실행 | 가장 작은 변경으로 진행하고, 승인한 범위 안에서는 고치고 다시 확인하는 반복을 묻지 않고 계속합니다 |
| 마무리 | 마지막 변경 뒤 새 증거로 진짜 끝났는지 다시 확인하고, 아니면 끝까지 마친 뒤 결론부터 보고합니다 |

### 원칙

- **사용자 지시가 1순위입니다.** 하네스는 지시를 막지 않고, 범위를 넘지 않게만 합니다.
- **읽기는 먼저, 많이.** 잘못 쓰는 것보다 읽는 비용이 적어서, 명세서를 쓰기 전에 현재 동작을 충분히 파악합니다.
- **승인은 짧게.** 명세서 맨 위에 내 프롬프트와 해석을 함께 보여 주고, `승인:코드` 한 줄로 승인합니다.
- **오버엔지니어링을 막습니다.** 불필요한 일 빼기, 기존 코드 재사용, 표준 기능 순으로 가장 작은 변경을 고릅니다.
- **사용자 환경을 바꾸지 않습니다.** 범위 밖 변경과 되돌리지 않은 자동화를 가장 큰 실패로 봅니다.
- **확인하지 않은 것은 말하지 않습니다.** 완료는 새 증거로만 판단하고, 확인 못 한 건 미검증으로 따로 적습니다.
- **보고는 두괄식 목록으로.** 결과와 실패를 먼저, 문단 대신 짧은 목록으로 씁니다.
- **도구가 바뀌어도 그대로.** 어떤 에이전트든 `AGENTS.md` 하나만 가리키면 같은 규칙이 적용됩니다.

<hr>

## 플러그인 원칙

- **플러그인 하나는 역할 하나, 스킬 하나는 일 하나.** 기능이 겹치면 합칩니다.
- **호스트가 할 수 있는 일은 다시 만들지 않습니다.** 셸, 파일, 브라우저, 서브에이전트는 에이전트 도구가 직접 씁니다. 스크립트는 모델이 손으로 하는 것보다 확실히 나을 때만 둡니다.
- **라우터 스킬을 만들지 않습니다.** 어떤 스킬을 쓸지는 호스트가 설명을 보고 고릅니다.
- **사람이 읽는 결과는 한국어로 씁니다.** 보고서, 명세서, 예시는 한국어로, 에이전트 사이의 내부 절차는 영어로 씁니다.
- **서브에이전트는 따로 떨어진 컨텍스트가 결과를 낫게 할 때만 씁니다.** 작업자의 설명에 끌려가지 않는 독립 검토가 대표적입니다.

<hr>

## 구성

| 층 | 저장소 |
|---|---|
| 스킬 | [skills](https://github.com/clonamic/skills) |
| 플러그인 | [plugin](https://github.com/clonamic/plugin) |
| 서브에이전트 | [subagents](https://github.com/clonamic/subagents) |

<hr>

## 규격

### 플러그인

[Agent Plugins 1.0.0](https://agent-plugins.org) 표준을 따릅니다.

- 원본 매니페스트 `plugin.json` 하나만 손으로 고치고, 도구별 사본(`.claude-plugin/`, `.codex-plugin/`, `.cursor-plugin/`)에 이름·버전·설명을 같게 맞춥니다.
- 이름은 영문 소문자, 숫자, 하이픈만 쓰고, 표준 스키마에 없는 키는 넣지 않습니다.
- 서브에이전트(`agents/`)가 없어도 스킬만으로 동작해야 합니다.

코드를 쓰는 플러그인은 아래 구조와 기준을 따릅니다.

```
my-plugin/
├── plugin.json                  # 원본 매니페스트
├── .claude-plugin/plugin.json   # Claude Code 사본
├── .codex-plugin/plugin.json    # Codex 사본
├── pyproject.toml               # Python 3.12 이상. 외부 의존성이 있을 때만
├── src/my_plugin/
│   ├── __main__.py              # 진입점 하나. python3 -m my_plugin <명령>
│   ├── commands/                # 명령 하나 = 스킬 하나
│   └── core/                    # 공통 로직과 스킬 사이 계약
├── skills/
│   └── skill-name/SKILL.md      # 명령 한 줄로 호출
├── agents/                      # 선택. 서브에이전트
├── tests/                       # 선택. 로컬 시험, 배포에서 제외
├── README.md
└── LICENSE
```

| 기준 | 내용 |
|---|---|
| Python | 3.12 이상. 낮은 버전으로 조용히 넘어가지 않습니다 |
| 한 스킬만 쓰는 코드 | 그 스킬의 `scripts/`에 표준 라이브러리만으로 둡니다 |
| 여러 스킬이 쓰는 코드 | 플러그인 루트 `src/` 패키지에 두고, 진입점 하나(`python3 -m`)에서 명령 하나가 스킬 하나를 맡습니다 |
| 외부 패키지 | 스크립트 머리에 PEP 723 메타데이터를 적고 `uv run`으로 실행합니다 |
| 두지 않는 것 | 플러그인 폴더 안의 `.venv`, `requirements.txt`, `.python-version` |

### 스킬

- 진입점은 `SKILL.md`입니다. 에이전트는 이름과 설명만 먼저 보고, 필요할 때 본문을 읽습니다.
- 앞부분 필수 항목은 `name`, `description` 두 개입니다. `description`은 1024자 이내로 하는 일과 쓰는 때를 함께 적습니다.
- 폴더 이름은 `name`과 같고, 소문자·숫자·하이픈으로 64자 이하입니다.
- 본문은 목적, 절차, 산출물, 예 순서로 보통 80~200줄, 길어도 500줄을 넘기지 않습니다. 세부 자료는 `references/`로 옮깁니다.
- 슬래시 명령으로만 켜야 하는 스킬은 `disable-model-invocation: true`를 넣습니다.

```
skill-name/
├── SKILL.md       # 필수. 이름, 설명, 절차
├── scripts/       # 선택. 반복해서 쓰는 코드
├── references/    # 선택. 특정 경우에만 읽는 문서
└── assets/        # 선택. 결과물에 넣는 템플릿
```

### 명세서

clonamic-harness가 쓰는 승인 양식입니다. 채팅에만 쓰고 파일로 남기지 않습니다.

| 양식 | 쓰는 때 | 담는 내용 |
|---|---|---|
| 작업명세서 | 요청이 아직 애매할 때만 | 프롬프트와 해석, 요구, 방법, 이유, 산출, 완료, 제외 |
| 개발명세서 | 파일이나 코드를 바꾸기 전 | 변경, 이유, 영향, 검증, 복구 |

요청이 분명하면 개발명세서 하나로 끝나고, 질문이나 검토는 승인 없이 바로 처리합니다.

<hr>

## 플러그인

| 플러그인 | 소개 |
|:--|:--|
| [clonamic-harness](https://github.com/clonamic/plugin/tree/main/clonamic-harness) | 요청 해석, 승인, 실행, 증거 보고 순서를 지키게 하는 작업 규칙. 승인은 작업당 최대 두 번 **(핵심)** |
| [clonamic-code](https://github.com/clonamic/plugin/tree/main/clonamic-code) | 코드를 쓰기 전에 나눈 근거가 남는 모듈 구조를 설계 |
| [clonamic-create-agent](https://github.com/clonamic/plugin/tree/main/clonamic-create-agent) | 에이전트 파이프라인을 그래프로 설계하고 한국어 설계 문서로 내보냄 |
| [clonamic-executor](https://github.com/clonamic/plugin/tree/main/clonamic-executor) | 다른 에이전트 CLI(Claude, Grok, Hermes)에 읽기 전용 질문을 한 번 보내고 답을 받음 |
| [clonamic-memory](https://github.com/clonamic/plugin/tree/main/clonamic-memory) | 요청할 때만 내 컴퓨터에 저장하고 꺼내 쓰는 메모 |
| [clonamic-korean](https://github.com/clonamic/plugin/tree/main/clonamic-korean) | 사실과 말투는 그대로 두고 어색한 한국어만 다듬음 |
| [clonamic-task-log](https://github.com/clonamic/plugin/tree/main/clonamic-task-log) | 그날의 git 작업을 포트폴리오용 한국어 작업 기록으로 정리 |
| [clonamic-documents](https://github.com/clonamic/plugin/tree/main/clonamic-documents) | 한글(HWPX) 문서와 PowerPoint 발표 자료를 만들고 고침 |
| [clonamic-ai-html-ppt](https://github.com/clonamic/plugin/tree/main/clonamic-ai-html-ppt) | 사람과 AI가 요소 단위로 함께 고치는 로컬 슬라이드·문서 편집기 |
| [clonamic-design](https://github.com/clonamic/plugin/tree/main/clonamic-design) | 화면 디자인 방향, 디자인 검토, 실제 브라우저 점검, 손그림 다이어그램 |
| [clonamic-data](https://github.com/clonamic/plugin/tree/main/clonamic-data) | 데이터셋 점검과 검증, Hugging Face 업로드 |

<hr>

## 설치

```bash
# Claude Code
/plugin marketplace add clonamic/plugin
/plugin install clonamic-harness@clonamic

# Codex
codex plugin marketplace add clonamic/plugin
codex plugin add clonamic-harness@clonamic

# Grok
grok plugin install https://github.com/clonamic/plugin#clonamic-harness --trust
```

`clonamic-harness` 자리에 원하는 플러그인 이름을 넣으면 됩니다. 플러그인마다 자세한 사용법은 각 폴더의 README에 있습니다.
