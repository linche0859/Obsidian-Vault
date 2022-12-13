[[網站指標]]

# 核心網站指標 (Core Web Vitals)
CWV 目前著重在四個方向：
1. 載入速度 -> 網頁載入後多快能看到畫面。
2. 載入互動性 -> 網頁在執行期間多回應使用者互動。
3. 視覺穩定性 -> 網頁元件出現不預期位移的頻率
4. 視覺流暢性 -> 網頁在視覺上能多快呈現在使用者面前。

# 載入速度 (Load Speed)
- 首次顯示內容 (First Contentful Paint) -> 簡稱 FCP。
- 最大內容繪製 (Largest Contentful Paint) -> 簡稱 LCP。

# 載入互動性 (Load Responsiveness)
- 首次輸入延遲 (First Imput Delay) -> 簡稱 FID。
	指測量網頁載入後使用者第一次與網頁互動，直到瀏覽器有空回應的時間。
	
	> 參考 [[2.1 FCP]]
	
- 互動準備時間 (Time to Interactive) -> 簡稱 TTI。
	測量網頁載入、在長時間的任務結束後，使用者可與瀏覽器互動並給予回應的延遲時間。
	
	也可以解釋為，FCP 和靜窗 (quiet window，主執行緒完成長時間任務後空閒五秒以上的時間點) 之間，使用者可與網頁互動的時間。
	
- 總阻塞時間 (Total Blocking Time) -> 簡稱 TBT。
	測量網頁載入後，主執行緒被長時間 (超過 50ms) 任務阻塞的總時間。
	
	通常是測量 FCP 和 TTI 之間，主執行緒被長時間任務佔據的時間。
	
在開發階段中，只能使用 TBT 與 TTI 作為代理指標來診斷潛在的互動行問題，藉由改善 TBT 與 TTI 進而改善 FID。

# 視覺穩定性 (Stability)
- 累計版位配置位移 (Cumulative Layout Shift) -> 簡稱 CLS。

# 視覺流暢性 (Smoothness)
- 速度指數 (Speed Index) -> 簡稱 SI。
	分數越低表示越流暢，使用者感受時間的流速越快。