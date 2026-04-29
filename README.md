# nvwriter

Claude Code용 소설 집필 플러그인. 아이디어 구상부터 원고 집필·검토·수정까지 전 과정을 커맨드로 지원합니다.

## 설치

```bash
claude plugin install github:js/nvwriter
```

## 커맨드

| 커맨드 | 설명 |
|--------|------|
| `/init` | 새 소설 프로젝트 초기화 (`docs/`, `src/` 구조 생성) |
| `/bs` | 브레인스토밍 — 캐릭터·세계관·플롯 아이디어 구상 |
| `/wiki` | 설정 문서 작성 — 인물, 세계관, 사건 정리 |
| `/plan` | 챕터 계획서 작성 |
| `/write` | 원고 집필 |
| `/style` | 문체 분석 및 스타일 프로필 적용 |
| `/review` | 챕터 품질 검토 (A/B/C 판정) |
| `/revise` | 검토 주석 기반 수정 |

## 사용법

새 프로젝트 폴더에서 시작:

```
/init
```

이후 권장 워크플로우:

```
/init → /bs → /wiki → /plan → /write → /review → /revise
```

## 요구사항

- Claude Code CLI
