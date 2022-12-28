### 4. SP 서버 개발

SP 라이브러리 샘플소스코드 : [개발지원센터](https://dev.mobileid.go.kr/mip/dfs/downapi/useguidedown.do).


### 4.1 설치 및 서버 구동

1. Eclipse 또는 Intellij 를 사용하여 해당 프로젝트를 실행.
2. IDE 툴에서 jdk 1.7 또는 jdk 1.8 로 설정.
3. IDE 툴 플로그인에서 spring boot 다운 후 maven build 실행



### 4.2 application.properties 설정

#### 1. Server Settings 

|key | config-value| 비고|
|-|-|-|
|app.blockchain-server-domain| 개발과 운영에 맞는 주소를 설정한다.|
|app.sp-server| 서비스할 주소를 입력|
|app.proxy-server| proxy 주소 입력| cpm 의 경우 사용
| app.push-server-domain| push server 주소 입력 | push 사용 시




#### 2. SP & Wallet 설정

|key|config-value|
|-|-|
|app.keymanager-path|생성한 wallet을 해당 경로로 지정한다. <br> - app.keymanager-path=./example_op.wallet|
|app.keymanager-password| 생성 시 입력한 패스워드 입력. <br> - app.keymanager-password=raon1234
|app.sp-key-id| 개발 지원 센터로 부터 안내 받은 계정 ?| 
| app.sp-rsa-key-id | ? |
| app.sp-account | 개발지원센터로 부터 안내받은 계정?|
| app.sp-did-path | 생성한 did의 위치를 저장한다. <br> app.sp-did-path=./example_op.sp.did |
| app.sp-bi-image-url | 검증 시 보이는 이미지 url 주소<br> app.sp-bi-image-url=https://www.mobileid.go.kr/resources/images/main/mdl_ico_homepage.ico |
| app.sp-bi-image-base64 | 이미지에 대한 값을 base64로 인코딩|
| app.include-profile | ? |
| app.sp-ci | ci 값을 안내받기 위한 여부|
| app.issuer-check-vc | 서명 검증 여부 |

질문 1. zpk, sdk, log, db, boot settings ??







