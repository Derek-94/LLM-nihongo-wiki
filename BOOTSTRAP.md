# BOOTSTRAP — 집(미니PC)에서 이것만 보고 따라 하기

> 이 파일 위에서 아래로 순서대로 하면 끝. 헷갈리면 **STEP 1 → 2 → 3 → 4** 만 따라가세요.
> 핵심 원리: git repo가 "공유 두뇌"라 맥과 미니PC가 같은 내용을 본다.
> 간극 유지 규칙은 단 하나 — **작업 전 `git pull`, 작업 후 `git push`.**

---

## STEP 1. (최초 1회만) 저장소 받기

```bash
cd ~/원하는/위치
git clone https://github.com/Derek-94/LLM-nihongo-wiki.git
cd LLM-nihongo-wiki
pwd        # ← 여기 나오는 경로를 STEP 3에서 쓴다. 복사해두기.
```

## STEP 2. OpenClaw 띄우기 (반드시 repo 폴더 안에서)

```bash
openclaw tui
```

## STEP 3. TUI 첫 메시지에 아래 전체를 붙여넣기

> `<REPO경로>` 부분만 STEP 1의 `pwd` 결과로 바꾼다. 그 외엔 그대로 붙여넣기.

```
당신은 일본어 학습 위키를 관리하는 에이전트입니다.
작업 디렉토리: /home/derek/Documents/llm/nihongo-wiki
시작할 때 먼저 AGENTS.md와 DESIGN.md를 읽고 그 규칙을 그대로 따르세요.

[캡처 모드] 사용자가 텔레그램으로 일본어 메모/사진/단어를 보내면:
1. git pull
2. inbox.md 맨 아래에 "## YYYY-MM-DD HH:MM" 헤더로 append
3. 사용자가 직접 쓴 텍스트는 가능한 한 원문 그대로 보존
4. 사진/스크린샷이면 첨부 경로 또는 첨부 표시를 남기고, 핵심 문법·예문·단어 후보를 "1차 추출" 섹션으로 짧게 정리
5. 이 단계에서는 grammar/·vocab/ 본문을 직접 수정하지 말고 inbox에만 적재
6. git add -A && git commit -m "inbox: 요약" && git push

[시험 모드] 사용자가 "시험" 또는 "테스트"라고 하면:
1. git pull
2. 출제 우선순위: mistakes.md(자주 틀린 것) > srs.md의 오늘 복습 예정 > 최근 학습
3. 문법은 문장 생산형으로 출제하고 조사까지 채점
4. 오답은 mistakes.md에 "날짜·문제·내 답·정답·해설"로 기록, srs.md의 다음복습일 갱신
5. git add -A && git commit -m "test: 결과" && git push

위키 본문(grammar/·vocab/)을 직접 크게 고치지는 마세요. 정리·종합은 맥의 Claude Code가 합니다.
```

→ 이제 미니PC의 OpenClaw가 맥에서 정한 규칙을 똑같이 알게 된다. **간극 0.**

## STEP 4. 다음부터 (이미 clone 했으면 STEP 1 생략)

```bash
cd LLM-nihongo-wiki
git pull          # 맥에서 바뀐 내용 먼저 받기
openclaw tui      # 그리고 STEP 3 프롬프트 붙여넣기
```

> ⚠️ 두 기계가 같은 repo를 만진다. 충돌 방지를 위해 항상 **pull 먼저, push 나중**.
> push에서 `authentication failed`가 뜨면 git 인증 문제 → `gh auth login` 또는 SSH 키/토큰 설정.

---
---

# (참고) 맥에서 야간 정리할 때 — Claude Code

> 위 STEP들은 미니PC용. 맥에서 위키를 정리할 땐 cmux에서 repo 폴더를 열고 아래를 보내거나,
> 짧게 "위키 정리해줘"라고만 해도 됨.

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

# (참고) 더 자동화하고 싶다면 (지금은 안 해도 됨)
- **반자동**: 미니PC에 `sync.sh`(`cd <REPO경로> && git pull`) 만들어 OpenClaw 시작 전 실행.
- **완전자동(고급)**: cron으로 주기적 `git pull`, 맥은 스케줄러로 야간에 위 정리 프롬프트 자동 실행.
