<h1 align="center">$\bf{\large{\color{#6580DD} Clonamic }}$</h1>

**Claude Code, Codex, Cursor, Grok, Antigravity에서 쓰는 AI 에이전트 스킬·플러그인 모음입니다.**

에이전트에게 일을 맡길 때 반복되는 문제(시키지 않은 일, 확인 없는 완료 보고, 번역투 한국어)를 줄이려고 만들었습니다. 지금 쓰는 에이전트 도구에 설치해 채팅이나 슬래시 명령으로 부르면 됩니다.

![Claude Code](https://img.shields.io/badge/Claude_Code-D97757?style=for-the-badge&logo=claudecode&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-000000?style=for-the-badge)
![Cursor](https://img.shields.io/badge/Cursor-000000?style=for-the-badge&logo=cursor&logoColor=white)
![Grok](https://img.shields.io/badge/Grok-000000?style=for-the-badge&logo=x&logoColor=white)
![Antigravity](https://img.shields.io/badge/Antigravity-4285F4?style=for-the-badge&logo=google&logoColor=white)

<hr>

## 설계 철학

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

```
plugin-name/
├── plugin.json                  # 원본 매니페스트
├── .claude-plugin/plugin.json   # Claude Code 사본
├── .codex-plugin/plugin.json    # Codex 사본
├── skills/<skill-name>/SKILL.md
├── agents/                      # 선택. 서브에이전트
├── mcp.json                     # 선택. MCP 서버
├── README.md
└── LICENSE
```

코드를 쓰는 플러그인은 아래 기준을 지킵니다.

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

<hr>

## 플러그인

| 플러그인 | 소개 |
|:--|:--|
| [clonamic-harness](https://github.com/clonamic/plugin/tree/main/clonamic-harness) | 요청 해석, 승인, 실행, 증거 보고 순서를 지키게 하는 작업 규칙. 승인은 작업당 최대 두 번 |
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
