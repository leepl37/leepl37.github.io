### 3.1.1. Wallet 생성

<p><img src="./img/3.1.1.png" alt="wallet 생성 커맨드" /> </p>

### 3.1.2. ECC 키쌍 생성
서명용 ECC 키쌍을 생성하고 키 이름을 <span style="color:purple">omni.sp</span>로 지정한다. 이름은 자유롭게 변경이 가능하며 Verifier SDK 사용 시 설정파 일에 이 이름을 지정하면 된다.

<p><img src="./img/3.1.2.png" alt="ECC 키쌍 생성" /> </p>

* -i: 키 인덱스
	*  향후 이 키를 참조할 때 사용할 이름
* --keytype: 키 종류 
	*  0: ECC 키쌍 
	*  1: RSA 키쌍
### 3.1.3. RSA 키쌍 생성
암호화용 RSA 키쌍을 생성하고 키 이름을 omni.sp.rsa로 지정한다. 이름은 자유롭게 변경이 가능하며 Verifier SDK 사용 시 설정파일에 이 이름을 지정하면 된다.

<p><img src="./img/3.1.3.png" alt="RSA 키쌍 생성" /> </p>

* -i: 키 인덱스
	* 향후 이 키를 참조할 때 사용할 이름
* --keytype: 키 종류 
	* 0: ECC 키쌍 
	* 1: RSA 키쌍

