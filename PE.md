# PE-01
![image](https://github.com/user-attachments/assets/54988558-252e-40ed-9e92-d27046133dc4)  
![image](https://github.com/user-attachments/assets/d3e38a69-be49-4863-a0e7-307f435f4ed6)  

2021-04-01 03:08:14
***

# PE-02
![image](https://github.com/user-attachments/assets/8d641584-5bc3-4d5d-8224-2a8b442d8b74)  

2021-04-01 03:08:14を基準にcorp.webdevのイベントログに注目する。
![image](https://github.com/user-attachments/assets/60102b12-0ddd-4b09-9e0b-1db68311b5bd)

  
すると、「ALIEN」のドメインユーザが「An account was successfully logged on.（アカウントが正常にログオンされました。）」とある。  
「ALIEN」でGrepすると、  
![image](https://github.com/user-attachments/assets/fcf04ae1-84b2-4e0e-975d-f3c22e3f45a7)  

「A security-enabled local group was changed.（セキュリティが有効なローカルグループが変更されました。）」ということから

![image](https://github.com/user-attachments/assets/5485adcb-32ce-4563-af61-7025f6543ed3)
```
Account Name:		CORP-WEBDEV$
Account Domain:		ALIEN
```
が何かしらしている可能性を視野に入れながら調査。

ここで、「A security-enabled local group membership was enumerated.（セキュリティが有効なローカルグループメンバーシップが列挙された。）」という文字列から探索されたユーザがわかるかもしれないと考える。
```
	Account Name:		CORP-WEBDEV$
	Account Domain:		ALIEN
```
```
	Group Name:		Administrators
	Group Domain:		Builtin
```
```
	Account Name:		alien
	Account Domain:		IIS APPPOOL
```
```
	Account Name:		Administrator
	Account Domain:		CORP-WEBDEV
```
これらが探知された可能性のあるもの。
ここで、「IIS APPPOOL」のドメイングループで検索すると
![image](https://github.com/user-attachments/assets/f12a9e57-0297-4c35-8a10-7f1c215aa869)  
３月ごろまではlocalhostに向けた操作しかしていなかったのに急に
orp-webdev.alien.localに対して操作を始めている。

よって、

**IIS APPPOOL\alien**


***
# PE-03
![image](https://github.com/user-attachments/assets/b3772446-e598-4722-b504-69302e7da88c)

![image](https://github.com/user-attachments/assets/cda9413e-d5b2-4f59-9b44-5186a8951df5)

DMZと比較したときに、「alien_db」だけが権限付きで唯一存在しているため、狙うならこのアカウントだと考えた。


