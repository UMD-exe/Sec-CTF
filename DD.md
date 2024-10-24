# DD-1
![image](https://github.com/user-attachments/assets/6c83db53-327c-4e79-a601-77cd28faee0b)  

問題的に何かしらのマッピングするツールをダウンロードしている。
とりあえずalien_dbのログをさかのぼると
![image](https://github.com/user-attachments/assets/27c32a73-ec7e-4a82-bd64-587e445f18a5)

あからさまにマッピングしそうな名前のZipファイルがある。
sample3＝psmap.zip

**2021-04-01 04:10:20**

***
# DD-2
![image](https://github.com/user-attachments/assets/2ac92264-ff02-4b22-92fe-2d1939ae90c7)

そもそもタスクにどのような設定がされたのか「C:\work\ACSC\Files\corp-webdev.alien.local\C\Windows\System32\Tasks\Windows YUPdate」を確認してみる
```Windows YUPdate
  <RegistrationInfo>
    <Date>2021-04-01T05:10:54</Date>
    <Author>IIS APPPOOL\alien</Author>
    <URI>\Windows YUPdate</URI>
  </RegistrationInfo>
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2021-04-01T05:12:00</StartBoundary>
      <Enabled>true</Enabled>
      <ScheduleByDay>
        <DaysInterval>1</DaysInterval>
      </ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <RunLevel>HighestAvailable</RunLevel>
      <UserId>alien_db</UserId>
      <LogonType>Password</LogonType>
    </Principal>
  </Principals>

（以下略）
```
とあった。このことから、「2021-04-01 05:10:54」にタスクが登録さて、「2021-04-01 05:12:00」がトリガーになるように設定されている様子。


***
# DD-3
![image](https://github.com/user-attachments/assets/dd8986ad-62c8-47eb-a2dd-c3581a1b92e8)   

イベントログのセキュリティログにて、ID:4648のログイン成功者から絞ってみる。
![image](https://github.com/user-attachments/assets/5441c4b2-964c-4eb5-b7d2-21333225e86c)
```
	Account Name:		alien
	Account Domain:		IIS APPPOOL
```

これかが仕掛けてきているのでこれがやられた可能性がある。

**dev_agardner**

***
# DD-4
【corp-file.alien.local】と【corp-dc.alien.local】へのログイン試行が見られる
おそらくcoro.webdevへの展開失敗か？

![image](https://github.com/user-attachments/assets/af0bfb1e-aa33-41f1-bb96-9681e40a5672)    
![image](https://github.com/user-attachments/assets/6bcc10e0-5942-4536-a0ca-4a53f5087b6b)  

SMB 経由で接続していると思われるので、【corp-file.alien.local】と【corp-dc.alien.local】それぞれのMicrosoft-Windows-SMBServer%4Operationalを見てみる。
![image](https://github.com/user-attachments/assets/be8268ea-3cca-4630-a90b-b3f9186a9391)  
![image](https://github.com/user-attachments/assets/a5334940-d5a0-45c1-be27-1e8334f0a55d)  
【corp-file.alien.local】側で失敗している模様。

**CORP-FILE**が失敗したホスト

***
# DD-5
![image](https://github.com/user-attachments/assets/3e07c3c0-de97-4072-9cf0-429164c6d752)


***
# DD-6
![image](https://github.com/user-attachments/assets/2ae8fe50-40c3-47a5-957d-fdde9bd567c9)  
問題文から何かしら【corp-dc.alien.local】内で出力したと考えられる。

exe,py,zipなどがないか見てみると４/１0500以降で怪しげなものがいる。
![image](https://github.com/user-attachments/assets/a8323d02-ebab-4717-92c0-ce2874b7c422)  
.\tempを基準に何か出てきたのかと思い検索してみると  
![image](https://github.com/user-attachments/assets/1c3f088d-4510-4889-b20d-c3d8c22a2316)

ntds.ditとntds.jfmが怪しげ。ググってみると
![image](https://github.com/user-attachments/assets/eb6ee8eb-6eea-43fd-ba85-6600a9a63c0d)  
なんか使われそうな予感
これが正規のログ  
![image](https://github.com/user-attachments/assets/ebdbaefa-9764-4941-98cc-2bf6ace41ca9)


これが今回のログ
![image](https://github.com/user-attachments/assets/2dd6dee1-3120-4fea-8559-8538d636a7f2)  


ad.zipが生成されていることからこれの大本を探す。
.\Temp関連で探すと、  
![image](https://github.com/user-attachments/assets/da300eea-15b4-4425-902d-2ac9b90c2aab)

ntds.ditとntds.jfmがある。これをさらに深堀してみる。
![image](https://github.com/user-attachments/assets/be92280c-34dd-4ce0-b328-7fd4c6eaf636)  

Ntds.dit ファイルは、ユーザー オブジェクト、グループ、グループ メンバーシップに関する情報を含む Active Directory データを保存するデータベースで、ドメイン内のすべてのユーザーのパスワード ハッシュが含まれている。
![image](https://github.com/user-attachments/assets/b95e60e2-624e-4bf7-b1bd-f5cc6efb7fa4)  
セキュリティログにこれの関連がないか検索すると案の定「C:\Windows\System32\ntdsutil.exe」が存在していた。

***
# DD-7
![image](https://github.com/user-attachments/assets/0667eec2-bbed-4771-898d-57610470e7d4)  
このことから、corp-webdevからのログを調べれば何かしら、出てきそう。  
![image](https://github.com/user-attachments/assets/dda53a94-0c66-48af-b332-78ff15f8f11b)

```
2021-04-01 06:18:53 - アカウントを使用した corp-webdev から corp-file への SMB アクセス試行re_bmilton
2021-04-01 06:18:53 - corp-file セキュリティ イベント ログにイベント ID 4624 (アカウントが正常にログオンされました) が記録されました
```
re_bmiltonに対する正常ログオンが見て取れる

***
# DD-8
![image](https://github.com/user-attachments/assets/450b2a69-0c71-4284-af20-d97a96ac1b5f)  
攻撃者がずっとTempフォルダで何かした生成していることから2021-04-01 06:18:53以降に何か作れれていないか確認すると、
![image](https://github.com/user-attachments/assets/f425971e-3a6e-4ddc-b384-a7b44762d777)  

filedir.txtが`2021-04-01 06:19:19`に生成されているのが、確認できる。







