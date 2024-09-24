# Log優化

### log檔名抓取IP調整

這個寫log需要取得IP的動作，開機後把第一次取得IP時直接記錄下來(因為IP不會變)，類似跟紀錄路線資料一樣的方式，然後紀錄log時就改查這個紀錄的IP，避免有時抓得到IP有時抓不到的問題，而造成log檔案虛增，還有上傳不完整的問題



### log檔案壓縮上傳

1. 檔案大小預設上限改為5M(先試試，之後可改成吃後台config)，滿了就建新檔
2. 改每60分鐘上傳一次，
   1. 壓縮：檢查上傳候選csv，若符合，則每個檔案各自壓縮檔案為zip，原csv押上(done)
   2. 上傳：zip檔案上傳，完成後押上(done)

### log減量

1. 呼叫首都API \
   1.有錯誤第一次印出來，同樣的錯誤累積100次後再印\
   2.中間恢復後，印出已恢復的訊息，並印出累積錯誤次數\
   3.呼叫失敗不將暫存站點變更為-1
2. 上傳log清單\
   不用全部檔案印出，只要印出統計數字，共N個檔案，已上傳N個檔案，剩餘N個檔案未上傳，再列出未上傳的檔案名稱
3. 不印預計到站時間(現在每10秒印一筆)\
   🔵callGetCarTTE url: [http://busgroup.capitaltour.com.tw/CarTTE/EAL-1112/:uid](http://busgroup.capitaltour.com.tw/CarTTE/EAL-1112/:uid)
4. 輔助報站showNextStopFromAPI: {"CarNum":"EAL-1116","updateTime":"2024/06/27 18:25:05","pathAttrId":11093,"ttiaRoute":"0612000","stops":\[{"stopSeq":68,"stopId":148752,"stopTTE":0},……]}\
   只要印第一站就好
