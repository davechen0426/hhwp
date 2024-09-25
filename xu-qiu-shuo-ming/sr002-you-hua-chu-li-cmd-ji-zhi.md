---
description: 解決cmd累積問題
---

# 優化處理cmd機制



* 新增cmd queue機制
* 只要收到完整cmd (包含$開頭\*結尾則為一個完整的整令)就提取出來到cmd queue執行，避免cmd持續累積超過上限就不執行了
* 當一次收到多個display指令時，drop掉前面報站指令，只保留最後一個display指令



> e.g. 收到以下cmd時$W1,Display,6120,1,4,0\*$W1,Display,6120,1,5,1\*$W1,Display,6120,1,5,0\*$W1,Display,6120,1,6,1\*$W1,Display,6120,1,6,0\*
>
> 因為都是display指令，只保留\*$W1,Display,6120,1,6,0\*到queue執行



