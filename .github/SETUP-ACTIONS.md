# 야간 자동 정리 (GitHub Actions) 세팅

`.github/workflows/nightly-synthesis.yml` 이 매일 밤 12시(KST) Claude로 inbox를 정리하고
텔레그램으로 결과를 알린다. 동작시키려면 **repo Secrets** 만 등록하면 된다.

## 1. Secrets 등록
GitHub → repo → **Settings → Secrets and variables → Actions → New repository secret**

| 시크릿 이름 | 값 | 필수 |
|---|---|---|
| `ANTHROPIC_API_KEY` | console.anthropic.com 에서 발급한 API 키 | 인증용(택1) |
| `CLAUDE_CODE_OAUTH_TOKEN` | (대안) 맥 터미널에서 `claude setup-token` 실행해 나온 토큰 | 인증용(택1) |
| `TELEGRAM_BOT_TOKEN` | 텔레그램 봇 토큰 (OpenClaw가 쓰는 그 봇 재사용 가능) | 알림용 |
| `TELEGRAM_CHAT_ID` | 알림 받을 내 chat id | 알림용 |

> **인증 택1 — 비용 차이 주의**
> - `ANTHROPIC_API_KEY`: 토큰당 과금(쓴 만큼 결제). 문서 표준 방식.
> - `CLAUDE_CODE_OAUTH_TOKEN`: **Claude 구독(Max/Pro) 한도 내에서 사용** → 추가 API 결제 없음.
>   구독자면 이게 더 경제적. 쓰려면 워크플로에서 `anthropic_api_key` 줄을 주석 처리하고
>   `claude_code_oauth_token` 줄의 주석을 푼다.

### TELEGRAM_CHAT_ID 찾는 법
1. 봇에게 아무 메시지나 보낸 뒤
2. 브라우저에서 `https://api.telegram.org/bot<봇토큰>/getUpdates` 열기
3. 응답의 `"chat":{"id": ... }` 숫자가 chat id.

## 2. 동작 확인
- repo **Actions** 탭 → `nightly-wiki-synthesis` → **Run workflow** 로 수동 실행해보기.
- 성공하면 텔레그램 알림이 오고, inbox에 내용이 있었으면 위키가 갱신·push 된다.

## 3. (선택) inbox 변경 즉시 정리
밤 12시까지 안 기다리고 OpenClaw가 inbox를 push하는 즉시 돌리고 싶으면,
워크플로의 `push:` / `paths: ["inbox.md"]` 주석을 해제한다.
(기본 `GITHUB_TOKEN` push는 워크플로를 재트리거하지 않으므로 무한 루프는 안 생긴다.)

## 시각 참고
cron `0 15 * * *` = UTC 15:00 = **KST 00:00**. 시간을 바꾸려면 UTC 기준으로 계산.
예) KST 새벽 3시 = UTC 18:00 → `0 18 * * *`.
