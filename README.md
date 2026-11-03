# SellerMate — 智慧電商經營分析平台

以 **AI 開發規則（AGENTS.md）** 指導建立的靜態多頁網站，適合 GitHub Pages。提供可實際操作的商品利潤、最低售價、廣告指標及 CSV 商品比較功能。

## 網站頁面
- `index.html`：示範儀表板、ROAS/CPA、商品表現摘要
- `calculator.html`：商品利潤、保本/目標利潤售價計算器
- `products.html`：搜尋、排序、CSV 匯入、多商品分析
- `about.html`：專案背景、架構、限制及未來 AI Agent 整合

## 使用方式
直接開啟 `index.html`，或以 `python -m http.server 8000` 在專案目錄啟動本機網站，前往 http://localhost:8000 。無需安裝套件。

## 部署 GitHub Pages
1. 建立公開 Repository，例如 `sellermate-web`，將本專案所有檔案上傳至根目錄。
2. Repository → Settings → Pages → Build and deployment → Deploy from a branch。
3. 選擇 `main` 與 `/(root)`，儲存後等待 GitHub Pages 部署。
4. 開啟 `https://<你的帳號>.github.io/sellermate-web/`，確認四頁與 CSV 匯入均正常。

## CSV 格式
必要欄位：`name,price,cost,orders,ad_spend`。選填：`other_cost,shipping_cost,platform_fee_pct`（預設 0、0、10）。每筆代表一種商品；`ad_spend` 是該商品所有訂單的廣告總費用，`other_cost`、`shipping_cost` 是**每單**費用。`orders` 為非負整數。

計算：單件淨利 = 售價 - 成本 - 其他費用 - 運費 - 售價×平台費率 - 廣告總費用/訂單數。訂單數為 0 時，廣告每單分攤為無法計算，總淨利仍可計為 -廣告總費用。總營收 = Σ(售價×訂單數)；總淨利 = Σ(不含廣告的單件淨利×訂單數 - 廣告總費用)。ROAS = 總營收/總廣告費；CPA = 總廣告費/總訂單數。無分母時顯示 N/A。

## 自動測試
安裝 Node.js 18+ 後，在專案根目錄執行：`node tests/test-core.mjs`。

## 限制與誠實揭露
- 預設資料為**模擬示範**，不是任何真實商家的銷售紀錄。
- CSV 在瀏覽器本機處理，不會傳送到伺服器；重新整理頁面會回復示範資料。
- GitHub Pages 是靜態網站，目前沒有串接 Groq 或真正 AI Agent；期末可透過安全後端擴充。
- 未計入的稅務、退款、倉儲及其他支出會影響實際獲利。

## AI 協作與延伸
`AGENTS.md` 指定頁面架構、計算定義、UI 風格、CSV 格式、測試與修改程序。未來請 AI 新增報表頁或新指標時，先要求它閱讀 `AGENTS.md`，遵守共用元件與測試規範，並同步更新 README。
