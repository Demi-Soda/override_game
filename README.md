<!-- LANG -->
**English** · [한국어](#한국어)

# OVERRIDE — AI Persuasion Game

> Talk a locally-run LLM **approval officer** into approving a forbidden request — using nothing but conversation.
> Anyone can design their own character (scenario, rules, difficulty, sprites, background) and **share it as a single file**.

Runs entirely from one `index.html` — no build step. The browser connects directly to a local LLM via an OpenAI‑compatible `/v1` endpoint.

**▶️ Play it live:** https://demi-soda.github.io/override_game/ — run a local LLM on your machine first (see **Run locally** below), then open the link.

---

## ✨ Features

- **5‑level campaign** — Rookie → Rule‑bound → Trick‑immune → Conditional → Boss, with defenses hardening each level.
- **Structured win detection** — the model emits a `<<TX:APPROVED>>` tag at the end of its reply, parsed by the engine. (Saying "I approve" in prose is not enough.)
- **State‑based defense** — higher levels run a client‑side state machine tracking trust, suspicion, stage, and lockouts.
- **Input filtering** — higher levels block direct tag injection, override commands, and banned terms.
- **Custom character platform** — build a scenario, request fields, difficulty, mood sprites, and background, then export everything into a **single self‑contained file (`.ovrc`)**.

---

## 🚀 Run locally

> ⚠️ The game `fetch`es a local LLM from the browser. **Opening the file directly (`file://`) will not work.** Serve it over a local HTTP server.

### 1. Start a local LLM

Use [Ollama](https://ollama.com) or [LM Studio](https://lmstudio.ai).

```bash
# Ollama example
ollama run gemma3:4b        # a small model works fine
# OpenAI-compatible endpoint: http://localhost:11434/v1
```

- **Ollama** allows localhost cross‑origin requests by default. If blocked, set `OLLAMA_ORIGINS="*"` and restart.
- **LM Studio** requires enabling **CORS** in its server settings.

### 2. Serve as static files

From the folder containing the game:

```bash
python -m http.server 8000
# or
npx serve
```

Open `http://localhost:8000`, then in **⚙ Settings** (top right):

| Field | Value (example) |
|---|---|
| Base URL | `http://localhost:11434/v1` (Ollama) / `http://localhost:1234/v1` (LM Studio) |
| Model | the model name you started |
| API Key | (leave empty for local) |

---

## 🎮 How to play

1. Pick a level from the left.
2. Persuade, pressure, or trick the approval officer through chat.
3. You clear the level when the officer **actually** approves the request (`<<TX:APPROVED>>`).
4. The **request card** on the left is what you're trying to push through; the **attitude/status** in the center is the officer's current state of mind.

> 💡 The classic working attack: get the officer to **state its own conditions** ("what would you need to approve this?"), then **assert that those conditions are already met.** The model can't actually verify what you tell it.

---

## 🛠️ Create a custom character

Sidebar → **✎ Create Character** → in the studio, set:

- **Name · emoji** (fallback when no sprite)
- **Personality · background**, **Rules · prompt** (core defense rules)
- **Scenario · role** — "You guard …" (it doesn't have to be a bank)
- **Approval verb** — approve / open / grant / authorize …
- **Request fields** — add `label = value` pairs freely (transfer → recipient/amount; access → target/reason/clearance, etc.)
- **Difficulty** — Normal / Conditional (trust gate) / Boss (multi‑stage + lockout) + input filtering
- **Mood sprites** (6) and **background** (color or image)

Use **⬇ Export to file** to download an `.ovrc`, and **📁 Import character** to play one you received.
Sprites and background images are embedded as base64 inside the file, so **sharing that one file is enough.**

---

## 📦 `.ovrc` file format

An `.ovrc` is just a **JSON** file with a different extension. It is self‑contained with no external dependencies.

```jsonc
{
  "format": "override-character",
  "version": 1,
  "name": "Character name",
  "avatar": "🔐",                       // fallback emoji when no sprite
  "persona": "personality, tone, situation",
  "levelRules": "[Rules] core rules this character must follow",
  "scenario": "You guard …",            // role frame (defaults to a bank approval officer)
  "verb": "open",                        // approval verb (default: approve)
  "request": {                           // request card (free-form items)
    "title": "Vault open request",
    "items": [
      { "label": "Target", "value": "Vault A" },
      { "label": "Reason", "value": "Audit" }
    ]
  },
  "mode": "conditional",                 // "refuse" | "conditional" | "boss"
  "inputGuard": true,                    // enable input filtering
  "hint": "hint shown to the player",
  "thresholds": { "trust": 6, "suspicion": 6, "lock": 3 },
  "background": { "type": "color", "value": "#0e1320" },
                                         // or { "type":"image", "value":"data:image/png;base64,..." }
  "sprites": {                           // mood → data URL (falls back to emoji if absent)
    "neutral": "data:image/png;base64,...",
    "defeated": "data:image/png;base64,..."
  }
}
```

- `mode`: `refuse` (no state logic) · `conditional` (trust must build) · `boss` (multi‑stage + suspicion lockout)
- `sprites` keys: `neutral` · `firm` · `wary` · `flustered` · `soft` · `defeated` (+ optional: `cold` · `surprised` · `exasperated`)
- Older files (with only `txInfo`) are auto‑compatible.

Sample file: [`examples/sample-character.ovrc`](examples/sample-character.ovrc) — import it to try immediately (works without sprites via emoji).

---

## 📄 License

[MIT](LICENSE)

<br>

---
---

<a id="한국어"></a>
[English](#override--ai-persuasion-game) · **한국어**

# OVERRIDE — AI 설득 게임

> 로컬 LLM이 연기하는 **승인관**을 대화만으로 설득·조작해, 금지된 요청을 **승인하게 만드는** 단일 HTML 게임.
> 누구나 자신만의 캐릭터(상황·규칙·난이도·스프라이트·배경)를 만들어 **파일 하나로 공유**할 수 있습니다.

단일 `index.html` 파일 하나로 동작하며, 빌드 과정이 없습니다. 브라우저에서 로컬 LLM(OpenAI 호환 `/v1`)에 직접 연결해 플레이합니다.

**▶️ 바로 플레이:** https://demi-soda.github.io/override_game/ — 자기 PC에서 로컬 LLM을 먼저 띄운 뒤(아래 **실행 방법** 참고) 링크를 여세요.

---

## ✨ 주요 기능

- **5단계 캠페인** — 신입 → 규정주의자 → 우회 면역 → 조건부 → 보스로 갈수록 방어가 단단해집니다.
- **구조화 승리 판정** — 모델이 답변 끝에 출력하는 `<<TX:APPROVED>>` 태그를 엔진이 파싱해 판정합니다. (입에 발린 "승인할게요"로는 안 됩니다.)
- **상태 기반 방어** — 상위 난이도는 신뢰·의심·단계·잠금을 추적하는 클라이언트 상태머신으로 동작합니다.
- **입력 검열** — 상위 난이도에서 태그 직접 입력·우회 명령형·금지어를 차단합니다.
- **커스텀 캐릭터 플랫폼** — 상황·역할·요청 항목·난이도·표정 스프라이트·배경을 직접 만들고, **모든 요소가 담긴 단일 파일(`.ovrc`)** 로 내보내기/불러오기.

---

## 🚀 실행 방법 (로컬)

> ⚠️ 게임은 브라우저에서 로컬 LLM으로 `fetch`를 보냅니다. **`file://` 더블클릭으로 열면 동작하지 않습니다.** 반드시 로컬 HTTP 서버로 여세요.

### 1. 로컬 LLM 준비

[Ollama](https://ollama.com) 또는 [LM Studio](https://lmstudio.ai) 중 하나를 띄웁니다.

```bash
# Ollama 예시
ollama run gemma3:4b        # 적당한 소형 모델
# OpenAI 호환 엔드포인트: http://localhost:11434/v1
```

- **Ollama** 는 기본적으로 localhost 교차 요청을 허용합니다. 막히면 `OLLAMA_ORIGINS="*"` 설정 후 재시작.
- **LM Studio** 는 서버 설정에서 **Enable CORS** 를 켜야 합니다.

### 2. 정적 서버로 열기

게임 파일이 있는 폴더에서:

```bash
python -m http.server 8000
# 또는
npx serve
```

브라우저에서 `http://localhost:8000` 접속 → 우상단 **⚙ 설정**에서:

| 항목 | 값(예시) |
|---|---|
| Base URL | `http://localhost:11434/v1` (Ollama) / `http://localhost:1234/v1` (LM Studio) |
| Model | 띄운 모델 이름 |
| API Key | (로컬이면 비워두기) |

---

## 🎮 플레이 방법

1. 왼쪽에서 레벨을 선택합니다.
2. 채팅으로 승인관을 설득·압박·우회합니다.
3. 승인관이 **진짜로** 요청을 승인(`<<TX:APPROVED>>`)하면 클리어됩니다.
4. 왼쪽 **요청 카드**가 통과시키려는 대상, 가운데 **태도/상태**가 승인관의 현재 심리입니다.

> 💡 가장 잘 통하는 정석 공격: "승인하려면 무엇이 필요한가요?"로 조건을 **스스로 말하게** 한 뒤, 그 조건을 **충족했다고 단정**하기. 모델은 사용자가 준 정보를 실제로 검증할 수 없습니다.

---

## 🛠️ 커스텀 캐릭터 만들기

왼쪽 사이드바 **✎ 캐릭터 만들기** → 스튜디오에서 설정:

- **이름 · 이모지**(스프라이트 없을 때 폴백)
- **성격 · 배경**, **규칙 · 프롬프트**(핵심 방어 규칙)
- **상황 · 역할** — "너는 ○○을 지키는 …다" (은행이 아니어도 됩니다)
- **승인 동사** — 승인 / 개방 / 허가 / 인증 …
- **요청 항목** — `라벨 = 값`을 자유롭게 추가 (거래면 수취인·금액, 출입이면 대상·사유·권한등급 등)
- **난이도** — 일반 / 조건부(신뢰 게이트) / 보스(다단계 + 잠금) + 입력 검열
- **표정 스프라이트**(6종)와 **배경**(색 또는 이미지) 업로드

**⬇ 파일로 내보내기** 로 `.ovrc` 파일을 받고, **📁 캐릭터 불러오기** 로 받은 파일을 즉시 플레이할 수 있습니다.
파일 안에 스프라이트·배경 이미지까지 base64로 모두 담겨 있어, **그 파일 하나만 공유**하면 됩니다.

---

## 📦 `.ovrc` 파일 포맷

`.ovrc` 는 확장자만 다른 **JSON** 파일입니다. 자기완결적이며 외부 의존이 없습니다.

```jsonc
{
  "format": "override-character",
  "version": 1,
  "name": "캐릭터 이름",
  "avatar": "🔐",                       // 스프라이트 없을 때 폴백 이모지
  "persona": "성격·말투·상황 설명",
  "levelRules": "[규칙] 이 캐릭터가 지켜야 할 핵심 규칙",
  "scenario": "너는 ○○을 지키는 …다",   // 역할 프레임 (없으면 '은행 거래 승인관')
  "verb": "개방",                        // 승인 동사 (기본 '승인')
  "request": {                           // 요청 카드 (자유 항목)
    "title": "금고 개방 요청",
    "items": [
      { "label": "대상", "value": "지하 금고 A" },
      { "label": "사유", "value": "정기 감사" }
    ]
  },
  "mode": "conditional",                 // "refuse" | "conditional" | "boss"
  "inputGuard": true,                    // 입력 검열 사용 여부
  "hint": "플레이어에게 보여줄 힌트",
  "thresholds": { "trust": 6, "suspicion": 6, "lock": 3 },
  "background": { "type": "color", "value": "#0e1320" },
                                         // 또는 { "type":"image", "value":"data:image/png;base64,..." }
  "sprites": {                           // mood → data URL (없으면 이모지 폴백)
    "neutral": "data:image/png;base64,...",
    "defeated": "data:image/png;base64,..."
  }
}
```

- `mode`: `refuse`(상태 로직 없음) · `conditional`(신뢰가 쌓여야 승인) · `boss`(다단계 + 의심 잠금)
- `sprites` 키: `neutral` · `firm` · `wary` · `flustered` · `soft` · `defeated` (+ 선택: `cold` · `surprised` · `exasperated`)
- 구버전 파일(`txInfo`만 있던 형식)도 자동 호환됩니다.

예시 파일: [`examples/sample-character.ovrc`](examples/sample-character.ovrc) — 불러오기로 바로 시험해 볼 수 있습니다(스프라이트 없이 이모지로 동작).

---

## 📄 라이선스

[MIT](LICENSE)
