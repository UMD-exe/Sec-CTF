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
![image](https://github.com/user-attachments/assets/c298aafd-2838-4dcf-ad1b-ab50770ed153)

３月ごろまではlocalhostに向けた操作しかしていなかったのに急に
orp-webdev.alien.localに対して操作を始めている。

よって、

**IIS APPPOOL\alien**


***
# PE-03
![image](https://github.com/user-attachments/assets/b3772446-e598-4722-b504-69302e7da88c)

![image](https://github.com/user-attachments/assets/cda9413e-d5b2-4f59-9b44-5186a8951df5)

DMZと比較したときに、「alien_db」だけが権限付きで唯一存在しているため、狙うならこのアカウントだと考えた。  

***
# PE-04
![image](https://github.com/user-attachments/assets/a6903af2-3880-4166-9d2c-c87ff3c6f64a)

「alien_db」が最初にログインしたときのものを探してUTFに変換
![image](https://github.com/user-attachments/assets/a47d5c84-5e9a-43db-af28-e2068c0e5127)

**2021-04-01 03:29:23**

***
# PE-05
![image](https://github.com/user-attachments/assets/750f3364-4fb6-4523-891c-6a771e14a27a)

ユーザが「alien_db」が作らせる前にタスクが生成されている。
おそらく永続化のための処理
![image](https://github.com/user-attachments/assets/479badf7-741c-4365-8667-d1fdf6d19dcd)  

ファイル名が「Windows NUPdate」のタスクをイベントログ「Microsoft-Windows-TaskScheduler%4Maintenance」から探すと  
![image](https://github.com/user-attachments/assets/7a5015c3-b561-4d19-ba9e-894ba2007712)  

2121/04/01 12:29:23なのでUTFに直して **2121/04/01 12:29:23**

***
# PE-6
![image](https://github.com/user-attachments/assets/07123341-55d0-4b71-9aff-ae5d8470fdbe)

![image](https://github.com/user-attachments/assets/d172d755-06b7-49de-a8d2-6d64a2bcb368)


