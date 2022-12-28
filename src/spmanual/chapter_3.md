### 3. 연계절차

모바일 신분증 서비스 신청 및 등록 절차는 다음 그림과 같다. 

<p><img src="./img/3-Verifier (SP).png"alt="url_img" /> </p>

### 3.1. Wallet, DID 생성

Wallet과 DID는 수행단이 제공한 CLI 툴을 이용한다. Wallet은 생성된 키를 암호화하여 저장하는 파일이므로 SP가 안전하게 보관하여야 한다. CLI 툴로 생성한 DID는 sp.did(생성 시 파일명 지정 가능)라는 파일에 로컬로만 만들어지는 것이므로 이를 블록체인에 등록하여야만 실제 사용이 가능하다. DID 등록은 Admin 권한을 가진 자만 가능하므로 수행단에 제출하여 등록을 의뢰하여야 한다.


Wallet&DID 생성 파일 다운로드링크 [생성파일](https://dev.mobileid.go.kr/mip/dfs/downapi/useguidedown.do)
- SP 서버 Wallet&DID 생성 가이드 클릭 후 다운로드.


CLI 툴은 윈도우와 리눅스 운영체제에서 모두 실행이 가능하며 각각 아래의 단축 명령을 이용한다. 
* 윈도우: omni-cli.bat
* 리눅스: <span style="color:blue">omni-cli.sh</span>



#### 개발/운영 구분에 따라 controller DID 변경

* 개발 : did:kr:mobileid:3pnuR77qKLZWnu4kaw4qmVtGcoZt

* 운영 : did:kr:mobileid:4VmgGJ3geNyyKRKupXDiCkw1kKw5



#### -- for Linux
---------------------------------------------------------------------------------

```console

// Wallet 생성 => sp.wallet

./omni-cli.sh keymanager createstore -p
./omni-cli.sh keymanager addkey -i omni.sp --keytype 0 -p
./omni-cli.sh keymanager addkey -i omni.sp.rsa --keytype 1 -p
./omni-cli.sh keymanager list -p


// DID Document 생성 (controller did 수정 금지) => sp.did   *controller did 값은 위에 입력된 개발과 운영에 따라 구분하여 입력함

//**운영용
./omni-cli.sh did2 create -i omni.sp --keymethod 3 -s sp.did --controller did:kr:mobileid:4VmgGJ3geNyyKRKupXDiCkw1kKw5 -p
//**개발용
./omni-cli.sh did2 create -i omni.sp --keymethod 3 -s sp.did --controller did:kr:mobileid:3pnuR77qKLZWnu4kaw4qmVtGcoZt -p

// 키(RSA) 추가 
./omni-cli.sh did2 addkey -i omni.sp.rsa --keymethod 4 -f sp.did -p

// 패스워드 변경시
./omni-cli.sh keymanager changepwd -m example_op.wallet -p -n


```

#### -- for Windows
---------------------------------------------------------------------------------

```console

// Wallet 생성 => sp.wallet

omni-cli.bat keymanager createstore -p
omni-cli.bat keymanager addkey -i omni.sp --keytype 0 -p
omni-cli.bat keymanager addkey -i omni.sp.rsa --keytype 1 -p
omni-cli.bat keymanager list -p

// DID Document 생성 (controller did 수정 금지) => sp.did   *controller did 값은 위에 입력된 개발과 운영에 따라 구분하여 입력함
//**개발용 
omni-cli.bat did2 create -i omni.sp --keymethod 3 -s sp.did --controller did:kr:mobileid:3pnuR77qKLZWnu4kaw4qmVtGcoZt -p
//**운영용
omni-cli.bat did2 create -i omni.sp --keymethod 3 -s sp.did -controller did:kr:mobileid:4VmgGJ3geNyyKRKupXDiCkw1kKw5 -p

// 키(RSA) 추가 
omni-cli.bat did2 addkey -i omni.sp.rsa --keymethod 4 -f sp.did -p

// 패스워드 변경시
omni-cli.bat keymanager changepwd -m example_op.wallet -p -n

```

* 더 자세한 설명을 원하시면 [부록](#etc.md)을 참고해 주시기 바랍니다. 


### 3.2. 신청서 작성

1. 신청서 양식 다운로드 : [개발지원센터](https://dev.mobileid.go.kr/mip/dfs/downapi/formdown.do).

2. 다운로드 후 신청서를 작성한다. 
 
신청서 작성에 대한 자세한 문의는 조폐공사 이승훈 과장님(1042-214-1231)에게 문의한다. 

### 3.2.1 앱 테스트를 위한 엑셀파일 및 개인정보 동의서 작성

1. 앱 테스트를 위한 엑셀파일 작성한다.

2. 개인정보 동의서를 작성한다.


### 3.2.2 제출

mid_apply@komsco.com 으로 생성한 DID 와 신청서, 개인정보 동의서, 작성된 엑셀을 제출한다.


### 3.2.3 서비스코드와 sp계정 & 테스터 등록

모바일신분증 운영단은 mid_apply@komsco.com 메일로 받은 did 파일을 블록체인 노드에 등록 후
해당 did에 대한 서비스코드와 sp 계정을 회신한다. 

* 이때 회신 메일에 적힌 sp 계정을 sp 서버 소스 코드 설정 파일에 입력한다. 

모바일신분증 운영단은 mid_apply@komsco.com 메일로 받은 엑셀에 대한 정보를 가지고 앱을 다운로드 받을 수 있는
링크를 전송하는데 이 링크를 통해 sp 는 테스트를 진행할 수 있다. 

앱 설치 및 테스트는 링크 전송 시 파일을 첨부하여 안내된다. 

