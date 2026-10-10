엔드게임 용산 4DX 잔여석 증가 감지봇 — Relay 버전
====================================================

왜 이 버전인가
--------------
Railway에서 CGV 자체를 직접 열었을 때:
- 일반 HTTP: 403
- Headless Chromium: CGV 첫 페이지부터 403

이 버전은 CGV를 Railway에서 직접 호출하지 않습니다.
공개 CGV timetable relay:
https://mcp.aka.page/api/cgv/timetable
에서 용산 10/11 시간표를 받아 15:20 엔드게임 4DX의
잔여석 숫자만 비교합니다.

로그인/좌석상세/홀딩/결제 기능은 없습니다.

알림 규칙
---------
3 -> 3 : 무시
3 -> 2 : 무시
2 -> 1 : 무시
1 -> 2 : 취소표 알림
2 -> 4 : 취소표 알림 (+2)

처음 시작했을 때 현재 잔여석은 기준값으로만 저장합니다.

GitHub에서 교체할 파일
----------------------
app.mjs
package.json
Dockerfile
railway.json
.env.example

Railway
-------
기존 새 Railway 서비스를 그대로 사용합니다.

Variables:
TELEGRAM_BOT_TOKEN=기존값
TELEGRAM_CHAT_ID=기존값

그 외 변수는 .env.example의 기본값 그대로 사용 가능.

필요 없는 것:
- Chromium
- Playwright
- Volume
- Public Networking
- CGV 로그인

정상 로그
---------
Starting relay-based Endgame Yongsan 4DX remaining watcher
Target: 20261011 1520 / 용산아이파크몰 / 엔드게임 / 4DX
Relay: https://mcp.aka.page/api/cgv/timetable
Baseline=...

정상 Telegram:
✅ [엔드게임 용산 4DX 잔여석봇] 감시 시작
현재 잔여석: N석

주의
----
Relay가 데이터를 캐시하면 알림 지연은 relay cache age만큼 생길 수 있습니다.
코드는 응답에 Age 헤더가 있으면 Telegram 시작 메시지와 로그에 표시합니다.
