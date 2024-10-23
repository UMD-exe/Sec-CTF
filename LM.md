# 問題LM-0
<img src=https://github.com/user-attachments/assets/2d92c5cb-2be3-46f1-9865-a9e418087fd3 width="300">

***
# 問題LM-1
<img src=https://github.com/user-attachments/assets/93276794-9ea4-49a8-801e-af20413a93fc width="300">  

- 02:50:42 ～ 02:54:56にかけて、/Telerik.Web.UI.WebResource.axdエンドポイントに対するPOSTリクエストが繰り返し行われてる。
- このエンドポイントは、Telerik Web UIのリソースハンドラであり、過去にリモートコード実行（RCE）やファイルアップロードの脆弱性が報告されています。攻撃者はこの脆弱性を悪用して、ファイルをアップロードしたり、サーバー上でコマンドを実行する可能性
- リクエストに対して、HTTP 200 OKが返っている箇所もあり、何らかの操作に成功している

- 02:55:29 ～ 03:01:19の間に、/submit.aspxへの複数のGETおよびPOSTリクエスト
- submit.aspxは通常、データを送信するためのエンドポイントです。攻撃者がサーバーに対して情報をアップロードするか、ダウンロードしたファイルの確認やデータ送信に利用している可能性があります。すべてのリクエストに対してHTTP 200が返されてる。
- 特に、03:00:02のリクエストで、比較的大きなデータ量（14,192バイト）が送信されており、これがファイルのダウンロードや大量のデータ送信を示唆

- 03:02:06に/dfsr-reports/Health-alien_webdev-31Mar2021-0421.xmlというファイルがGETリクエストでダウンロードされています。これは、攻撃者が興味を持つファイルである可能性が高く、システムやサーバーのヘルスレポート、構成情報、あるいは内部データを含んでいる

- Python-urllib3が使われたリクエスト（02:54:56）が確認されています。これは、攻撃者がスクリプトを使って自動化されたリクエストを送信している

- submit.aspxのリクエストに関連するプロセスは、攻撃者がファイルをダウンロードまたはサーバー上で取得した情報を送信するために使われている可能性

## 結論
　攻撃者は、Telerikの脆弱性を利用してサーバーにアクセスし、ファイルやデータをダウンロードしています。このログでは、特に**dfsr-reports/Health-alien_webdev-31Mar2021-0421.xml**が攻撃者にダウンロードされた興味深いファイル

dfsr-reportsとはなにかを調べると、「Write-DfsrHealthReport (DFSR) | Microsoft Learn…」と出てくる。
分散 ファイルシステム レプリケーションのことらしい。

おそらく、**DFSR**が正解である。

***
# 問題LM-2
<img src=https://github.com/user-attachments/assets/7d7f5215-8841-43f1-b0bc-9111f04322fc width="300">  

問題から、おそらくDFSRのイベントログから通信した先の相手がわかるはずなので、  
<img src=https://github.com/user-attachments/assets/b5e5503a-d93b-4f8d-958c-e8c3a09fb0eb width="800">  

「corp-webdev.alien.local」が通信した相手。つまり、corp-webdev.alien.localのセキュリティログを見れば必ず認証している形跡がある。    

<img src=https://github.com/user-attachments/assets/7ccc6bec-158f-47d4-a812-84e5b94c358a width="800">  


**CORP-WEBDEV**

***
# 問題LM-3
<img src=https://github.com/user-attachments/assets/4e37c2cf-8167-442b-bf79-6977ac93269f width="300">    

ここにDFRSのイベントログがある。  
<img src=https://github.com/user-attachments/assets/23696150-9105-4ba2-9ff7-4922be1818cc width="600">    


時間帯的にこれが怪しい。  
<img src=https://github.com/user-attachments/assets/4af444b2-baa5-4243-b310-3c9910b160b5 width="600">  


C:\inetpub\wwwroot\alien
