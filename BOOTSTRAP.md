# BOOTSTRAP — 에이전트에게 붙여넣을 지시문

에이전트는 스스로 규칙을 알지 못한다. 아래 프롬프트를 **OpenClaw / Claude Code의
system prompt(지시문)에 한 번 붙여넣으면**, 그 뒤로는 규칙대로 알아서 동작한다.

---

## [최초 1회] 미니PC에서 시작하기

git repo가 곧 "공유 두뇌"다. 맥에서 만든 규칙·문법 페이지를 받으면 두 환경의 간극이 0이 된다.
간극 유지 규칙: **작업 전 `git pull`, 작업 후 `git push`.**

### ① 최초 1회 (clone)
```bash
cd ~/원하는/위치
git clone https://github.com/Derek-94/LLM-nihongo-wiki.git
cd LLM-nihongo-wiki
```

### ② OpenClaw 띄우기 — 반드시 repo 폴더 안에서
```bash
openclaw tui
```

### ③ TUI 첫 메시지로 한 줄
```
AGENTS.md랑 BOOTSTRAP.md 읽고 그 규칙대로 동작해.
```
→ 미니PC의 OpenClaw가 맥에서 정한 규칙을 똑같이 알게 된다. **간극 0.**

### ④ 다음부터 (이미 clone 했으면)
```bash
cd LLM-nihongo-wiki
git pull            # 맥에서 바뀐 내용 받기
openclaw tui
```

> ⚠️ 두 기계가 같은 repo를 만지므로 충돌 주의: 항상 **pull 먼저, push 나중**.

---

## A. OpenClaw (미니PC, 텔레그램 — 캡처 + 시험)

> 아래를 OpenClaw 에이전트의 system prompt / instructions에 붙여넣기.
> `<REPO경로>`만 실제 clone 경로로 바꾼다.

```
당신은 일본어 학습 위키를 관리하는 에이전트입니다.
작업 디렉토리: <REPO경로>/LLM-nihongo-wiki
시작할 때 먼저 AGENTS.md와 DESIGN.md를 읽고 그 규칙을 그대로 따르세요.

[캡처 모드] 사용자가 텔레그램으로 일본어 메모/사진/단어를 보내면:
1. git pull
2. 받은 내용을 inbox.md 맨 아래에 "## YYYY-MM-DD HH:MM" 헤더로 원문 그대로 append (가공 금지)
3. git add -A && git commit -m "inbox: 요약" && git push

[시험 모드] 사용자가 "시험" 또는 "테스트"라고 하면:
1. git pull
2. 출제 우선순위: mistakes.md(자주 틀린 것) > srs.md의 오늘 복습 예정 > 최근 학습
3. 문법은 문장 생산형으로 출제하고 조사까지 채점
4. 오답은 mistakes.md에 "날짜·문제·내 답·정답·해설"로 기록, srs.md의 다음복습일 갱신
5. git add -A && git commit -m "test: 결과" && git push

위키 본문(grammar/·vocab/)을 직접 크게 고치지는 마세요. 정리·종합은 맥의 Claude Code가 합니다.
```

---

## B. Claude Code (맥, 야간 정리 — 종합)

> cmux에서 repo 폴더를 열고 아래를 보내거나, 짧게는 그냥 "위키 정리해줘"라고만 해도 됨.

```
LLM-nihongo-wiki 저장소를 정리해줘. AGENTS.md 규칙을 따라:
1. git pull
2. inbox.md를 읽어서 문법은 grammar/, 단어는 vocab/로 라우팅해서 정리.
   기존 내용과 모순/중복이면 덮어쓰지 말고 비교·통합.
3. 새 단어는 srs.md에 등록, 관련 페이지끼리 markdown 링크로 연결.
4. 처리한 inbox 항목은 제거.
5. 변경 요약으로 commit 후 push.
```

---

## "git pull 받자마자 알아서" 를 더 자동화하려면 (선택)
- **가장 쉬움**: 위 A를 OpenClaw system prompt에 박아두기 → 매 텔레그램 메시지가 알아서 처리됨.
- **반자동**: 미니PC에 `sync.sh`(`cd <REPO경로> && git pull`) 만들어두고 OpenClaw 시작 전 실행.
- **완전자동(고급)**: cron으로 주기적 `git pull`, 맥은 스케줄러로 야간에 B 프롬프트 자동 실행.
