# 問題GS-0
<img src=https://github.com/user-attachments/assets/46bd6c7a-6474-41a3-a4d2-59553a5e7a40 width="400">


# 解法
7Zipで結合すると下記のようにファイルが形成される。  
<img src=https://github.com/user-attachments/assets/03aa1a32-b12a-4d11-b1fc-9c494b11279b width="400">  

これがフラグ↓  
<img src=https://github.com/user-attachments/assets/4699663c-78a2-4df8-81c2-4dbe00022a1c width="400">

***

# 問題GS-1
<img src=https://github.com/user-attachments/assets/fab807a3-c876-4d02-b017-690a98ad052b wdth="200">

<img src=https://github.com/user-attachments/assets/1fa411d9-5c85-445d-b816-229b444cefc6 wdth="300">

9個

***
# 問題GS-2
<img src=https://github.com/user-attachments/assets/cd05f457-e1e9-4abf-8219-3c1f0e942db8 width="300">

# 解法
<img src=https://github.com/user-attachments/assets/208516bb-8683-4ef4-9e99-ba4e466e14ea width="500">

```
certutil -hashfile memory.raw MD5
```

20b25f76cc1839c2e7759a69a82bf664

***
# 問題GS-3
<img src=https://github.com/user-attachments/assets/917f8d70-ca0f-4dd4-b19d-3f10793b894d width="300">

# 解法
<img src=https://github.com/user-attachments/assets/019786dd-2c09-4f7d-90c7-0c690a21b54d width="500">  

2021-04-06 10:56:57 JST+0900と判明したが、UTCで問われているので-9をして

2021-04-06 01:56:57が答え

***
# 問題GS-4
<img src=https://github.com/user-attachments/assets/ce30f670-78b2-48c3-aa05-e9a5afcfe0f6 width="300">  

一般に公開するWebサイトが第一のヒントっぽいのでおそらく外向けのサービスだろうと考える。  
dmz-webpub.alien.localに何かしらのログがヒント  
<img src=https://github.com/user-attachments/assets/c6224932-125b-4693-aeb4-e9cf1e3c4450 width="700">   
どれでもいいのでなにか特徴がないか見てみる。    
<img src=https://github.com/user-attachments/assets/8f00a6eb-555a-4355-9749-69954fbabaeb width="700">  

「/Default.aspx」というWebに関係ありそうな拡張子（Aspxファイルは主にWebアプリケーションで使用され、動的なコンテンツを提供するために作られます。）があり調べてみるとASP.NETベースのCMSっぽい。

代表例として「Orchard、DotNetNuke、Umbraco」などがあるらしい。

総当たりしてみたら答えは【DotNetNuke】であった。




