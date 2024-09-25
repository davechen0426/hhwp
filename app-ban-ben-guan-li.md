---
description: 版本管理
---

# APP版本管理

## 版本管理



{% file src=".gitbook/assets/車屏APP版本管理.xlsx" %}

### <mark style="color:red;">寶儷明版</mark>

#### 需求內容

<details>

<summary>14版 (Unreleased)</summary>

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

<details>

<summary>12版 (1.2.7.3d0eb06(12))</summary>

包含需求：

增加Log優化功能

1.  \#1 若log檔未滿500KB，&#x20;

    a.正在記錄不上傳

    b.檔案名稱為當天日期的log不上傳，即隔天要上傳 c.檔案名稱時間 比 系統時間要晚的不上傳
2. \#2 刪除檔案檢查包含檔案最後修改時間
3. \#3 若檔案名稱時間 比 抓到的系統時間還新，代表系統時間未更新，則新的log檔，將以上一次的log檔加1秒命名
4.  \#4 can bus連接成功後，&#x20;

    a. 列印第一筆can bus log資料後會先關閉

    b. 10分鐘再打開，重複a, b c. 若10秒內沒有收到can bus資料，socket會重開，重開同時會再將can bus log打開, 重複a, b

</details>



***



### <mark style="color:red;">立皓版</mark>

#### 需求內容

<details>

<summary>18版 (Unreleased)</summary>

1. APP設備資料回傳後台 (測試通過)
2. APP版本更新API多傳設備ID及車號 (測試通過)

</details>

<details>

<summary>17版 (1.2.cc948c5(17))</summary>

Log優化機制 時間固定顯示功能，無can bus時間校正功能

</details>

<details>

<summary>16版 (1.2.c3554dc(16))</summary>

包含需求：

1. Log優化機制驗證
2. 發車及重啟APP reset TempSeq

</details>

<details>

<summary>15版 (1.2.15(15))</summary>

包含需求：

1. 除車機會送cmd報站之外，新增用首都API 的時間(改10 sec呼叫一次)作為進站依據，第一個只要顯示是0s就報進站，只會播過一次，那後續若收到車機的 cmd ，就再重複報出站，正常一個站最多報兩次進站(API一次+車機一次)。API一個站最多觸發一次，而車機一個站送多次cmd時一樣會報站多次。
2. 兩個來源的報站需控制不往前報站：當API觸發A站到站後更新畫面，再收到車機A站到站就再報一次，當收到車機的是前一站離站時，就不更新了，因為最近一次收到API的已經是A站進站了，原則應該是不往前報站。所以要把最後報站記錄下來，之後不管是API還是車機送的cmd若早於最新報站紀錄時，就不報站。
3. 出站的時候，不採用 API 的時間作出站的報站，而僅以車機cmd 才報出站。
4. 10s偵測一次 偵測到沒收到can 會等10s重啟記log

</details>

<details>

<summary>14版 (1.2.14(14))</summary>

新增 首都兩隻API錯誤處理優化機制

</details>







***



### <mark style="color:red;">寶錄版</mark>

#### 需求內容

<details>

<summary>8版 (1.2.5832835(8))</summary>

修正因版本merge造成路線顯示bug：固定顯示目的站 (R\_DATA去返程目的地會反過來)

</details>

<details>

<summary>7版 (1.2.0ec0477(7))</summary>

包含需求：

1. For 台中客運，有網路，所以要提供一版正常版連線版
2. 增加離線功能(預設開啟)，開啟離線功能時，左下角不顯示時間，右上角不顯示"準備影片下載"
3. 增加log上傳功能
4. UI的部分要與立皓新版一樣，差別是立皓版中間路線圖上的會顯示預約到站時間，其他的版本沒有
5. 增加Log優化功能&#x20;
   1. \#1 若log檔未滿500KB， a.正在記錄不上傳 b.檔案名稱為當天日期的log不上傳，即隔天要上傳 c.檔案名稱時間 比 系統時間要晚的不上傳
   2. \#2 刪除檔案檢查包含檔案最後修改時間
   3. \#3 若檔案名稱時間 比 抓到的系統時間還新，代表系統時間未更新，則新的log檔，將以上一次的log檔加1秒命名
   4.  \#4 can bus連接成功後，&#x20;

       a. 列印第一筆can bus log資料後會先關閉

       b. 10分鐘再打開，重複a, b c. 若10秒內沒有收到can bus資料，socket會重開，重開同時會再將can bus log打開, 重複a, b

</details>







***

### <mark style="color:red;">中華電信版</mark>

#### 需求內容

<details>

<summary>2版  (1.1.9071b12(2))</summary>

包含需求：

1. log上傳及機制優化 (merge 寶麗明功能)&#x20;
2. 離線功能 (merge 寶錄功能)&#x20;
3. can log減量

</details>

<details>

<summary>1版  (1.1.0(1))</summary>

ack版

</details>

