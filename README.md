# nvwriter

![Version](https://img.shields.io/badge/Version-1.0.4-blue)
![Tool](https://img.shields.io/badge/Tool-Claude_Code-blueviolet)
![Type](https://img.shields.io/badge/Type-Plugin-green)

Claude Code용 소설 집필 플러그인입니다. 소프트웨어 개발 방법론(이슈 트래킹, 문서화, 버전 관리)을 소설 집필에 적용한 워크플로우를 8개 커맨드로 제공합니다.

---

## 설치

```bash
# Claude Code 내에서 실행
/plugin marketplace add nnopia/nvwriter-plugin
/plugin install nvwriter@nvwriter-plugin
```

또는 수동 설치:

```bash
git clone https://github.com/nnopia/nvwriter-plugin
cp nvwriter-plugin/skills/*/SKILL.md ~/.claude/commands/
```

---

## 전체 워크플로우

```
[Phase 1]       [Phase 2]       [Phase 3]     [Phase 4]     [Phase 5]     [Phase 6]
아이디어 발산 → 기획 확정    → 챕터 설계  → 집필        → 검토        → 수정
docs/idea/      docs/TODO.md    docs/plans/   src/           /review       /revise
/bs             docs/events/    /plan         /write         ↓             ↓
                docs/           ↓                            A.통과→완료   주석 처리
                characters/     씬·감정선                    B.→ /revise   후 재검토
                /wiki           복선 설계                    C.→ /revise
```

---

## /init 이 생성하는 디렉터리 구조

```
your-novel/
├── src/                       # 소설 원고
│   └── TOC.md                 # 목차 — 챕터별 집필 진행 상태
│
└── docs/                      # 설정·기획 문서
    ├── overview.md            # 전체 파일 네비게이션 허브
    ├── synopsis.md            # 전체 줄거리·3막 구조 (살아있는 문서)
    ├── TODO.md                # 집필 확정 아이디어·씬 목록
    ├── issues.md              # 미결 설정·플롯 구멍·기술 부채
    ├── idea/                  # 브레인스토밍 아이디어 보관소
    │   ├── _template.md
    │   ├── character/
    │   ├── setting/
    │   ├── story/
    │   └── theme/
    ├── characters/            # 인물 설정집
    ├── world/                 # 세계관 문서
    │   ├── setting.md
    │   └── rules.md
    ├── events/                # 사건 설정 레퍼런스
    └── plans/                 # 챕터별 씬 설계 계획서
```

> 파일을 찾을 때는 항상 `docs/overview.md`를 먼저 확인하세요.

---

## Phase별 사용법

<details>
<summary><strong>Phase 1 — 아이디어 발산</strong> (docs/idea/)</summary>

> 아직 소설에 쓸지 모르는 아이디어를 자유롭게 쌓는 단계입니다.

**저장 위치:** `docs/idea/{타입}/{주제명}.md`

| 타입 폴더    | 담는 내용                 |
| ------------ | ------------------------- |
| `character/` | 인물, 관계, 캐릭터 아크   |
| `setting/`   | 장소, 시대, 분위기        |
| `story/`     | 플롯, 씬, 사건, 갈등 구조 |
| `theme/`     | 주제, 모티프, 상징        |

**파일 형식:** `docs/idea/_template.md` 양식을 복사해서 작성합니다.

**상태 흐름:**

```
구상 중  →  검토 가능  →  반영 완료
```

**반영 결정 시:** 아이디어 타입에 따라 목적지가 다릅니다.

| 타입 | 목적지 | 다음 단계 |
| ---- | ------ | --------- |
| `character` | `docs/characters/{이름}.md` | `/wiki` |
| `setting` | `docs/world/setting.md` 또는 `rules.md` | `/wiki` |
| `story` | `docs/TODO.md` | `/plan` |
| `theme` | `docs/synopsis.md` 주요 테마 | — |

**추천 명령:** `/bs` — 아이디어·플롯·캐릭터 브레인스토밍

</details>

<details>
<summary><strong>Phase 2 — 기획 확정</strong> (docs/)</summary>

> 소설에 쓰기로 결정한 요소들을 구체화하는 단계입니다.

### 설정 문서 작성

| 문서           | 위치                                           |
| -------------- | ---------------------------------------------- |
| 인물 설정      | `docs/characters/{이름}.md`                    |
| 배경·세계 법칙 | `docs/world/setting.md`, `docs/world/rules.md` |
| 사건 정의      | `docs/events/{사건명}.md`                      |
| 전체 줄거리    | `docs/synopsis.md`                             |

### events/ vs plans/ 구분

|      | `docs/events/`                    | `docs/plans/`                   |
| ---- | --------------------------------- | ------------------------------- |
| 질문 | 소설 세계에서 무슨 일이 일어났나? | 그걸 어떻게 글로 쓸 것인가?     |
| 내용 | 언제·어디서·인과관계·영향         | 씬 순서·시점·감정선·복선        |
| 성격 | 레퍼런스 문서 (계속 참조)         | 작업 문서 (집필 후 참조 줄어듦) |

### issues.md 등급

| 등급        | 의미                | 처리                  |
| ----------- | ------------------- | --------------------- |
| 🔴 blocker  | 해당 챕터 집필 불가 | 집필 전 반드시 해결   |
| 🟡 normal   | 집필은 가능         | 다음 챕터 전까지 해결 |
| 🟢 deferred | 나중에 해결 가능    | 자유롭게 쌓아두기     |

**추천 명령:** `/wiki` — 설정집·세계관·사건 문서 작성

</details>

<details>
<summary><strong>Phase 3 — 챕터 설계</strong> (docs/plans/)</summary>

> 집필 전 챕터의 씬 순서·감정선·복선을 설계하는 단계입니다.

계획서 위치: `docs/plans/plan_ch{번호}.md`

| 항목       | 내용                              |
| ---------- | --------------------------------- |
| 씬 목록    | 순서·씬 요약·목적·시점 인물       |
| 감정선     | 챕터 시작·전환점·끝의 감정        |
| 복선       | 설치할 복선 / 회수할 기존 복선    |
| 묘사 포인트 | 공들여야 할 장면·감각·분위기 메모 |

계획서가 없어도 집필은 가능하지만, 구조 문제(C 판정)를 줄이려면 미리 설계하는 것을 권장합니다.

**추천 명령:** `/plan` — 챕터 씬·감정선·복선 설계

</details>

<details>
<summary><strong>Phase 4 — 집필</strong> (src/)</summary>

> 계획서를 바탕으로 원고를 작성하는 단계입니다.

### 네이밍 규칙

```
src/
├── part01_{파트제목}/
│   ├── ch01_{챕터제목}.md
│   └── ch02_{챕터제목}.md
└── part02_{파트제목}/
    └── ch03_{챕터제목}.md
```

- 파트 번호: `01`, `02` … (두 자리 패딩)
- 챕터 번호: 파트 구분 없이 전체 통번호

### 집필 전 체크리스트

- [ ] `docs/issues.md` 🔴 blocker 이슈 없는지 확인
- [ ] `docs/plans/plan_ch{번호}.md` 씬 설계 확인
- [ ] 관련 `docs/characters/`, `docs/world/`, `docs/events/` 문서 확인
- [ ] `docs/TODO.md` 집필 확정 아이디어 확인

### TOC 상태값

```
미집필  →  집필 중  →  초고 완료  →  검토 완료
```

**추천 명령:** `/write` — 장면·챕터·대화 집필

</details>

<details>
<summary><strong>Phase 5 — 검토</strong> (/review)</summary>

> 완성된 챕터를 점검하고 결과에 따라 세 가지 경로로 분기합니다.

```
/review 요청
    │
    ├─ A. 통과
    │      └─ TOC 상태 → "검토 완료"
    │         synopsis.md 갱신
    │
    ├─ B. 일부 수정 (문장·감정·묘사)
    │      └─ 챕터에 <!-- [B] 문제 내용 → --> 주석 삽입 → /revise로 처리
    │
    └─ C. 구조 문제 (씬 순서·인물 동기·복선)
           └─ 챕터에 <!-- [C] 문제 내용 → --> 주석 삽입 → /revise로 처리
              (plans/ 계획서 재설계 후 재집필)
```

> **수정 방향 직접 지정:** `→ ` 뒤에 원하는 방향을 바로 입력하면 `/revise`가 해당 방향으로 수정합니다.
> ```
> <!-- [B] 감정 묘사가 약하다 → 회상 장면으로 대체해줘 -->
> ```
> 비워두면 `/revise`가 설정 문서를 참조해 자체 판단합니다.

### B vs C 판단 기준

| 증상                       | 경로 |
| -------------------------- | ---- |
| 문장·표현이 어색하다       | B    |
| 감정 묘사가 약하다         | B    |
| 문체가 스타일 프로필과 불일치한다 | B |
| 챕터 내 문체 일관성이 흔들린다 | B |
| 씬 순서가 이상하다         | C    |
| 인물 행동에 납득이 안 된다 | C    |
| 복선이 누락됐다            | C    |
| 설정 문서와 충돌한다       | C    |

</details>

<details>
<summary><strong>Phase 6 — 수정</strong> (/revise)</summary>

> 검토에서 삽입된 주석을 하나씩 해결하는 단계입니다.

**처리 순서:** `[C]` 먼저, `[B]` 나중. 같은 등급이면 파일 위에서 아래 순서로 처리합니다.

| 등급 | 처리 방식 |
| ---- | --------- |
| `[C]` 구조 문제 | 계획서 재설계 → 원고 재집필 |
| `[B]` 문장·감정 | 해당 부분 직접 수정 |

**처리 방식 선택:** 전체 이슈 목록을 먼저 확인한 뒤 선택합니다.

| 모드 | `→` 채워짐 | `→` 비어있음 |
| ---- | ---------- | ------------ |
| 하나씩 | 자동 수정 | 사용자 입력 요청 |
| 한꺼번에 | 자동 수정 | 자체 판단 수정 |

**완료 시:** 모든 주석 제거 → 사용자 확인 → TOC 상태 `검토 완료` · `docs/synopsis.md` 업데이트

**추천 명령:** `/revise` — 검토 주석 기반 이슈 수정

</details>

---

## 커맨드 레퍼런스

| 커맨드    | 역할                              | 단계    |
| --------- | --------------------------------- | ------- |
| `/init`   | 새 소설 프로젝트 초기화           | 시작    |
| `/bs`     | 아이디어·플롯·캐릭터 브레인스토밍 | Phase 1 |
| `/wiki`   | 설정집·세계관·사건 문서 작성      | Phase 2 |
| `/plan`   | 챕터별 씬·감정선·복선 설계        | Phase 3 |
| `/write`  | 장면·챕터·대화 집필               | Phase 4 |
| `/style`  | 문체 분석 및 스타일 프로필 적용   | Phase 4 |
| `/review` | 완성도 검토·A/B/C 판정            | Phase 5 |
| `/revise` | 검토 주석 기반 이슈 수정          | Phase 6 |

---

## Tips

**막힐 때**

- **뭘 써야 할지 모르겠다** → `docs/idea/`를 열고 `/bs`로 브레인스토밍부터 시작하세요.
- **설정이 기억 안 난다** → `docs/overview.md`를 먼저 열어 네비게이션으로 사용하세요.
- **집필이 막힌다** → `docs/plans/`의 계획서를 다시 읽거나 씬을 더 잘게 쪼개보세요.

**검토·수정 순서**

1. `/review` — 구조·완성도·문체 확인 (A/B/C 판정)
   - `.claude/styles/default.md`가 있으면 스타일 프로필과 자동 대조
2. `/revise` — 검토 주석 처리 (B: 문장·문체 수정, C: 구조 재설계)
3. `/style` — 특정 작가 문체 분석·저장 또는 기본 스타일 새로 설정할 때

## 요구사항

- Claude Code CLI
