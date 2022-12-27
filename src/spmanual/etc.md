# 부록 


매 명령 실행 시마다 동일한 입력을 생략하기 위해 <span style="color:purple">omni-cli.properties</span>에 이를 등록할 수 있다. 설정은 아래와 같다.

Wallet 암호는 보안을 위하여 매번 실행 시마다 입력하는 것을 권장한다.(-p 옵션을 주면 명령 실행 전 암호 입력) 그러므로 최 초 생성 시 적용한 암호는 잘 기억해 두어야 한다.

### Wallet 생성

```console
1 | > omni-cli.bat keymanager createstore -p
2 |
3 | Enter value for --keymanagerpass (keymanager password): 
4 | create call/
5 | Command: keymanger->createstore
6 | keymanagerType DEFAULT
7 | KeyStore Generate Success:sp.wallet

```
<span style="color:purple">sp.wallet</span> 파일이 생성 되었음을 확인한다.


### ECC 키쌍 생성
서명용 ECC 키쌍을 생성하고 키 이름을 <span style="color:purple">omni.sp</span>로 지정한다. 이름은 자유롭게 변경이 가능하며 Verifier SDK 사용 시 설정파 일에 이 이름을 지정하면 된다.

```console
1 | > omni-cli.bat keymanager addkey -i omni.sp --keytype 0 -p 
2 |
3 | Enter value for --keymanagerpass (keymanager password):
4 | Command: keymanger->addkey
5 | keymanagerType DEFULT
6 | Unlock Success
7 | SECP256k1 key create
8 | Base58! = zBoekah8ZZAFPuystRhsKSi1biBY2TMzxSy4JcopqVoj
9 | HEX! = 0351aeb61260dd8b4c2090a470a52bcdc0470f40c68c637ceaaf8cdca334a096f6
```


* -i: 키 인덱스
	*  향후 이 키를 참조할 때 사용할 이름
* --keytype: 키 종류 
	*  0: ECC 키쌍 
	*  1: RSA 키쌍
### RSA 키쌍 생성
암호화용 RSA 키쌍을 생성하고 키 이름을 omni.sp.rsa로 지정한다. 이름은 자유롭게 변경이 가능하며 Verifier SDK 사용 시 설정파일에 이 이름을 지정하면 된다.


```console
1 > omni-cli.bat keymanager addkey -i omni.sp.rsa --keytype 1 -p 
2
3 Enter value for --keymanagerpass (keymanager password):
4 Command: keymanger->addkey
5 keymanagerType DEFULT
6 Unlock Success
7 SECP256k1 key create
8 Base58! = zBoekah8ZZAFPuystRhsKSi1biBY2TMzxSy4JcopqVoj
9 HEX! = 0351aeb61260dd8b4c2090a470a52bcdc0470f40c68c637ceaaf8cdca334a096f6

```

* -i: 키 인덱스
	* 향후 이 키를 참조할 때 사용할 이름
* --keytype: 키 종류 
	* 0: ECC 키쌍 
	* 1: RSA 키쌍

### 키 목록 확인
```console
1 | > omni-cli.bat keymanager list -p
2 |
3 | Enter value for --keymanagerpass (keymanager password):
4 | Command: keymanger->list
5 | keymanagerType DEFULT
6 | Unlock Success
7 | Key Size: 2
8 | Index1-KeyID:omni.sp
9 | Index1-PublicKey:zBoekah8ZZAFPuystRhsKSi1biBY2TMzxSy4JcopqVoj
10| Index2-KeyID:omni.sp.rsa
11| Index2-PublicKey:
  | 2TuPVgMCHJy5atawrsADEzjP7MCVbyyCA89UW6Wvjp9HrBayS3H4kT7RvSwrmodxQXyi
  | vW2R9wPXs166oTT27GirNbmpCPUGMofAhXv265oCwwGkDLpvR1NW23fjv9uEb8r6bJr4TcKxE6jGTnJ9GAEKn 
  | zDwFvrHoypnoneMfhdToPdmL5W3YAs7ojXKHCzL54ULoSyUndy5M3njgW1gffAbtMwMN5sBtbHhnPwyMNZcG3 
  | DHb2epW4w2C9zYfgNQYfcVXeWeHC7VpgV32ZUJk3HLZMH3WaAYA52rSwmHVmn7NrLYsAmb34nWgpUSDK2tU2P 
  | wMjFmADGWNbr99gVXYujCYDKLrzvNU5J3ogFd6HkqPkJNiQvn7CxPTJ1v1774XASKgv8B7L8bENbNnU

```

### DID Document 생성

DID Document를 생성하고 서명용 ECC 공개키를 추가한다.

```console
 1 | > omni-cli.bat did2 create -i omni.sp --keymethod 3 -s sp.did 
   |	--controller did:omn:3UGo7s2S3gC7doT7Ud3uo3rRJnab -p 
 2 | 
 3 | Enter value for --keymanagerpass (keymanager password): 
 4 | new did create call
 5 | Command: v2->create
 6 | keymanagerType DEFULT
 7 | {
 8 | 	"authentication": [
 9 | 	{
 10| 		"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp"
 11| 	} 
 12| 	],
 13| 	"controller": "did:omn:3UGo7s2S3gC7doT7Ud3uo3rRJnab", 
 14| 	"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ", 
 15| 	"proof": {
 16| 		"created": "2021-11-23T20:35:37",
 17| 		"creator": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp",
 18| 		"nonce": "99933519f4f50ff1111292b9234d1641ccc0f643",
 19| 		"signatureValue": "3rbzFqkcYRQkjbwtPFhb9VvekhwVja7amzm4pNqqXqHGBV5tZUVM8
   |					rB8KYyFoCKpyv6tSPUtRcZZvaV8HUtf9VcWT",
 20| 		"type": "Secp256k1VerificationKey2018"
 21| 		},
 22| 		"updated": "2021-11-23T20:35:37", "verificationMethod": [
 23| 		{
 24| 		"controller": "did:omn:3UGo7s2S3gC7doT7Ud3uo3rRJnab", 
   |				"expirationDate": "2021-12-03T23:59:59",
 25| 		"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp", 
   |		"publicKeyBase58": "zBoekah8ZZAFPuystRhsKSi1biBY2TMzxSy4JcopqVoj", 
   |		"status": "valid",
 26| 		"type": "Secp256k1VerificationKey2018"
 27| 		} 
 28| 	]
 29| 	}
 30| DidDocument Save: sp.did

```

* -i
	* 서명용 ECC 키 인덱스 
* --keymethod
	* 3으로 고정 
* -s
	* 생성할 DID Document 파일명
	* 원하는 파일명으로 생성 가능
* --controller
	* DID Document 수정 권한을 가진 DID id
	* 개발환경에서는 did:omn:3UGo7s2S3gC7doT7Ud3uo3rRJnab로 고정



### DID Document에 RSA 공개키 추가 

Wallet에 있는 암호용 RSA 공개키를 DID Document에 추가한다.


```console
 1 | > omni-cli.bat did2 addkey -i omni.sp.rsa --keymethod 4 -f sp.did -p
 2 | 
 3 | Enter value for --keymanagerpass (keymanager password): 
 4 | DID addkey Call......
 5 | Command: v2->addkey
 6 | keymanagerType DEFULT
 7 | Unlock Success 
 8 | {
 9 | 	"authentication": [ 
 10| 		{
 11| 			"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp" 
 12| 			}
 13| 		],
 14| 		"controller": "did:omn:3UGo7s2S3gC7doT7Ud3uo3rRJnab", 
 15| 		"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ", 
 16| 		"keyAgreement": [
 17| 			{
 18| 			"controller": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ",
 19| 			"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp.rsa", 
 20| 			"publicKeyBase58": "2TuPVgMCHJy5atawrsADEzjP7MCVbyyCA89UW6Wvjp9HrBayS3H4kT7RvSw
 21| 			rmodxQXyivW2R9wPXs166oTT27GirNbmpCPUGMofAhXv265oCwwGkDLpvR1NW23fjv9uEb8r6bJr4TcKxE6jG 
   | 			TnJ9GAEKnzDwFvrHoypnoneMfhdToPdmL5W3YAs7ojXKHCzL54ULoSyUndy5M3njgW1gffAbtMwMN5sBtbHhn 
   |			PwyMNZcG3DHb2epW4w2C9zYfgNQYfcVXeWeHC7VpgV32ZUJk3HLZMH3WaAYA52rSwmHVmn7NrLYsAmb34nWgp 
   | 			USDK2tU2PwMjFmADGWNbr99gVXYujCYDKLrzvNU5J3ogFd6HkqPkJNiQvn7CxPTJ1v1774XASKgv8B7L8bENb NnU",
 22| 			"status": "valid",
 23| 			"type": "RSAEncryptionKey" 
 24| 			}
 25| 		], 
 26| 		"proof": {
 27| 			"created": "2021-11-23T21:04:10",
 28| 			"creator": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp",
 29| 			"nonce": "5c25cd7a5e703adb07889e32543fe782d5c5dd20",
 30| 			"signatureValue": "3rA4HzUDuN1FMBxeFgsNb9z3TPqSzDTSo91TYWpQvJc6w3HNauvT1Y7TjoSJQu
 31| 			7MNsByyGirWsv9BNTtxHEw7y9Ag",
 32| 			"type": "Secp256k1VerificationKey2018"
 33| 			},
 34| 		"updated": "2021-11-23T21:04:10", "verificationMethod": [
 35| 			{
 36| 			"controller": "did:omn:3UGo7s2S3gC7doT7Ud3uo3rRJnab", 
 37| 			"expirationDate": "2021-12-03T23:59:59",
 38| 			"id": "did:kr:mobileid:43eiB4Qorv2hV93Bg7eKHdkUAYHZ#omni.sp", 
 39| 			"publicKeyBase58": "zBoekah8ZZAFPuystRhsKSi1biBY2TMzxSy4JcopqVoj", 
 40|	 		"status": "valid",
 41| 			"type": "Secp256k1VerificationKey2018"
 42| 			}
 43| 		]
 44| 	}
 45| DidDocument Save: sp.did
 46| 
```


* -i
	* 암호화용 RSA 키 인덱스 
* --keymethod
	* 4로 고정 
* -f
	* DID Document 파일명 


