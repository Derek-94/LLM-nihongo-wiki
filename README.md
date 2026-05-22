# LLM-nihongo-wiki

LLM이 관리하는 **일본어 학습 위키**. 자료를 넣을수록 똑똑해진다(compounding).

- **무엇/왜**: [DESIGN.md](DESIGN.md)
- **운영 규칙(에이전트 공통)**: [AGENTS.md](AGENTS.md)
- **문법**: [grammar/_index.md](grammar/_index.md)
- **단어**: [vocab/_index.md](vocab/_index.md)
- **틀린 것**: [mistakes.md](mistakes.md) · **복습 스케줄**: [srs.md](srs.md)
- **캡처 큐**: [inbox.md](inbox.md)

## 전체 그림

```
📱 폰 텔레그램  ──캡처──▶  🖥️ 미니PC OpenClaw  ──push──▶  ☁️ GitHub repo
                                                              │
                                              매일 밤 12시 cron │
                                                              ▼
                                        🤖 GitHub Actions + Claude (정리)
                                                              │
                                          ┌──push──▶ repo 갱신 ┘
                                          └──알림──▶ 📲 텔레그램
```

세 역할이 **Git repo 하나**로 이어진다.

| 역할 | 담당 | 시점 |
|---|---|---|
| 캡처 | 미니PC OpenClaw(OpenAI) | 텔레그램 받을 때마다 → `inbox.md`에 적재 후 push |
| 종합·정리 | GitHub Actions + Claude(Anthropic) | 매일 밤 12시(KST) 자동, `inbox` → `grammar/`·`vocab/` |
| 시험 | 텔레그램 OpenClaw | "시험"이라 하면 `srs.md`/`mistakes.md`로 출제 |

## 자동 정리 스케줄
- **언제**: 매일 자정(00:00 KST). 워크플로 `.github/workflows/nightly-synthesis.yml`의 cron `0 15 * * *`(=UTC 15:00).
  - GitHub 스케줄은 정각 근처이며 부하 시 수십 분 지연될 수 있음.
  - repo 활동이 60일 없으면 GitHub가 스케줄을 자동 비활성화(이메일 통지).
- **수동 실행**: GitHub → Actions 탭 → `nightly-wiki-synthesis` → Run workflow.
- **즉시 처리**: 워크플로의 `push: paths: ["inbox.md"]` 주석을 풀면 inbox push 즉시 정리.
- **세팅**: [.github/SETUP-ACTIONS.md](.github/SETUP-ACTIONS.md) (필요 Secrets 등).
