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


**dev_agardner**

