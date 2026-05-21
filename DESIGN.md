# DESIGN.md — 이 위키는 무엇이고 왜 이렇게 만들었나

> Andrej Karpathy의 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
> 패턴을 **일본어 학습**에 적용한 개인 지식베이스.

## 한 줄 요약
매번 자료를 다시 읽는 RAG 대신, **LLM이 직접 관리하는 markdown 위키**를
점점 키워나간다. 자료를 넣을수록 위키가 똑똑해지고 그 가치가 영구히 남는다(compounding).

## 비유
- **Git 저장소** = 코드베이스(위키 본체, 모델 중립)
- **LLM** = 프로그래머(정리·종합을 대신 해주는 일꾼)
- **사람(나)** = 자료 큐레이션 + 좋은 질문 + 시험 보기

---

## 아키텍처 (역할 분리가 핵심)

세 동작은 **서로 다른 기계·모델**이 맡아도 된다. 위키가 plain markdown이라
모델 중립이고, **Git이 두 AI 사이의 중립 교환소** 역할을 한다.

```
[폰] 텔레그램에 일본어 메모/사진 전송
   ↓
[집 미니PC · OpenClaw / OpenAI]   ← 항상 켜짐. 받아서 inbox.md에 append + 시험 출제
   ↓  git push  (중립 교환소)
[맥 · Claude Code / Anthropic]    ← 야간 배치. inbox 읽고 위키 종합·정리 후 push
   ↓
[폰 · GitHub 앱]  정리된 위키 읽기 / 복습
```

| 역할 | 담당 | 모델 품질 민감도 |
|---|---|---|
| 캡처 받기 (inbox append) | OpenClaw(OpenAI), 미니PC | 낮음 — 단순 기록 |
| 시험 출제·채점 | OpenClaw(OpenAI), 미니PC | 중간 — OpenAI 일본어 채점 충분 |
| 위키 종합·정리 | Claude Code(Anthropic), 맥 | 높음 — 링크 정합성/모순 제거 |

> 모델이 다른 건 버그가 아니라 설계다. 똑똑해야 하는 종합 단계는 Claude가,
> 항상 켜져 있어야 하는 입구는 이미 돌고 있는 OpenClaw가 맡는다.
> 두 모델의 일관성은 공유 규칙 파일 [AGENTS.md](AGENTS.md)가 보장한다.

---

## 시험 방식 — "철학 B" (LLM이 출제관)

플래시카드(Anki)가 아니라 **텔레그램 채팅으로 적응형 시험**을 본다.
- 문법은 **문장 생산**으로 테스트 (조사 변화까지 채점)
- 틀린 것은 `mistakes.md`에 맥락과 함께 누적 → 다음 시험에 우선 출제
- `srs.md`에 단어/문법별 다음 복습일을 기록(간격 반복을 LLM이 bookkeeping)

---

## 결정 로그 (2026-05-21)
- **용도**: 일본어 학습(단어+문법 통합). 영어/사이드프로젝트는 후순위 확장 후보.
- **저장**: Git private 저장소(`LLM-nihongo-wiki`), plain markdown. 폰은 GitHub 앱으로 읽기.
- **링크 형식**: `[[wikilink]]` 대신 일반 markdown 링크 `[텍스트](path.md)` — GitHub에서 렌더링·클릭 이동되도록.
- **캡처**: 텔레그램.
- **시험**: 철학 B(LLM 출제관, 적응형).
- **정리 시점**: 야간 자동 배치(맥의 Claude Code).

## 집에서 마저 할 세팅 (체크리스트)
- [ ] 미니PC에서 이 저장소 `git clone`
- [ ] OpenClaw가 텔레그램 메시지를 받아 `inbox.md`에 append 하도록 연결
- [ ] OpenClaw가 변경분을 `git commit && push` 하도록 설정 (또는 주기적 cron)
- [ ] OpenClaw 시험 모드: `vocab/`·`grammar/`·`mistakes.md`를 읽고 텔레그램으로 출제 → 오답 `mistakes.md` 기록
- [ ] 맥의 Claude Code 야간 배치: `git pull` → inbox 정리 → `git push` (수동으로 시작, 익으면 스케줄)
- [ ] 두 에이전트 모두 [AGENTS.md](AGENTS.md) 규칙을 따르도록 프롬프트에 명시
