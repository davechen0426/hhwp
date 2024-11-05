# 24/10/28 Infra配合部分

主管：Rachel

育芬：infra PM

乃為：infra 實際操作配合

Key：OCP Application相關驗證

Lisa ：infra CI/CD



***

### &#x20;架構審查簡報說明



P.26 環境圖

FTPS 個人會員系統

eShop=T-Shopping

ETRAC 流量控制，之後約場次說明

AD: 後台整合

AGTS:高鐵假期

AORS/ETC票務

&#x20;

P.29

大版更：晚上做

作業時間：若小更新的話，可以在UAT 1/2/4白天做; 正式環境 3/5凌晨

Site Crash 後服務回覆順序，服務等級A內優先順序2

&#x20;

P32

歷史資料不提供線上查詢，user進線客服查詢

後台要能夠讓客服到後台查，不一定要有UI

做House keeping 機制

&#x20;

&#x20;

Logging檢索

Elastic(ELK)

ISO定義183天 infra log

定義AP log保留多久？>>>Elastic 決定空間

&#x20;

P33

保留專案測試時間 for WAF/CDN

起停服務watch dog會先停

各環境都會有

&#x20;

P34

排程若需要機房監控，有固定格式，

&#x20;

應用系統層監控+告警

E.g. 確認DB連線

其他專案

&#x20;

要定義log告警機制：目前高鐵沒有特定方式

AP錯誤，要丟到哪？

通知機房或OP人員？
