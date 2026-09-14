# Chrome 웹 스토어 등록 준비

확인일: 2026-09-14 · 현재 배포: 1.2.3 · 아직 웹 스토어 등록/승인되지 않았습니다.

## 1. 개발자 계정

[Chrome Developer Dashboard](https://chrome.google.com/webstore/devconsole)에 사용할 Google 계정으로 가입합니다. 개발자 이메일을 확인하고 2단계 인증, 개발자 약관 동의, 일회성 등록비 결제를 직접 완료하세요. 금액과 추가 본인 확인 항목은 실제 계정 화면에서 확인합니다. [공식 계정 등록 안내](https://developer.chrome.com/docs/webstore/register), [2단계 인증 등 정책](https://developer.chrome.com/docs/webstore/program-policies/policies).

## 2. 현재 남은 준비물

- 확장 ZIP 안에 PNG 아이콘을 넣고 `manifest.json`의 `icons`로 연결합니다. 현재 1.2.3에는 아이콘이 없으므로 그대로 심사 제출하지 마세요. 128×128 아이콘과 브라우저 표시용 16/32/48 크기를 준비하고 패키징 허용 목록에도 이미지 파일을 추가해야 합니다.
- 실제 기능을 보여주는 개인정보 없는 1280×800 스크린샷(최소 1장)과 440×280 작은 홍보 이미지를 준비합니다. 실 공문 대신 가상 문서로 시연하고 타 기관 로고를 허가 없이 사용하지 않습니다. 영상은 선택 사항입니다. [공식 이미지 요구사항](https://developer.chrome.com/docs/webstore/images).
- 개인정보처리방침과 아래 권한·데이터 설명이 제출 직전 코드와 일치하는지 배포자가 확인합니다. 외부 API 옵션이 있으므로 무조건 ‘데이터를 전혀 처리하지 않는다’고 신고하면 안 됩니다. 로컬 처리도 설명 대상입니다. [개인정보 FAQ](https://developer.chrome.com/docs/webstore/program-policies/user-data-faq).
- 내부 기관 사이트 기능의 재현 방법을 준비합니다. 일반 웹 선택 요약만으로 공람 저장 기능까지 검증되는 것은 아닙니다. 실 계정 비밀번호를 공개하거나 접근 제한을 우회하지 말고, 허가된 테스트 환경·계정 또는 심사팀이 요구하는 별도 시연 자료를 준비하세요.

## 3. 업로드할 파일

준비가 끝나면 새 버전의 **`klef-extension-버전.zip`만** 대시보드의 새 항목에 업로드합니다. ZIP 맨 위에 `manifest.json`이 있어야 합니다. 워커 ZIP, Node.js, 모델, `.cmd`를 확장 ZIP에 섞지 마세요. 워커는 GitHub Releases에서 별도로 제공합니다. [공식 게시 절차](https://developer.chrome.com/docs/webstore/publish).

스토어에서 설치한 확장 ID는 압축 해제 설치 때와 다릅니다. 업로드 후 정식 ID를 확인하고 네이티브 호스트의 `allowed_origins`에 그 ID를 등록해야 합니다. 현재는 워커 폴더에서 아래 명령으로 등록할 수 있습니다.

```powershell
npm.cmd run install-native-host -- --extension-id <스토어의-32자-ID>
```

웹 스토어 설치만으로 PC의 도우미가 설치되거나 네이티브 호스트가 등록되지는 않습니다. 수동 `start-klef.cmd` 방식도 계속 지원됩니다. [공식 Native Messaging 안내](https://developer.chrome.com/docs/extensions/develop/concepts/native-messaging).

## 4. 스토어 설명 초안

이름: KLEF 공람문서 요약

단일 목적: 사용자가 열람 권한을 가진 공문 또는 직접 선택한 텍스트를 요약해 브라우저에서 핵심 내용을 확인하도록 돕습니다.

설명: 공문 목록에 요약·조치·기한·대상을 표시하고, 웹페이지에서 선택한 본문을 우클릭으로 요약합니다. 기본은 PC의 CPU 로컬 모델이며 로컬 도우미와 Node.js의 별도 설치가 필요합니다. 공문 요약에는 선택형 외부 API와 실험적인 Chrome 내장 AI를 지원합니다. 외부 API는 사용자가 설정하고 전송에 동의한 경우에만 이용하며 제공자 요금이 발생할 수 있습니다. 기관/Google의 공식 제품이 아니고 DRM 해제 기능은 없습니다. 작은 모델의 요약에는 사실·대상·기한 오류가 있을 수 있습니다.

홈페이지: https://github.com/jkf87/klef-collector-releases

지원 URL: https://github.com/jkf87/klef-collector-releases/issues

개인정보처리방침 URL: https://github.com/jkf87/klef-collector-releases/blob/main/docs/PRIVACY.md

## 5. Privacy 탭 입력 근거

| 권한 | 실제 사용 이유 |
| --- | --- |
| `storage` | 요약 로컬 캐시와 선택 요약의 탭별 세션 상태 저장 |
| `contextMenus` | 사용자가 선택한 본문의 우클릭 요약 메뉴 |
| `activeTab` | 사용자 요청 시 현재 탭에만 선택 요약 패널 표시 |
| `scripting` | 확장에 포함된 선택 요약 UI 코드를 현재 탭에 주입 |
| `nativeMessaging` | 사용자가 등록한 도우미의 상태 확인·실행·중지 |
| `https://klef.cbe.go.kr/*` | 사용자가 접근한 공람 목록과 로컬 요약을 연결·표시 |
| `http://127.0.0.1:7654/*` | 같은 PC의 도우미에 요약·설정·저장 상태 요청 |

데이터 항목은 웹사이트 콘텐츠, 본문에 포함될 수 있는 개인정보, API 인증 정보 처리 여부를 실제 UI 정의와 대조해 배포자가 신고합니다. 선택 탭 URL은 로컬 세션 확인용이고 일반 방문 이력 수집은 하지 않습니다. 동의 없이 원격으로 보내지 않는다는 사실과 데이터를 아예 취급하지 않는다는 주장은 다릅니다.

확장 자체의 JS는 패키지에 포함되며 원격 JS를 받아 실행하지 않습니다. 도우미가 별도 프로세스로 실행하는 모델·런타임 다운로드와 API 요청은 심사 설명에서 숨기지 않습니다. 모델이 반환한 결과를 실행 코드로 평가하지 않습니다. Native Messaging을 사용한다는 사실만으로 심사가 자동 승인되는 것은 아닙니다. [권한·데이터 입력 안내](https://developer.chrome.com/docs/webstore/cws-dashboard-privacy), [Manifest V3 정책](https://developer.chrome.com/docs/webstore/program-policies/mv3-requirements).

## 6. 심사 재현 안내 초안

1. Windows x64 PC에 Node.js 24.14.0 이상을 설치합니다.
2. 공개 릴리즈의 워커 ZIP 전체를 풀고 `start-klef.cmd`를 실행합니다. 초기 의존성 설치와 모델 다운로드에 인터넷 및 디스크 공간이 필요합니다.
3. `http://127.0.0.1:7654/health`의 버전을 확인합니다. 다른 PC의 도우미를 연결하는 구조가 아닙니다.
4. 확장 AI 설정에서 로컬 모델을 선택하고 ‘이 모델 준비하기’를 완료합니다. 실제 모델별 다운로드 용량과 CPU 처리 시간을 스토어 설명에 명시합니다.
5. ‘가상 문장으로 시험하기’로 연결을 확인하고, 일반 HTTP/HTTPS 페이지에서 공개 텍스트 일부를 선택하여 우클릭 → 선택 본문 요약을 실행합니다. 종료한 패널이 다시 나타나지 않는지, 도우미를 끄면 설치 안내가 나오는지 확인합니다.
6. 공람 저장·목록 기능은 허가된 테스트 접근과 비식별 시연 자료를 별도로 제공합니다. 일반 웹 선택 시험만으로 이 부분의 테스트를 대체했다고 주장하지 않습니다.

Store listing, Privacy, Distribution, Test instructions를 완료한 후 심사 요청합니다. 승인 후 수동 공개를 선택할 수 있습니다. 심사 기간이나 승인을 보장할 수 없으며, 계정 등록·약관 동의·결제·심사 제출은 배포자가 확인하고 진행합니다.
