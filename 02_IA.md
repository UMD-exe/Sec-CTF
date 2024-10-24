# 問題IA-0
<img src=https://github.com/user-attachments/assets/449e9121-9abf-4017-867b-2b83b311e3bc wdth="200">

***
# 問題IA-1
<img src=https://github.com/user-attachments/assets/88e6057b-61d3-4c1c-be27-f43bd2b8153f wdth="200">  

インストールを試みているようなので何かしたPOSTしているはずなので  
<img src=https://github.com/user-attachments/assets/f82d3916-41bc-4acd-8d14-9467c678a63c wdth="200">  
これしかいなそう。

13.54.35.87 

***
# 問題IA-2
<img src=https://github.com/user-attachments/assets/87c01df3-cb0e-4b28-9edd-641b4ccd8adf wdth="200"> 

13.54.35.87が何かしているログであることは明白なので、IPアドレスで一括検索すると

<img src=https://github.com/user-attachments/assets/86fc39d7-4384-401c-8ef9-41c519be9a48 wdth="200">  


「Telerik.Web.UI.WebResource.axd type=rau」が比較的新しく怪しそう...  


<img src=https://github.com/user-attachments/assets/c1f74072-6720-4a8c-a924-e88af4f904e2 wdth="200">   

CVE-2019-18935で正解だった。

***
# 問題IA-3
<img src=https://github.com/user-attachments/assets/480aa664-ff24-41e4-b842-ef86d7308eee wdth="200">  


下記のログが初めて攻撃が成功した時間↓
<img src=https://github.com/user-attachments/assets/89ea1892-3631-4808-9384-beb10c487d44 wdth="200">   

そこで近しい時間に作られたファイルをMFTから抽出して表示↓
<img src=https://github.com/user-attachments/assets/a312c59a-8491-4026-a32a-04062c68b981 wdth="200">   

1617245455.5314393.dll

***
# 問題IA-4
<img src=https://github.com/user-attachments/assets/f2d62623-b88f-47d2-8215-1bc734eb87fa wdth="200">

「1617245455.5314393.dll」がダウンロードの確認に使われていることから、これより後に本体をダウンロードしている可能性が高い

<img src=https://github.com/user-attachments/assets/6d8fc4a1-d88f-434e-b7db-c5f2a4d66068 wdth="200">  

App_Web_aa0aecbt.dllというあからさまに名前が怪しいものが直後にダウンロードされている
。

2021-04-01 02:55:29


