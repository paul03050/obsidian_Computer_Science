
## 為什麼 AI Agent 需要 MCP？

在 MCP 出現之前，如果要讓 AI Agent 使用特定的外部工具（例如：讀取 GitHub 程式碼、查詢資料庫、串接 Google 雲端），工程師必須為每個工具單獨編寫客製化的 API 整合代碼。這導致了幾個痛點： [2, 4]

- 開發破碎化：每換一個工具或模型，程式碼就要重寫。
- 資訊斷層：AI 無法輕鬆取得最新、最即時的本地或企業私有數據（上下文資訊）。 [1, 2, 5]

MCP 解決了這個問題：  
過去的 API 是寫給「工程師與固定系統」看的，呼叫方式必須寫死；而 MCP 是專門寫給「AI 模型」看的通用中介層。AI 只要連接到支援 MCP 的伺服器，就能自我探索並理解有哪些工具可用、該怎麼使用。 [3, 7]

---

## MCP 的三層架構

MCP 協定主要由三個核心角色組成，形成無縫的資訊流： [8, 9]

1. MCP Host（主機/客戶端環境）
    
    - 定義：執行 AI Agent 或整合開發環境（IDE）的應用程式。
    - 範例：Claude Desktop、Cursor、Trae、VS Code 等開發工具。 [6]
    
2. MCP Client（客戶端協定）
    
    - 定義：負責在 Host 內部與 AI 模型溝通，將模型的「意圖和需求」轉換為標準的 MCP 請求。 [6, 9]
    
3. MCP Server（伺服器端）
    
    - 定義：獨立運行的輕量級程式，專門向 AI 公開特定系統的數據或功能（如：讀取特定檔案、執行終端機指令、串接 Slack 等）。 [3, 8]
    

---

## MCP 的三大核心功能

透過 MCP 協定，AI Agent 能夠安全地使用以下三種基礎能力：

- Resources（資源）：讓 AI 以唯讀方式安全讀取外部數據（如：本地檔案、資料庫內容、API 回傳值）。
- Prompts（提示詞範本）：提供預設的快捷提示詞或任務模板，方便使用者或系統快速引導 AI。
- Tools（工具行動）：允許 AI 執行實際的動態操作（如：自動寫入程式碼、發送電子郵件、建立系統表單等）。 [3, 8, 10]

---

## 總結 MCP 的核心優勢

|優勢維度|傳統 API 模式|MCP 協定模式|
|---|---|---|
|開發成本|需針對每個工具寫死 API 串接程式碼|一次接入，即插即用所有支援 MCP 的工具|
|運作本質|靜態手冊，功能與規則無法動態改變|即時地圖，AI 能主動探索並理解新工具|
|安全管控|權限通常直接綁定在程式或金鑰中|具備嚴密的權限控管與本地安全機制|


# 常用
1. Context7
2. Playwright
3. Filesystem MCP (檔案系統)
4. Sequential Thinking MCP (連續思考)
5. PostgreSQL / Supabase MCP
6. GitHub / GitLab MCP
7. Docker MCP
8. Brave Search / Tavily MCP
9. Fetch MCP
10. Linear / Jira MCP 
11. Slack MCP
12. Notion / Obsidian MCP (知識庫)
13. Figma MCP
14. 

