# 🛍️ 電子商城購物平臺（E-Commerce Shopping Platform）

本專案為一套 全功能電子商城購物系統，使用 C#、ASP.NET Core Web API 與 MySQL 建置，提供商品瀏覽、購物車、訂單管理、金流支付與後台庫存管理等完整功能。 系統採用 前後端分離 與 多層式架構（Layered Architecture），確保在高併發環境下的穩定性，並具備易於維護、測試與後續擴充的特性。


## 系統介紹

    前後端分離：RESTful API 設計，支援 Web、行動 App 與桌面端介接。

    分層架構：Controller / Service / Repository 職責分明，符合 SOLID 原則。

    完整購物流程：從商品搜尋、加入購物車、結帳到訂單追蹤。

    事務一致性：使用 Entity Framework Core 交易機制確保庫存扣減與訂單建立的原子性（Atomicity）。

    權限控管：區分消費者（會員）、商家（供應商）與系統管理員。
