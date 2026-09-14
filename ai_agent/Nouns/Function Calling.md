# MCP vs Function Calling：AI 工具整合的兩種典範深度分析


大型語言模型（LLM）本身僅能生成文字，無法直接與外部世界互動。要讓 AI 模型具備查資料庫、操作檔案、呼叫 API 等能力，就需要一套機制來連接模型與外部工具。

傳統做法是每個模型搭配每個工具各自寫一套整合，形成所謂的 **M×N 問題**（M 個模型 × N 個工具 = M×N 套整合），維護成本極高。

Function Calling 和 MCP（Model Context Protocol）代表了兩種解決這個問題的典範：前者是供應商內建的功能，後者是開放、標準化的協議。

---

## 什麼是 Function Calling？

### 起源

OpenAI 在 2023 年首次引入 Function Calling 能力，隨後 Anthropic（Claude）、Google（Gemini）也各自推出類似機制。

### 運作方式

1. 開發者在 API 請求中提供函式的 JSON Schema（名稱、參數、描述）
2. LLM 根據使用者提問決定是否調用某個函式
3. 若需要調用，模型回傳一個 JSON 物件，內含函式名稱與參數
4. 開發者程式碼執行實際函式，將結果傳回給模型

```python
# OpenAI Function Calling 範例
response = openai.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "台北天氣如何？"}],
    tools=[{
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "取得指定城市的天氣資訊",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {"type": "string"}
                }
            }
        }
    }]
)
```

### 局限

- **供應商綁定**：每家 LLM 供應商的 Function Calling 格式不盡相同，切換模型需改寫程式碼
- **無工具發現機制**：工具定義需開發者手動撰寫，模型無法動態發現可用工具
- **每次請求獨立**：函式定義每次 API 呼叫都要傳入，缺乏狀態管理
- **語意層不足**：函式定義僅描述參數與回傳值，缺乏更高層次的資料存取模式

---

## 什麼是 MCP（Model Context Protocol）？

### 起源

- **2024 年 11 月**：Anthropic 開源 MCP 規範與 SDK
- **2025 年 12 月**：Anthropic 將 MCP 捐贈給 Linux Foundation 旗下的 Agentic AI Foundation（AAIF），共同創立者包含 Anthropic、Block、OpenAI
- **2026 年現狀**：已成 AI Agent 工具整合的 de facto 標準，月 SDK 下載量超過 9700 萬

### 核心架構：三層模型

```
┌─────────────────────┐
│   MCP Host          │  ← AI 應用程式（Claude Desktop, Cursor, VS Code...）
│  ┌───────────────┐  │
│  │ MCP Client A  │──┼──→ MCP Server A（GitHub）
│  ├───────────────┤  │
│  │ MCP Client B  │──┼──→ MCP Server B（PostgreSQL）
│  ├───────────────┤  │
│  │ MCP Client C  │──┼──→ MCP Server C（Slack）
│  └───────────────┘  │
└─────────────────────┘
```

- **MCP Host**：需要外部能力的 AI 應用程式，如 Claude Desktop、Cursor、OpenCode、VS Code
- **MCP Client**：Host 內部建立的 1:1 連線，負責與單一 Server 通訊
- **MCP Server**：提供特定能力的輕量行程，可本機執行或遠端部署

### 三大原語

| 原語 | 說明 | 範例 |
|------|------|------|
| **Tools** | 模型可調用的函式，執行後回傳結果 | 查資料庫、搜尋網路、建立檔案 |
| **Resources** | 模型可讀取的資料，唯讀存取 | 檔案內容、資料庫 schema、文件 |
| **Prompts** | Host 可使用的可重用提示模板 | 常見任務的模板 |

### 傳輸層

MCP 基於 **JSON-RPC 2.0**，支援多種傳輸方式：

- **stdio**：本機通訊，Server 作為子行程執行
- **HTTP + SSE**：遠端通訊，Server 端事件串流
- **Streamable HTTP**：2026 年新增，簡化遠端部署

---

## 核心差異對比

| 維度 | Function Calling | MCP |
|------|------------------|-----|
| **標準化** | 供應商專屬，每家格式不同 | 開放協議，廠商中立 |
| **架構** | 工具定義內嵌在 API 請求中 | 工具獨立為外部 Server |
| **工具發現** | 手動定義 Schema | 自動發現（ListTools） |
| **跨模型可移植性** | 低，綁定特定供應商 | 高，任何相容 Client 皆可使用 |
| **狀態管理** | 無狀態，每次請求獨立 | 支援有狀態會話 |
| **安全性** | 憑證混在應用程式程式碼中 | 憑證隔離在 Server 端，最小權限原則 |
| **生態系** | 各供應商各自為政 | 數千個現成 MCP Server |
| **複雜度** | 低，快速上手 | 中高，需架設 Server |
| **延遲** | 低（直接內嵌） | 中（多一層網路傳輸） |

---

## 兩者並非互斥，而是層級關係

MCP 與 Function Calling 不是二選一的關係。在實作上：

- **MCP Client 底層仍使用 Function Calling**：讓 LLM 決定要調用哪個工具
- **MCP 在其上增加標準化層**：工具發現（discovery）、傳輸抽象、狀態管理、資源存取
- **可以混合使用**：例如在 OpenAI Function Calling 中包裝一個 `call_mcp_tool` 函式，讓 GPT 透過它調用任何 MCP Server

```
Function Calling = LLM 的內建工具調用機制（底層）
MCP              = 標準化的工具管理與發現協議（上層）
```

---

## 使用場景建議

### 適合 Function Calling

- 快速原型開發
- 已鎖定單一 LLM 供應商
- 對延遲敏感，需要最低開銷
- 工具數量少（1-5 個）

### 適合 MCP

- 需要跨模型相容（同時支援 OpenAI、Claude、Gemini）
- 工具數量多，需要動態發現
- 需要安全隔離（憑證不進 LLM Context）
- 建構多步驟、有狀態的複雜 Agent
- 工具需要被多個應用程式共享

---

## 2026 年生態系現狀

### 原生支援 MCP 的工具

- **IDE / Editor**：Claude Code、Cursor、Windsurf、VS Code、Zed、Continue.dev
- **AI 開發工具**：OpenCode（650 萬月活躍開發者）、Cline
- **MCP Server 數量**：數千個，涵蓋 GitHub、Slack、PostgreSQL、Google Drive、Notion、Figma 等

### 安全性考量

隨著 MCP 普及，安全研究也隨之深入。Adversa AI 發布的 **MCP Security Top 25** 指出主要風險包括：

- **Prompt Injection**：惡意提示注入（嚴重性最高）
- **Command Injection**：命令注入攻擊
- **Tool Poisoning（TPA）**：工具描述中毒，引導模型執行非預期操作

---

## 結論

| | Function Calling | MCP |
|--|-----------------|-----|
| 本質 | LLM 供應商的內建功能 | 開放標準協議 |
| 定位 | 底層機制 | 上層框架 |
| 優勢 | 簡單、快速、低延遲 | 標準化、可移植、可擴展 |
| 劣勢 | 綁供應商、無標準化 | 架構較複雜、多一層開銷 |

兩者將長期共存。Function Calling 是單次模型調用中的工具選擇機制，MCP 則是跨模型、跨工具、跨應用的標準化生態系統。對於認真建構 AI Agent 的團隊來說，**理解並善用二者的層級關係**，才是正確的架構思路。

---

