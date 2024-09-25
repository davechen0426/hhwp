---
description: 版本管理
---

# APP版本管理

## 版本管理

### <mark style="color:red;">寶儷明版</mark>

#### 需求內容

<details>

<summary>14版</summary>

包含需求：

1. SR001 時間校正機制
2. SR002 優化處理cmd機制
3. SR003 Log 優化
4. APP設備資料回傳後台
5. APP版本更新API多傳設備ID及車號

</details>

<details>

<summary>13版 (1.2.7.bf2cb76(13))</summary>



業者偶而會反應時間顯示不正確的情況，原因是沒有網路的情況下無法完成NTP校時，討論後決定以can時間來當校時備援&#x20;

1. 優先序：NTP校準時間 > canbus廣播時間 > 本地系統時間&#x20;
2. 網路校時當主要的機制&#x20;
3. can當校時備援&#x20;
4. 頻率：開啟APP後持續一直listen，直到抓到時間並完成補充校時程序即停止&#x20;
5. CAN校時方式：判斷can抓到的時間，&#x20;
   1. 若與我們目前的系統時間為同一日，就不更新(代表網路校時過了，或時間已經是正確的)，&#x20;
   2. 若否，則更新系統時間&#x20;
6. 預設不顯示時間，透過can校時程序完成後才顯示時間 7.離線模式下也依同邏輯顯示時間

</details>



### <mark style="color:red;">立皓版</mark>

<details>

<summary>19版</summary>

1. APP設備資料回傳後台 (測試通過)
2. APP版本更新API多傳設備ID及車號 (測試通過)

</details>



### <mark style="color:red;">寶錄版</mark>

