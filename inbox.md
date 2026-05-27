# inbox — 캡처 큐 (raw)

텔레그램으로 들어온 원본이 여기 시간순으로 쌓인다.
종합 에이전트(맥 Claude Code)가 야간에 읽어서 위키로 정리한 뒤 비운다.
규칙: [AGENTS.md](AGENTS.md) 참고.

## 캡처 표준 형식

```md
## YYYY-MM-DD HH:MM

원문:
(사용자가 직접 보낸 텍스트를 가능한 한 그대로)

첨부:
- image: 파일명 또는 경로
- file: 파일명 또는 경로

1차 추출:
- 문법 주제:
- 예문:
  - ...
- 단어 후보:
  - ...
- 메모:
  - ...
```

---

<!--
정리 로그
- 2026-05-22: 2026-05-21 캡처 4건(사역 문법 + 단어) 종합 완료.
  → grammar/13-사역동사.md 보강, vocab/* 분류 확인, srs.md 등록. 처리된 raw는 git 히스토리 참조.
- 2026-05-24: 2026-05-24 캡처 2건(14차시 수동태 문법 + 단어) 종합 완료.
  → grammar/14-수동태.md 신규 작성, grammar/_index.md 차시/연결 갱신,
    vocab/{nouns,verbs,adverbs-expressions}.md 분류 추가, srs.md에 신규 항목 등록.
    처리된 raw는 git 히스토리 참조.
- 2026-05-25: 2026-05-25 캡처 3건(Lesson 12 수수동사·Lesson 13 단어) 종합 완료.
  → grammar/12-수수동사.md에 〜て+수수동사 표·예문 보강,
    grammar/n-yotte.md 신규 작성(수단/원인/기준/수동행위자 4갈래),
    grammar/14-수동태.md 조사표에 によって 행 추가,
    grammar/_index.md에 표현·기타 섹션과 によって↔수동 연결 추가,
    vocab/{nouns,verbs,adverbs-expressions}.md에 12·13과 단어 분류 추가,
    srs.md에 신규 항목 등록. 처리된 raw는 git 히스토리 참조.
- 2026-05-27: 2026-05-27 캡처 3건(15과 사역수동 + 표현 ～のに/～ばかり/～だけ·しか·も + 보충 메모) 종합 완료.
  → grammar/15-사역수동.md 신규 작성,
    grammar/{n-noni,n-bakari,n-shika-dake-mo}.md 신규 작성,
    grammar/_index.md에 15차시 차시·표현 페이지·주제 연결 추가,
    grammar/13-사역동사.md·14-수동태.md의 '사역수동 후속 차시 예정' 메모를 15차시 링크로 교체,
    vocab/{nouns,verbs,adverbs-expressions}.md에 15과 단어 분류 및
    ごろごろする↔うろうろする 의태어 섹션 추가,
    srs.md에 신규 grammar 6건·vocab 24건 등록.
    사용자 메모의 '~이나=모우'는 も(もう 아님), '~밖에'는 しか…ない로 정리.
    처리된 raw는 git 히스토리 참조.
-->
