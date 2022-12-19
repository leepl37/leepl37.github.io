# 1.2.3. 수집정보(Claim)

## 1.2.3.1 Claim 리스트

SP는Holder에게 다음의 정보(Claim)를 VP로 제출하도록 요청할 수 있다.

**[운전면허증VC]**

|코드|이름|타입|예시|
|-|-|-|-|
|name|이름|string|"홍길동"|
|ihidnum|주민등록번호|string|"8601021111111"|
|address|주소|string|"서울특별시종로구 XX대로 103"|
|birth|생년월일|string|"860102"|
|dlno|운전면허증번호|string|"10-20-123456-11"|
|asort|면허종별|(구분자사용)|string|"2종보통,2종원동기"|
|inspctbegend|적성검사시작,종료일자(구분자사용)|string|"20220101,20221231"|
|issude|발급일자|string|"20141010"|
|locpanm|지방경찰청명의|string|"서울경찰청장"|
|passwordsn|암호일련번호|string|"A1XY28"|
|inorgdonnyn|장기기증여부(구분사용)|string|"Y"|
|dlphotoimage|운전면허증사진이미지|hexstring|"2f396a...6b3d"|
|lcnscndcdnm|면허조건명(구분자사용)|string|"A,D"|
**[영지식VC]**
|코드|이름|설명|
|-|-|-|
|zkpaddr|영지식주소(시/군/구/동)|상세주소|없음|
|zkpsex|영지식|성별|
|zkpasort|영지식|면허종별(구분자사용)|
|zkpbirth|영지식|생년월일||



# 1.2.3.2. 제출 방식
서비스신청서에 제출받을 claim을 선택하여 기재한다. VP 제출 방식은 다음 2가지가 있다.
※일반 VC와 영지식 VC를 섞어서 제출할 수 없음

* 일반VP
	* 운전면허증VC에서 제출받을 claim을 선택
* Holder의DID와 서명이 포함되므로 제출자 식별이 가능
* 영지식VP
	* 영지식VC에서 제출받을 claim을 선택
	* Holder정보가 전혀 포함되지 않아 제출자 식별이 불가능
	* 예를들어, "영지식 생년월일(zkpbirth)"을 제출받을 경우 SP가 제시한 기준일보다 이전에 태어났는지 파악 가능
