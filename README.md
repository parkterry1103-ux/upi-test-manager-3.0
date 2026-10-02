# UPI 거래 테스트 매니저

UnionPay 가맹점 거래 테스트 결과를 카드 종류와 결제 방식별로 기록하고,
Google Sheets 기반 데이터베이스에 저장하는 모바일 친화형 정적 웹 앱입니다.

## 실행 방법

별도 빌드 과정은 없습니다. 저장소 루트에서 정적 파일 서버를 실행한 뒤
브라우저로 접속하면 됩니다.

```powershell
python -m http.server 8000
```

브라우저에서 `http://localhost:8000`을 엽니다.

## Google Sheets 연결

앱은 기본적으로 공유 Google Sheet인 `1. 테스트 어플 DataBase`의
Acceptance용 Apps Script 웹 앱에 연결됩니다. 웹에서
`업로드 및 저장`을 실행하면 `saveSession` 요청을 통해 거래 결과가
`Transactions` 시트에 등록됩니다.

앱 우측 상단의 설정에서 다른 Google Apps Script 웹 앱 URL로 변경할 수
있으며, 변경한 URL은 해당 브라우저의 `localStorage`에 저장됩니다.

## 카드정보 관리

- 카드번호는 공개 소스에 포함하지 않고 브라우저 `localStorage`에 저장합니다.
- 홈 화면 설정에서 비공개 카드정보 JSON을 가져오거나 내보낼 수 있습니다.
- 새 컴퓨터나 휴대폰에서는 설정의 공유 Drive 버튼으로 팀 카드정보 파일을 받은 뒤 한 번 가져오면 됩니다.
- 카드 구분과 결제 방식을 선택하면 해당하는 카드 목록만 마스킹하여 표시합니다.
- 직접 입력한 카드번호는 해당 브라우저의 선택 목록에 자동 추가됩니다.
- QR과 HCE는 Debit/Credit 실물 IC·NFC 카드 목록을 합쳐 표시합니다.
- 같은 테스트 결과를 여러 번 저장해도 브라우저의 중복 방지 기록으로 한 번만 전송합니다.

## 파일 구성

- `index.html`: 현재 웹 앱 전체 코드
- `manifest.json`: PWA 설치 정보
- `Icon.png`, `Logo.png`, `icon-192.png`, `icon-512.png`: 앱 이미지
- `archive/UPI-test-manager-3.0.html`: 이전 HTML 버전 참고본
- `private-data/`: Git에 올리지 않는 실제 DB와 연결 정보

## 보안 주의사항

원본 Excel 데이터베이스에는 거래 기록과 카드번호 형식의 값이 포함되어
있어 GitHub 저장소에서 제외했습니다. 실제 Google Apps Script 배포 URL도
`private-data/`에만 보관하며 커밋하지 않습니다.

카드번호, 거래 데이터, 운영 중인 Apps Script URL은 공개·비공개 여부와
관계없이 Git에 커밋하지 마세요.

카드정보 JSON은 팀 내부의 접근 제한된 저장소로만 전달하고 공개 GitHub에는
절대 업로드하지 마세요.

공유 Drive의 카드정보 파일은 현재 거래 DB와 같은 팀 폴더에 있으며, 해당
폴더의 승인된 사용자만 열 수 있습니다.
