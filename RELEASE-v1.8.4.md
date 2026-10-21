# v1.8.4 · 모든 시도교육청 K-에듀파인에서 동작합니다

충북이 아닌 시도교육청의 K-에듀파인에서 확장이 붙지 않던 문제를 고쳤습니다. 도우미의 보안 문제([이슈 #1](https://github.com/jkf87/klef-collector-releases/issues/1))도 함께 고쳤습니다. 요약 결과와 저장 방식은 1.8.3과 같습니다.

## 바뀐 점

- **17개 시도교육청 K-에듀파인 주소를 모두 엽니다.** 지금까지 확장은 충북 주소(`klef.cbe.go.kr`)에서만 공람 요약 패널을 띄웠습니다. 다른 지역에서는 공람 목록을 열어도 아무것도 나타나지 않았습니다. 이제 아래 17곳에서 똑같이 동작합니다.
- **도우미가 `Origin: null` 요청을 받지 않습니다.** `null` 은 어떤 웹사이트든 sandbox iframe 하나로 얻는 값이라, 도우미가 켜져 있는 동안 아무 페이지나 요약 목록을 읽고 요약 실행을 걸 수 있었습니다. 확장은 이 값을 쓰지 않으므로 뺐습니다.
- **도우미가 `127.0.0.1`·`localhost` 가 아닌 주소로 온 요청을 거절합니다.** DNS 리바인딩으로 바깥 사이트가 도우미를 부르는 길을 막습니다.

| 시도 | 주소 | 시도 | 주소 |
|---|---|---|---|
| 서울 | klef.sen.go.kr | 강원 | klef.gwe.go.kr |
| 부산 | klef.pen.go.kr | 충북 | klef.cbe.go.kr |
| 대구 | klef.dge.go.kr | 충남 | klef.cne.go.kr |
| 인천 | klef.ice.go.kr | 전북 | klef.jbe.go.kr |
| 광주 | klef.gen.go.kr | 전남 | klef.jne.go.kr |
| 대전 | klef.dje.go.kr | 경북 | klef.gbe.kr |
| 울산 | klef.use.go.kr | 경남 | klef.gne.go.kr |
| 세종 | klef.sje.go.kr | 제주 | klef.jje.go.kr |
| 경기 | klef.goe.go.kr | | |

## 설치·업데이트

[설치 안내 페이지](https://jkf87.github.io/klef-collector-releases/)를 따라 하세요.

- **설치 파일로 쓰는 분:** 새 `klef-helper-setup.exe`를 실행하면 업데이트됩니다. 요약 기록·설정·받은 모델은 그대로 둡니다.
- **ZIP 폴더로 쓰는 분:** 도우미를 끄고 `klef-worker-1.8.4.zip`의 파일로 같은 폴더의 코드를 바꿉니다. `summaries.json`, `.native-save-ledger.json`, `.cache`는 그대로 둡니다. 그다음 `시작.cmd`를 실행합니다.
- **확장도 바꿔야 합니다.** 주소 목록이 확장에 들어 있습니다. 압축해제 폴더로 쓰는 분은 새 ZIP의 `extension` 폴더로 바꾼 뒤 `chrome://extensions`에서 새로고침합니다. 접속 허용 주소가 늘어서 Chrome 이 확장을 잠시 끄고 권한 승인을 다시 묻습니다. 허용하면 다시 켜집니다.

## 확인

Windows에서 자동 테스트 225개와 배포 전 검사를 통과했습니다. 17개 주소는 여러 공개 K-에듀파인 도구가 쓰는 주소를 모아 DNS로 확인했습니다. 충북 외 지역의 실제 공람 화면에서는 아직 시험하지 못했습니다. 다른 지역에서 패널이 뜨지 않으면 [문의](https://github.com/jkf87/klef-collector-releases/issues)에 시도와 주소창의 도메인만 남겨 주세요.
