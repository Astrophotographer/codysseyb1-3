# codysseyb1-3

반복 업무 자동화를 주제로 한 2개 프로젝트 산출물 저장소입니다.

## 제출 산출물 구성

| 구분 | 파일 | 내용 |
|---|---|---|
| 프로젝트 1 | [`docs/project1-automation-tool-comparison.md`](docs/project1-automation-tool-comparison.md) | Make와 n8n으로 동일 자동화 워크플로우를 구현한 비교 분석 보고서 |
| 프로젝트 2 | [`docs/project2-cs-desk-automation.md`](docs/project2-cs-desk-automation.md) | CS Desk 자동화 설계 및 구현 문서 |
| 스크린샷 안내 | [`assets/README.md`](assets/README.md) | 구현 화면/실행 결과 캡처 파일 배치 안내 |

## 프로젝트 요약

### 프로젝트 1. 자동화 도구 비교 구현

- 비교 도구: **Make**, **n8n**
- 동일 업무: 문의/메일 수신 내용을 구조화하여 스프레드시트에 저장하고 팀 채널로 공유하는 자동화
- 핵심 비교 포인트:
  - Make: 시각적 구성과 빠른 SaaS 연동에 강점
  - n8n: 직접 호스팅 시 무료로 운영 가능하며, MCP 연동을 통해 AI가 워크플로우 노드를 직접 수정하는 바이브코딩 방식에 유리

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
assets/project2/cs-desk-form.png
assets/project2/n8n-workflow.png
assets/project2/discord-result.png
assets/project2/email-result.png
```

> 현재 문서는 위 파일명을 기준으로 이미지 링크를 포함하고 있습니다. 실제 제출 전 캡처 이미지를 해당 경로에 추가하면 됩니다.
