# 🛍️ 電子商城購物平臺（E-Commerce Shopping Platform）

本專案為一套 全功能電子商城購物系統，使用 C#、ASP.NET Core Web API 與 MySQL 建置，提供商品瀏覽、購物車、訂單管理、金流支付與後台庫存管理等完整功能。 系統採用 前後端分離 與 多層式架構（Layered Architecture），確保在高併發環境下的穩定性，並具備易於維護、測試與後續擴充的特性。

一、需求說明書（Software Requirement Specification, SRS）（Software Requirement Specification, SRS）
### 1.專案目標
  建置一套高可用、可擴充、安全的電子商城購物平台，提供使用者完整的線上購物體驗，並支援後台營運與管理需求。
### 2.使用者角色
   
  - **訪客（Guest）**：瀏覽商品、搜尋商品

  - **會員（Member）**：下單、付款、訂單追蹤

  - **商家（Vendor）**：商品與庫存管理
  
  - **系統管理員（Admin）**：系統設定、報表、權限控管

### 3.功能性需求
- **3.1 會員模組**:

  使用者註冊、登入、登出（JWT）
  
  個人資料與收件地址管理
  
  訂單查詢與狀態追蹤
  
- **3.2 商品模組**:
   
  商品 CRUD（新增 / 修改 / 下架）
  
  多層分類管理
  
  關鍵字搜尋與條件篩選
  
- **3.3 購物車與訂單**
  
  加入 / 移除購物車
  
  修改商品數量
  
  建立訂單並鎖定庫存
  
- **3.4 金流支付**
  
  第三方金流串接
  
  支付結果回調（Webhook）
  
  訂單狀態更新
- **3.5 後台管理**
  
  使用者與權限管理
  
  商品與訂單管理
  
  銷售報表與操作日誌
  
### 4. 非功能性需求
  效能：支援高併發下單
  
  安全性：HTTPS、JWT、Role-based Authorization
  
  可維護性：分層架構、單元測試
  
  可擴充性：模組化設計

### 二、概要設計說明書（High-Level Design, HLD）
- **1. 系統架構**

    前後端分離
    
    RESTful API
    
    Layered Architecture

- **2. 系統分層**

    Presentation Layer：Controller
    
    Application Layer：Service
    
    Domain Layer：Entity / Business Logic
    
    Infrastructure Layer：Repository / EF Core

- **3. 技術架構**

    Backend：ASP.NET Core Web API
    
    Frontend：React / Vue / Blazor WASM
    
    Database：MySQL
    
    Cache：Redis

- **4. 資料流程說明**

    使用者請求 → Controller → Service → Repository → Database
    
## 系統架構圖
<img width="1646" height="827" alt="圖片" src="https://github.com/user-attachments/assets/c70c1766-65f3-4585-b569-5e723d678b79" />



🔍 關聯說明
- **User ⇄ Cart (1:1)**：每個會員擁有一台專屬的購物車。

- **Cart ⇄ Product (M:N)**：透過 CartItem 關聯表，記錄購物車內有哪些商品及其數量。
 
- **Product ⇄ Inventory (1:1)**：將商品資訊（價格、描述）與庫存數量（變動頻繁）分開，避免鎖表競爭，也方便管理預警值。

- **Order ⇄ OrderItem (1:N)**：一張訂單包含多項商品明細。OrderItem 通常會記錄「下單時的價格 (UnitPrice)」，防止未來商品漲價影響歷史訂單金額。

- **Order ⇄ Payment (1:N)**：訂單與支付紀錄為一對多，這設計是為了容許「付款失敗後重試」或「分批付款」的情況，每一筆嘗試都會產生一條支付紀錄。

### 三、詳細設計說明書（Detailed Design, DSD）
## 資料庫系統

<img width="2112" height="960" alt="圖片" src="https://github.com/user-attachments/assets/729e9fd8-886e-45f1-b042-a367b948b0fb" />

- **1. 主要資料表設計（簡述）**
	User
	
		UserId (PK)
		
		Email
		
		PasswordHash
		
		Role
	
	Product
	
		ProductId (PK)
		
		Name
		
		Price
		
		CategoryId
	
	Inventory
	
		ProductId (PK, FK)
		
		StockQty
	
	Order
	
		OrderId (PK)
		
		UserId (FK)
		
		TotalAmount
		
		Status
	
	OrderItem
	
		OrderItemId (PK)
		
		OrderId (FK)
		
		ProductId (FK)
		
		UnitPrice
		
		Quantity

- **2. 核心流程設計（下單）**

	驗證使用者身份
	
	檢查購物車內容
	
	開啟 Transaction
	
	扣減庫存
	
	建立訂單與訂單明細
	
	提交交易

- **3. 交易與一致性**

	使用 EF Core Transaction
	
	防止髒讀 / 重複下單

## 安全性與效能優化 (Security & Performance)
**1. 安全性 (Security)**
- HTTPS： 全站加密，保護傳輸中的信用卡資訊與個資。
- SQL Injection 防護： 使用 ORM 或參數化查詢。
- CSRF & XSS 防護： 確保前端輸入的資料經過過濾。
- 敏感資料不落地： 絕對不要在自己的資料庫明文儲存使用者的信用卡卡號 (PCI DSS 標準)。

**2.效能優化 (Performance)**
- CDN (內容傳遞網路)： 將圖片、CSS、JS 檔案快取在全球節點，加速載入速度。
- 全文檢索： 使用 Elasticsearch 替代傳統 SQL 的 LIKE 搜尋，提升商品搜尋的精準度與速度。
- 非同步處理： 發送訂單確認信、生成報表等耗時操作，應透過 Message Queue (如 RabbitMQ, Kafka) 非同步處理，避免阻塞主執行緒。
- 
### 四、測試計畫（Test Plan）
- **1. 測試目標**:確保系統功能正確、效能穩定與安全性符合需求。

- **2. 測試類型**

	單元測試（Unit Test）

	整合測試（Integration Test）

	API 測試（Postman / Swagger）

	壓力測試（JMeter）

- **3. 測試範圍**

	會員註冊 / 登入

	商品搜尋與下單

	金流回調處理

	權限與安全性

- **4. 測試環境**

	OS：Windows / Linux

	DB：MySQL Test Instance

  測試工具：xUnit、Postman

- **5. 驗收標準**

	核心功能通過率 100%

	無重大資安漏洞

	高併發下系統可正常運作

## 結語（Conclusion）

本電子商城購物平臺成功整合 C#、ASP.NET Core Web API 與 MySQL，採用多層式架構與前後端分離設計，兼顧系統穩定性、可維護性與擴充彈性。系統完整實作使用者、商品、購物車、訂單與金流等核心商業流程，具備處理複雜業務邏輯的能力。
在開發過程中遵循 SOLID 原則與 RESTful API 規範，並透過交易機制確保訂單與庫存資料的一致性。同時兼顧安全與效能需求，落實基本資安防護，並預留 CDN、非同步處理等擴充空間。
也展現出良好的架構設計，具備未來持續擴展與實務應用的可行性。


