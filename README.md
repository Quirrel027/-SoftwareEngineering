# 🛍️ 電子商城購物平臺（E-Commerce Shopping Platform）

本專案為一套 全功能電子商城購物系統，使用 C#、ASP.NET Core Web API 與 MySQL 建置，提供商品瀏覽、購物車、訂單管理、金流支付與後台庫存管理等完整功能。 系統採用 前後端分離 與 多層式架構（Layered Architecture），確保在高併發環境下的穩定性，並具備易於維護、測試與後續擴充的特性。


## 系統介紹

    前後端分離：RESTful API 設計，支援 Web、行動 App 與桌面端介接。

    分層架構：Controller / Service / Repository 職責分明，符合 SOLID 原則。

    完整購物流程：從商品搜尋、加入購物車、結帳到訂單追蹤。

    事務一致性：使用 Entity Framework Core 交易機制確保庫存扣減與訂單建立的原子性（Atomicity）。

    權限控管：區分消費者（會員）、商家（供應商）與系統管理員。
    
## 主要功能
👤 會員中心 (Member)

    註冊 / 登入 (JWT Token 驗證)

    個人資料修改、地址簿管理

    歷史訂單查詢與狀態追蹤

📦 商品與庫存管理 (Product & Inventory)

    商品上架 / 下架 / 編輯

    多層級分類管理

    關鍵字搜尋與篩選（價格範圍、熱銷度）

    庫存預警與自動扣減機制

🛒 購物車與交易 (Cart & Transaction)

    加入購物車 / 修改數量 / 移除商品

    結帳流程：

        確認訂單資訊（配送地址、發票）

        選擇付款方式

        鎖定庫存（防止超賣）

        產生訂單

💳 金流支付 (Payment)

    支援第三方金流介接（模擬 API 或 綠界/藍新/Stripe）

    信用卡、ATM 轉帳、貨到付款狀態更新

    付款失敗重試機制

⚙️ 系統與報表 (Admin)

    銷售儀表板：即時查看今日訂單數、營收

    報表導出：月營收報表、熱銷商品排行

    操作日誌：記錄管理員的敏感操作（如強制修改訂單狀態）

## 系統架構圖
<img width="1646" height="827" alt="圖片" src="https://github.com/user-attachments/assets/55326a5b-4550-42b0-bf54-dfe79d030cc2" />


