# Pipedrive-整合操作 MCP 伺服器

這是一個 MCP (Mission Control Processor) 伺服器，旨在簡化與 Pipedrive CRM 的互動操作。

透過提供簡單易用的工具，您可以根據使用者需求，自動化地在 Pipedrive 中新增交易 (Deal)、聯絡人 (Person) 和組織 (Organization)。

## 主要功能

本伺服器的核心是一個名為 `add_pipedrive_deal` 的工具，它具備以下智慧功能：

- **智慧新增交易**：提供交易標題、金額、聯絡人與組織資訊，即可在 Pipedrive 建立一筆新交易。
- **自動化聯絡人與組織管理**：
  - 在建立交易時，工具會自動檢查聯絡人或組織是否已存在，**避免重複建立資料**。
  - **聯絡人**：優先使用 `email` 進行精確搜尋，若無 email 則使用姓名。
  - **組織**：優先使用 `統一編號 (tax_id)` 進行精確搜尋，若無統編則使用組織名稱。
  - 如果找不到對應資料，工具會**自動建立新的聯絡人或組織**，並將其與該筆交易關聯。

## 快速開始

### 1. 設定 Pipedrive API 金鑰

打開 `server.py` 檔案，找到 `PIPEDRIVE_TOKEN` 這個變數，並將其值替換為您自己的 Pipedrive API 金鑰。

```python
PIPEDRIVE_TOKEN = "您的Pipedrive金鑰"
```

### 2. 安裝依賴套件

本專案使用 `uv` 進行環境管理，您也可以使用 `pip`。請安裝必要的套件：

```bash
pip install "fastmcp[server]" requests
```

### 3. 啟動伺服器

執行以下指令以啟動伺服器：

```bash
python server.py
```

伺服器預設將會啟動在 `http://0.0.0.0:8080`。

## 工具使用說明 (`add_pipedrive_deal`)

您可以透過向 MCP 伺服器發送請求來使用此工具。

### 參數說明

| 參數 | 型別 | 說明 | 是否必填 |
| :--- | :--- | :--- | :--- |
| `title` | `string` | 交易的標題。 | **是** |
| `value` | `float` | 交易的金額。 | **是** |
| `person_name` | `string` | 聯絡人的姓名。 | 否 |
| `person_email` | `string` | 聯絡人的電子郵件。**強烈建議提供**，以利精確搜尋、避免重複建立聯絡人。 | 否 |
| `person_phone` | `string` | 聯絡人的電話。 | 否 |
| `org_name` | `string` | 組織的名稱。 | 否 |
| `org_tax_id` | `string` | 組織的統一編號。**強烈建議提供**，以利精確搜尋、避免重複建立組織。 | 否 |
| `org_address` | `string` | 組織的地址。 | 否 |
