# Discord 설정 과정 상세 가이드

이 문서는 CS Desk 자동화에서 Discord 알림과 쓰레드 답변 기능을 사용하기 위해 필요한 설정 과정을 처음 하는 사람도 따라 할 수 있도록 정리한 문서이다.

## 1. Discord 구성 목표

CS Desk 자동화에서는 Discord를 두 가지 용도로 사용한다.

1. **CS 알림 채널**
   - 일반 문의는 일반 채널로 전송한다.
   - 장애/오류 문의는 긴급 채널로 전송한다.

2. **Discord 쓰레드 답변 자동화**
   - CS 알림 메시지 아래 쓰레드에서 담당자가 `메일발송:`으로 시작하는 댓글을 작성한다.
   - n8n이 해당 댓글을 읽는다.
   - AI가 고객용 답변 메일을 생성한다.
   - Gmail로 고객에게 답변을 발송한다.
   - Discord 쓰레드에 발송 완료 메시지를 남긴다.

---

## 2. Discord 서버와 채널 만들기

### 2.1 서버 준비

이미 사용할 Discord 서버가 있다면 기존 서버를 사용해도 된다. 새로 만들 경우 다음 순서로 만든다.

1. Discord 왼쪽 `+` 버튼 클릭
2. `서버 만들기` 선택
3. 서버 이름 입력
4. `만들기` 클릭

예시 서버명:

```text
테스트서버
```

### 2.2 카테고리 만들기

CS 관련 채널을 묶기 위해 카테고리를 만든다.

1. 서버 이름 클릭
2. `채널 만들기` 또는 `카테고리 만들기` 선택
3. 카테고리 이름 입력

예시:

```text
CS Desk
```

### 2.3 일반 CS 채널 만들기

1. CS Desk 카테고리 오른쪽 `+` 클릭
2. 채널 유형: `텍스트`
3. 채널 이름 입력

예시:

```text
일반
```

용도:

```text
서비스 이용, 기능 문의 등 일반 문의 알림 수신
```

### 2.4 긴급 CS 채널 만들기

같은 방식으로 긴급 채널을 만든다.

예시:

```text
긴급
```

용도:

```text
서비스 장애/오류 등 우선 확인이 필요한 문의 알림 수신
```

---

## 3. Discord 개발자 모드 켜기

n8n에서 Discord 채널을 읽거나 메시지를 보내려면 채널 ID가 필요하다.

1. Discord 왼쪽 아래 사용자 설정 클릭
2. `고급` 메뉴 클릭
3. `개발자 모드` 활성화

```text
사용자 설정 → 고급 → 개발자 모드 ON
```

---

## 4. 채널 ID 복사

개발자 모드를 켠 뒤 각 채널 ID를 복사한다.

1. `일반` 채널 우클릭
2. `채널 ID 복사` 클릭
3. 값을 메모한다.
4. `긴급` 채널도 같은 방식으로 복사한다.

예시:

```text
일반 채널 ID: 1513841300737228841
긴급 채널 ID: 1513841926057496586
```

주의할 점:

- 서버 이름을 우클릭해서 복사하면 **서버 ID**가 복사된다.
- 채널 이름을 우클릭해야 **채널 ID**가 복사된다.
- 서버 ID를 n8n 채널 ID로 넣으면 Discord API에서 `Unknown Channel` 오류가 난다.

---

## 5. Discord Webhook 만들기

CS 알림 메시지를 Discord 채널에 보내기 위해 각 채널에 Webhook을 만든다.

### 5.1 일반 채널 Webhook

1. `일반` 채널 오른쪽 톱니바퀴 클릭
2. `연동` 또는 `Integrations` 메뉴 클릭
3. `웹후크` 또는 `Webhooks` 클릭
4. `새 웹후크` 클릭
5. 이름 입력

예시 이름:

```text
Captain Hook
```

6. 채널이 `일반`으로 선택되어 있는지 확인
7. `웹후크 URL 복사` 클릭
8. n8n의 `Discord 일반 CS 알림` HTTP Request 노드 URL에 붙여넣기

### 5.2 긴급 채널 Webhook

동일한 방식으로 `긴급` 채널에도 Webhook을 만든다.

예시 이름:

```text
Spidey Bot
```

n8n의 `Discord 긴급 CS 알림` HTTP Request 노드 URL에 붙여넣는다.

### 5.3 Webhook 보안 주의

Webhook URL은 외부에 노출되면 누구나 해당 채널에 메시지를 보낼 수 있다.

따라서 다음을 지킨다.

- GitHub 저장소에 Webhook URL을 커밋하지 않는다.
- 보고서 캡처 화면에 URL이 보이면 가린다.
- URL이 노출되면 Discord에서 Webhook을 삭제하고 새로 만든다.

---

## 6. Discord Bot Application 만들기

Webhook은 메시지를 보내는 데는 충분하지만, 쓰레드 댓글을 읽고 완료 메시지를 보내려면 Bot Token이 필요하다.

### 6.1 Developer Portal 접속

1. 브라우저에서 접속

```text
https://discord.com/developers/applications
```

2. `New Application` 클릭
3. Application 이름 입력

예시:

```text
n8n bot
```

4. `Create` 클릭

### 6.2 Bot 생성

1. 왼쪽 메뉴에서 `Bot` 클릭
2. `Add Bot` 또는 `Reset Bot` 영역 확인
3. Bot이 없으면 `Add Bot` 클릭
4. Bot 이름 확인

예시:

```text
n8n bot
```

---

## 7. Bot Token 발급 및 n8n 등록

### 7.1 Bot Token 발급

1. Developer Portal의 Application에서 `Bot` 메뉴 클릭
2. `Token` 영역에서 `Reset Token` 또는 `Copy Token` 클릭
3. Token을 복사한다.

주의:

- Bot Token은 비밀번호와 같다.
- Discord, GitHub, 문서, 스크린샷에 노출하면 안 된다.
- 노출되었다면 즉시 `Reset Token`으로 새 토큰을 발급한다.

### 7.2 n8n에 Token 입력

n8n 워크플로우에서 다음 노드를 연다.

```text
[메일답변] 감시 채널/토큰 설정
```

아래 값을 설정한다.

```javascript
const DISCORD_BOT_TOKEN = '새로 발급한 Bot Token';
const CHANNEL_IDS = [
  '일반 채널 ID',
  '긴급 채널 ID',
];
const TRIGGER_PREFIX = '메일발송:';
```

예시:

```javascript
const CHANNEL_IDS = [
  '1513841300737228841', // 일반 CS 알림 채널
  '1513841926057496586', // 긴급 CS 알림 채널
];
```

---

## 8. Privileged Gateway Intent 설정

Bot이 메시지 내용을 읽으려면 Message Content Intent가 필요하다.

1. Discord Developer Portal 접속
2. Application 선택
3. 왼쪽 `Bot` 메뉴 클릭
4. 아래로 내려 `Privileged Gateway Intents` 찾기
5. 다음 항목 활성화

```text
Message Content Intent: ON
```

가능하면 아래 항목도 함께 확인한다.

```text
Server Members Intent: 필요 시 ON
Presence Intent: 일반적으로 필수 아님
```

이 프로젝트에서 핵심은 `Message Content Intent`이다.

---

## 9. Bot 초대 URL 만들기

Bot이 서버에 들어와 있어야 채널 메시지를 읽을 수 있다.

### 9.1 OAuth2 URL Generator 사용

1. Developer Portal에서 Application 선택
2. 왼쪽 `OAuth2` 클릭
3. `URL Generator` 클릭
4. `Scopes`에서 아래 선택

```text
bot
applications.commands
```

5. `Bot Permissions`에서 아래 권한 선택

```text
View Channels
Send Messages
Read Message History
Create Public Threads
Send Messages in Threads
Use Public Threads
Embed Links
```

6. 아래에 생성된 URL 복사
7. 브라우저에서 URL 열기
8. 현재 CS Desk 서버 선택
9. `Continue` 클릭
10. `Authorize` 클릭

### 9.2 초대 확인

초대가 완료되면 Discord 서버 멤버 목록에 Bot이 보여야 한다.

예시:

```text
n8n bot
```

Bot이 서버 멤버에 보이지 않으면 n8n에서 채널 접근 시 다음 오류가 발생할 수 있다.

```text
Unknown Channel
Missing Access
```

---

## 10. 채널 권한 설정

Bot이 서버에 있어도 채널 권한이 없으면 메시지를 읽지 못한다.

### 10.1 카테고리 권한 설정

채널이 카테고리 아래에 있다면 먼저 카테고리 권한을 확인한다.

1. CS Desk 카테고리 우클릭
2. `카테고리 편집` 클릭
3. `권한` 클릭
4. `역할 또는 멤버 추가` 클릭
5. `n8n bot` 선택
6. 아래 권한을 초록 체크로 허용

```text
채널 보기 / View Channel
메시지 기록 읽기 / Read Message History
메시지 보내기 / Send Messages
공개 스레드 만들기 / Create Public Threads
공개 스레드 사용 / Use Public Threads
스레드에서 메시지 보내기 / Send Messages in Threads
링크 첨부 / Embed Links
```

### 10.2 일반 채널 권한 설정

1. `일반` 채널 톱니바퀴 클릭
2. `권한` 클릭
3. `n8n bot` 추가
4. 위 권한을 동일하게 허용

### 10.3 긴급 채널 권한 설정

`긴급` 채널도 같은 방식으로 설정한다.

중요:

- 카테고리에서 권한이 거부되어 있으면 채널에서 허용해도 막힐 수 있다.
- 채널과 카테고리 모두에서 Bot 권한을 확인하는 것이 안전하다.

---

## 11. n8n Discord 알림 노드 설정

### 11.1 일반 CS 알림 노드

노드 이름:

```text
Discord 일반 CS 알림
```

설정:

```text
Method: POST
URL: 일반 채널 Webhook URL
Body type: JSON 또는 key/value
content: 일반 CS 알림 메시지 템플릿
```

메시지 예시:

```text
**[일반 CS 알림]**
**제목**: {{ 제목 }}
**이름**: {{ 이름 }}
**이메일**: {{ 이메일 }}
**카테고리**: {{ 카테고리 }}

**문의내용**
{{ 내용 }}
```

### 11.2 긴급 CS 알림 노드

노드 이름:

```text
Discord 긴급 CS 알림
```

설정:

```text
Method: POST
URL: 긴급 채널 Webhook URL
Body type: JSON 또는 key/value
content: 긴급 CS 알림 메시지 템플릿
```

메시지 예시:

```text
**[긴급 CS 알림]**
**제목**: {{ 제목 }}
**이름**: {{ 이름 }}
**이메일**: {{ 이메일 }}
**카테고리**: {{ 카테고리 }}

**문의내용**
{{ 내용 }}
```

---

## 12. Discord 쓰레드 답변 자동화 설정

### 12.1 스캔 주기 설정

노드 이름:

```text
[메일답변] 1분마다 Discord 답글 확인 - 설정 후 활성화
```

설정:

```text
Schedule Trigger
Interval: 1 minute
```

이 노드는 1분마다 Discord 채널의 최근 CS 알림과 쓰레드를 확인한다.

### 12.2 댓글 명령어 규칙

담당자는 Discord 쓰레드에 아래 형식으로 댓글을 작성한다.

```text
메일발송: 고객에게 보낼 답변 방향 또는 내부 메모
```

예시:

```text
메일발송: 빠르게 확인하겠다고 안내하고 불편을 드려 죄송하다고 답변해줘
```

### 12.3 댓글이 없을 때 동작

`메일발송:` 댓글이 없으면 다음 단계로 넘어가지 않는다.

```text
Discord 스캔
→ 메일발송: 댓글 없음
→ 처리할 항목 없음
→ 정상 종료
```

즉, 워크플로우 실행 로그는 남을 수 있지만 메일 발송이나 완료 알림은 실행되지 않는다.

### 12.4 댓글이 있을 때 동작

```text
Discord 스캔
→ 메일발송: 댓글 감지
→ 고객 이메일/제목/문의내용 추출
→ AI 고객 답변 생성
→ Gmail로 고객에게 메일 발송
→ Discord 쓰레드에 발송 완료 메시지 작성
```

완료 메시지 예시:

```text
✅ 정상적으로 발송 완료했습니다.
수신자: customer@example.com
제목: Re: 서비스 장애 발생 — 확인 요청
```

---

## 13. 테스트 방법

### 13.1 일반 알림 테스트

1. CS Desk 웹폼에서 카테고리를 `서비스 이용`으로 선택
2. 문의 제출
3. Discord `일반` 채널에 `[일반 CS 알림]` 메시지가 올라오는지 확인

### 13.2 긴급 알림 테스트

1. CS Desk 웹폼에서 카테고리를 `서비스 장애/오류`로 선택
2. 문의 제출
3. Discord `긴급` 채널에 `[긴급 CS 알림]` 메시지가 올라오는지 확인

### 13.3 답변 메일 테스트

1. Discord 알림 메시지의 쓰레드로 이동
2. 아래처럼 댓글 작성

```text
메일발송: 빠르게 확인하겠다고 안내하고 사과해줘
```

3. 최대 1분 기다림
4. 고객 이메일로 답변 메일이 도착했는지 확인
5. Discord 쓰레드에 발송 완료 메시지가 표시되는지 확인

---

## 14. 자주 발생한 오류와 해결 방법

| 오류 | 원인 | 해결 방법 |
|---|---|---|
| `Missing Access` | Bot이 채널을 볼 권한이 없음 | 카테고리/채널 권한에서 View Channel, Read Message History 허용 |
| `Unknown Channel` | 서버 ID를 채널 ID로 넣었거나 Bot이 서버에 없음 | 채널 우클릭으로 채널 ID 복사, Bot 서버 초대 확인 |
| `Cannot send an empty message` | Discord 메시지 body의 `content` 값이 비어 있음 | HTTP Request body에 `content` 필드 추가 |
| `메일발송:` 댓글을 못 읽음 | Message Content Intent 꺼짐 또는 권한 부족 | Developer Portal에서 Message Content Intent ON, Read Message History 권한 확인 |
| 긴급 문의가 분기 안 됨 | Switch 조건 텍스트 불일치 | 실제 입력값 `서비스 장애/오류`와 조건을 동일하게 설정 |
| 댓글이 없는데 계속 실행 로그가 남음 | 1분 주기 폴링 방식이라 정상 | 댓글이 없으면 스캔만 하고 종료되도록 구성 |

---

## 15. 보안 체크리스트

- [ ] Discord Bot Token을 GitHub에 커밋하지 않는다.
- [ ] Discord Webhook URL을 GitHub에 커밋하지 않는다.
- [ ] 스크린샷에 Token/Webhook URL이 보이면 가린다.
- [ ] Token이 노출되면 Developer Portal에서 즉시 Reset한다.
- [ ] Webhook URL이 노출되면 해당 Webhook을 삭제하고 새로 만든다.
- [ ] Bot 권한은 필요한 채널에만 최소 권한으로 부여한다.

---

## 16. 최종 Discord 설정 요약

| 항목 | 설정값 |
|---|---|
| 일반 채널 | 일반 CS 알림 수신 |
| 긴급 채널 | 서비스 장애/오류 알림 수신 |
| 일반 Webhook | `Discord 일반 CS 알림` 노드에서 사용 |
| 긴급 Webhook | `Discord 긴급 CS 알림` 노드에서 사용 |
| Bot Token | `[메일답변] 감시 채널/토큰 설정` 노드에서 사용 |
| Bot 권한 | View Channel, Read Message History, Send Messages, Thread 권한 |
| Message Content Intent | ON |
| 답변 명령어 | `메일발송:` |
| 스캔 주기 | 1분 |
