# codysseyb1-3

반복 업무 자동화를 주제로 한 2개 프로젝트 산출물 저장소입니다.

## 제출 산출물 구성

| 구분 | 파일 | 내용 |
|---|---|---|
| 프로젝트 1 | [`docs/project1-automation-tool-comparison.md`](docs/project1-automation-tool-comparison.md) | Make와 n8n으로 동일한 Google Form → Google Sheets 응답 저장 → 카테고리별 시트 분류 워크플로우를 구현한 비교 분석 보고서 |
| 프로젝트 2 | [`docs/project2-cs-desk-automation.md`](docs/project2-cs-desk-automation.md) | CS Desk 자동화 설계 및 구현 문서 |
| Discord 설정 | [`docs/discord-setup-guide.md`](docs/discord-setup-guide.md) | Discord 채널, Webhook, Bot, 권한 설정 상세 가이드 |
| 스크린샷 안내 | [`assets/README.md`](assets/README.md) | 구현 화면/실행 결과 캡처 파일 배치 안내 |

## 프로젝트 요약

### 프로젝트 1. 자동화 도구 비교 구현

- 비교 도구: **Make**, **n8n**
- 동일 업무: Google Form 제출 데이터를 Google Sheets에서 확인하고 카테고리에 따라 `일반문의`/`이용문의` 시트로 분리 저장
- n8n 워크플로우: `[과제] 프로젝트1 - Google Form 응답 카테고리별 시트 분류 (n8n)`
- 핵심 포인트: 단순한 폼 입력 자동화 흐름을 유지하면서 Trigger, Action 2개 이상, 조건 분기를 모두 포함
- MCP 관점: Make와 n8n 모두 MCP/AI 도구 연동이 가능하며, 문서에서는 두 도구의 연동 방식과 제약 차이를 함께 비교

### 프로젝트 2. CS Desk 자동화

- 반복 업무: 고객 문의 접수, 분류, 기록, 담당 채널 알림, 고객 접수 확인 및 답변 발송
- 사용 도구: **n8n**
- 구현 흐름:
  1. CS Desk 웹폼 제출
  2. n8n Webhook 수신
  3. AI Agent가 문의 내용 구조화 및 제목 생성
  4. Google Sheets 기록
  5. 카테고리에 따라 일반/긴급 Discord 채널 분기
  6. 고객에게 접수 완료 메일 발송
  7. Discord 쓰레드에 `메일발송:` 댓글 작성 시 AI 답변 메일 발송

## 스크린샷 파일명 권장

스크린샷은 아래 경로에 넣으면 문서의 링크와 바로 연결됩니다.

```text
assets/project1/make-workflow.png
assets/project1/make-result.png
assets/project1/n8n-workflow.png
assets/project1/n8n-result.png
assets/project1/google-sheets-result.png
assets/project1/n8n-project1-simple-workflow.json
assets/project2/cs-desk-form.png
assets/project2/n8n-workflow.png
assets/project2/discord-result.png
assets/project2/email-result.png
```

> 현재 문서는 위 파일명을 기준으로 이미지 링크를 포함하고 있습니다. 실제 제출 전 캡처 이미지를 해당 경로에 추가하면 됩니다.
