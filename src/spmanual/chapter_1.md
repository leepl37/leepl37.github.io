# 1. 개요

본 문서는 모바일 신분증을 이용한 서비스를 제공하고자 하는 SP(Service Provider)에게 다음의 정보를 제공함을 목적으로 한다.

* 모바일 신분증 검증 서비스 개요
* 연계 절차
* SP가 준비해야 할 항목
* 모바일 신분증 수행단 제공정보 및 기타

# 1.1. 용어 

* mDL
	* Mobile Driver License의 약어
	* 모바일 운전면허증 모바일 신분증
* 모바일로 발급한 국가 신분증
	* 현재는 모바일 운전면허증(mDL)만 있음 신분증앱
* 신분증앱
	* "모바일 신분증 앱"의 줄임말 
* 블록체인 계정
	* 블록체인 노드 상에서 참여자를 식별하기 위한 계정
	* 스마트 컨트랙트 호출을 위해 필요 
* 서비스 코드
	* Holder가 제출한 VP를 검증하기 위해 블록체인에 등록한 서비스 구분자
	* 매칭 정보: Verifier의 블록체인 계정, VC type, 검증할 개인정보 리스트 등 
* CLI (Command Line Interface)
	* 명령창에서 실행 가능한 툴
	* Wallet 및 DID 생성 DID (Decentralized ID) 탈중앙화된 신원
* DID (Decentralized ID) 
	* 탈중앙화된 신원
* DID Document (DID 문서)
	* DID의 요소로서 블록체인에 등록되어 누구나 조회 가능한 문서
	* DID 소유자의 id (예 - did:kr:mobileid:1234567890) 및 공개키 등이 저장됨 
	* W3C의 Decentralized Identifier v.1.0을 준수
* Holder
	* Issuer가 발급한 VC를 소유하는 주체
* Issuer
	* VC를 발급하는 주체
* Verififer
	* Holder가 제출한 VP를 검증하는 검증자
	* 일반적으로 서비스를 제공하는 SP(Service Provider)가 verifier 역할을 수행함
* VC (Verifiable Credential)
	* Issuer(발급자)가 holder의 요청에 의해 holder의 개인정보를 증명 가능한 형태로 발급한 문서 W3C의 Verifiable Credential Data Model v.1.0을 준수
* VP (Verifiable Presentation)
	* Holder가 서비스를 제공받거나 기타 용도로 VC를 verifier에게 제출하기 위해 작성하고 서명한 문서 
	* 여러 발급자의 여러 VC를 하나의 VP에 담을 수도 있음
* Wallet
	* 개인키를 담고 있는 파일 형태의 암호화 지갑
	* DID의 개인키를 보관하고 있어 SDK 연동시 반드시 필요

 
