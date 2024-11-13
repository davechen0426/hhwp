# 24/10/28+30 Infra配合部分

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





***

### Infra配合的部分



<table data-header-hidden><thead><tr><th width="80">#</th><th>提問</th><th>回覆</th></tr></thead><tbody><tr><td>8</td><td><p>Native App 與 WebView 之間的溝通</p><p>- 驗證方式</p><p>- 取得使用者資訊</p><p>- 取得定位、beacon資訊</p><p>- 功能導轉 取得Deeplink</p></td><td>與新芽談，開規格給廠商</td></tr><tr><td>9</td><td>QA/PROD環境，問題查看、log調閱方式？</td><td><p>3個月?Read only</p><p>Prod DB by table申請 非全部</p><p>Log存取權限再整理</p><p> </p></td></tr><tr><td>10</td><td>PROD一般/緊急 狀況處理方式？</td><td><p>服務上線後，發生問題</p><p>DBA按照手冊，排除初步問題</p><p>1.由Joey跟infra優先恢復服務</p><p>2.再收集log，查看問題</p><p> </p><p>主機掛掉: 會聯繫主機廠商</p><p>OS層：update問題，快照還原</p><p>程式：功能問題，ci/cd 判斷後rollback(搭配health check)</p><p>若waf，可請示主管後，快速開通</p><p>若AP，走緊急上線流程，須經兩位部主管同意才可</p></td></tr><tr><td>11</td><td>當流量大量湧入，造成系統負載過大，目前的因應措施為何？</td><td>網路組LC監控流量，若異常會發佈訊息到群組，會由WAF回覆頁面</td></tr><tr><td>12</td><td>目前使用的雲基礎服務有哪些？能否提供詳細的服務列表？及相關說明文件 (DB運維?/Redis/AD/FTP/API/APIM/Queue/MinIO....)</td><td><p>1.備份follow policy，申請後自動備份VM、或整個DB，由AP決定</p><p>2.改DB schema寫SQL語法，併入CI/CD作業</p><p>3.上版(包含rollback語法)，DBA不會直接進DB操作</p><p>4. Queue自建到container</p><p>5.用OCP redis cluster, 不自己建</p><p>miniIO再評估>架構審查先擺入（關鍵要有原廠支援，持續性服務）</p><p>6.此階段APIM來不及導入，呼叫外部API不用指定proxy<br> </p></td></tr><tr><td>13</td><td>資料串接的方式除了透過 APIM 及 FTP, 還有其他的嗎？(Queue/服務發現/ETL)</td><td>SFTP共用(內部MIS zone及B2B zone)只能傳非機敏資料，加密後能不能用？</td></tr><tr><td>14</td><td>第三方 API 的測試環境是否可用？如何進行測試？</td><td>模擬API</td></tr><tr><td>15</td><td>程式碼交付的方式是什麼？是否有特定的 Git 或開發規範？</td><td><p>-打包（整包或是增量異動的部分，異動的部分要提供說明，截圖即可）後給Joey，checkin到bitbucket後透過Jenkins先部署到Dev</p><p>-OCP導入建置k8s到時子專案討論細節(起停服務scirpt須先定義, manifest要提供, 程式內要寫pre start hook?  For rolling update 不停服務的更新)</p><p> </p><p>-UAT是infra上，user測完再上PROD</p><p>-UAT跟PROD版本會是由infra控制一致</p><p> </p></td></tr><tr><td>16</td><td>CI/CD 作業的具體規範是什麼？pipeline 使用了哪些工具？版本是什麼？</td><td><p>-PROD直接用UAT的image</p><p>-UAT ci/cd會分別產生到uat及prod的quae</p><p>-要提供API(e.g DB health check)給vital check(UTP/UiPath)工具呼叫</p><p>-從prod的quae直接抓下來用</p><p>-看是否找先廠商開會說明?</p></td></tr><tr><td>17</td><td>壓力測試的要求有哪些？</td><td><p>-進開發後再討論測試相關</p><p>-之前做法：在UAT擴大至PROD同規格來做</p><p>-考慮直接壓DB，檢測工具應包含DB檢測結果，進而優化</p><p>-HPA架構的情境:擴展上去後的能力</p></td></tr><tr><td>18</td><td>資安檢測的標準是什麼？</td><td><p>AP掃描：Checkmarx/Mend 檢查套件/Webinspect</p><p>主機OS掃描：Nexus</p><p>滲透測試，由工具來做(一年兩次)，不用找廠商來做</p></td></tr><tr><td>19</td><td>網路環境的設置需求有哪些？是否有特定的配置要求？</td><td> </td></tr><tr><td>20</td><td>資料或檔案的備援及存檔規則是什麼？如何實施？</td><td> </td></tr><tr><td>21</td><td>上線後的遠程偵錯方式是什麼？是否有特定的工具或流程？</td><td> </td></tr><tr><td>22</td><td>能提供哪些遠程監控的選項？報警設置?</td><td>能做在PRTG(監控軟體)就做在裡面</td></tr><tr><td>23</td><td>在資料安全方面，您最關心的風險是什麼？(敏感資料加密)</td><td>之後找相關同事來說明</td></tr><tr><td>24</td><td>在運維方面，有哪些具體需求或挑戰？</td><td> </td></tr><tr><td>25</td><td>對於未來的平台架構有沒有期望或建議？</td><td><p>共用檔案可以再討論<br>多VM增加NFS節點，脆弱點，提供建議</p><p>不影響服務</p><p>需要規劃HPA架構，worker node擴展，資源要估寬鬆一點。<br> </p></td></tr></tbody></table>

