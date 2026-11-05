<h1 align="center">$\bf{\large{\color{#6580DD} Clonamic }}$</h1>

**Claude Code, Codex, Cursor, Grok에서 쓰는 AI 에이전트 스킬·플러그인 모음입니다.**

에이전트에게 일을 맡길 때 반복되는 문제(시키지 않은 일, 확인 없는 완료 보고, 번역투 한국어)를 줄이려고 만들었습니다. 지금 쓰는 에이전트 도구에 설치해 채팅이나 슬래시 명령으로 부르면 됩니다.

![Claude Code](https://img.shields.io/badge/Claude_Code-D97757?style=for-the-badge&logo=claudecode&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-000000?style=for-the-badge)
![Cursor](https://img.shields.io/badge/Cursor-000000?style=for-the-badge&logo=cursor&logoColor=white)
![Grok](https://img.shields.io/badge/Grok-000000?style=for-the-badge&logo=x&logoColor=white)

<hr>

## 설계 철학

- **플러그인 하나는 역할 하나, 스킬 하나는 일 하나.** 기능이 겹치면 합칩니다.
- **호스트가 할 수 있는 일은 다시 만들지 않습니다.** 셸, 파일, 브라우저, 서브에이전트는 에이전트 도구가 직접 씁니다. 스크립트는 모델이 손으로 하는 것보다 확실히 나을 때만 둡니다.
- **라우터 스킬을 만들지 않습니다.** 어떤 스킬을 쓸지는 호스트가 설명을 보고 고릅니다.
- **사람이 읽는 결과는 한국어로 씁니다.** 보고서, 명세서, 예시는 한국어로, 에이전트 사이의 내부 절차는 영어로 씁니다.
- **서브에이전트는 따로 떨어진 컨텍스트가 결과를 낫게 할 때만 씁니다.** 작업자의 설명에 끌려가지 않는 독립 검토가 대표적입니다.

<hr>

## 구성

| 층 | 하는 일 | 저장소 |
|---|---|---|
| 스킬 | 일 하나를 하는 방법과 양식 | [skills](https://github.com/clonamic/skills) |
| 플러그인 | 한 역할을 위한 스킬 묶음. 필요하면 코드와 서브에이전트를 함께 담는 배포 단위 | [plugin](https://github.com/clonamic/plugin) |
| 서브에이전트 | 독립된 컨텍스트에서 정해진 절차를 실행하는 일꾼 | [subagents](https://github.com/clonamic/subagents) |

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
