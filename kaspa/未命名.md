這句話**有一部分正確，但如果拿來批評 UTXO，就有點過度概括**。尤其你現在正在研究 Kaspa 的 Covenant / SilverScript / Argent，這句話其實很值得用邏輯拆開。

### 先把它拆成 3 個主張

原句：

> UTXO 模型因為「缺乏全局狀態、狀態不可變、單次消耗」，所以在複雜、頻繁互動、需要全域共享狀態的智能合約上非常不便。

可以拆成：

**前提 P1：** UTXO 沒有像 Ethereum EVM 那樣的「單一全局可直接修改的共享狀態」。

**前提 P2：** UTXO 一旦被花費，就不能再次被同一個交易直接當作未花費輸出使用。

**前提 P3：** 複雜 DeFi 應用經常需要多個參與者反覆讀取、更新共同狀態。

**結論 C：** 因此 UTXO 模型對這類應用比較不方便。

這個推論**基本上是合理的**。

但要注意：

> **「比較不方便」≠「做不到」。**

這是這段話最重要的邏輯界線。

---

## 1. 「缺乏全局狀態」到底是什麼？

可以把兩種模型想成兩種會計制度。

### Ethereum 類 Account Model

想像公司有一個中央總帳：

```text
Global State
│
├── Alice：100
├── Bob：50
├── Token A：供應量...
├── Lending Pool：...
└── Contract：目前狀態...
```

智能合約可以：

> 讀取目前狀態 → 修改狀態 → 寫回新的狀態

所以很像：

> 「大家共同使用同一本活的總帳。」

---

### UTXO

UTXO 比較像：

> 「桌上有很多張獨立的支票／會計憑證。」

例如：

```text
UTXO #1 → Alice 100 KAS
UTXO #2 → Bob 50 KAS
UTXO #3 → Alice 30 KAS
UTXO #4 → Contract State A
```

Alice 要花 #1：

```text
消耗 UTXO #1
      ↓
產生新的 Outputs
      ↓
UTXO #5
UTXO #6
```

所以 UTXO 的狀態不是：

> 「修改原本那一筆」

而比較像：

> **消耗舊狀態 → 創造新狀態。**

這就是所謂的 **state transition（狀態轉移）**。

---

# 2. 「狀態不可變」容易讓人誤會

這裡其實是一個非常重要的概念。

說：

> UTXO 狀態不可變

並不是說：

> 整個系統的狀態永遠不能改。

而是：

> **一個已存在的 UTXO 本身不能被原地修改。**

例如：

```text
UTXO A
Alice：100 KAS
```

不能直接變成：

```text
UTXO A
Alice：70 KAS
```

而是：

```text
消耗 UTXO A
       ↓
產生
UTXO B：Alice 70
UTXO C：Bob 30
```

所以更精確的說法是：

> **UTXO 是 immutable object，但整個 UTXO set 可以透過交易不斷演化。**

這個差異非常重要。

---

# 3. 「單次消耗」反而是 UTXO 的核心優勢

原文把：

> 單次消耗

放在「缺點」裡。

這其實要非常小心。

UTXO 的：

> **一個 UTXO 只能被成功花費一次**

正是它避免 double-spending 的核心設計之一。

例如：

```text
UTXO A = Alice 100
```

Alice 不能同時做：

```text
交易 T1：A → Bob 100
交易 T2：A → Charlie 100
```

因為：

```text
A
↓
T1 消耗
```

之後 A 已經不存在於 UTXO set。

所以：

> **「單次消耗」本身不是缺陷。**

真正的問題是：

> **如果你的應用需要頻繁操作「同一個共享狀態」，UTXO 模型需要透過消耗舊 UTXO、建立新 UTXO 的方式來表達狀態更新。**

這就可能比較麻煩。

---

# 4. 為什麼 DeFi 特別容易遇到這個問題？

假設我們做一個：

> Lending Pool

裡面有：

```text
Pool State
├── 總存款
├── 總借款
├── 利率
├── collateral
├── liquidation threshold
└── user balances
```

在 Account Model 裡，可以想像：

```text
User A 存錢
     ↓
Contract State 更新

User B 借錢
     ↓
同一個 Contract State 更新

User C 還款
     ↓
同一個 Contract State 更新
```

大家都在操作：

```text
        Shared State
       ↙     ↓      ↘
      A      B       C
```

所以非常自然。

---

## UTXO 就不同

你不能直接：

```text
Shared State
   ↓
修改一下
   ↓
Shared State'
```

而比較像：

```text
State UTXO
     ↓
被消耗
     ↓
State UTXO'
```

下一次：

```text
State UTXO'
     ↓
被消耗
     ↓
State UTXO''
```

所以如果很多人同時想操作同一個 State UTXO：

```text
A ─┐
B ─┼──→ Shared State UTXO
C ─┘
```

就會產生競爭。

這就是 UTXO 模型在「高度共享狀態」應用上的一個重要設計挑戰。

---

# 5. 但這裡剛好就是你前面看到 Covenant 的關鍵

這也是為什麼你前面那篇 Kaspa 文章會一直講：

> Constraint / Covenant  
> State transition  
> Successor  
> Inter-Covenant Communication

因為它其實是在回答：

> **「UTXO 模型既然沒有一個可以任意修改的全局共享狀態，那複雜智能合約怎麼做？」**

答案不是：

> 把 UTXO 變成 Ethereum。

而是：

> **利用 UTXO 本身的交易驗證規則，描述「下一個狀態必須長什麼樣子」。**

例如：

```text
現在 State UTXO
       ↓
允許某些交易
       ↓
新的 State UTXO
```

Covenant 可以進一步規定：

> 「你要花掉我這個 UTXO，可以，但你產生的下一個 UTXO 必須符合這些條件。」

於是：

```text
State₀
  ↓
Constraint
  ↓
State₁
  ↓
Constraint
  ↓
State₂
  ↓
Constraint
  ↓
State₃
```

這其實就是：

> **用一連串 UTXO 狀態轉移，建立一個具有持續性的應用程式狀態。**

---

# 6. 所以這句話真正應該改成什麼？

我會把原句改成：

> **UTXO 模型沒有像 Account Model 那樣提供一個可直接原地修改的全局共享狀態；每個 UTXO 通常只能被消耗一次，因此需要透過「消耗舊狀態 → 建立新狀態」來實現狀態轉移。這使得高度依賴共享、頻繁更新同一狀態的複雜智能合約，在程式設計與狀態協調上通常比 Account Model 更具挑戰，但並不代表 UTXO 無法實現複雜智能合約。**

這個版本邏輯嚴謹很多。

---

# 7. 從你正在學的「必要條件／充分條件」看

這裡非常適合用你最近學的概念。

有人可能會說：

> 「UTXO 沒有全局共享狀態 → 所以不能做複雜 DeFi。」

這個推論太強。

因為：

```text
沒有原生全局共享狀態
        ↓
實作複雜 DeFi 比較困難
```

**不等於：**

```text
沒有原生全局共享狀態
        ↓
不可能實作複雜 DeFi
```

也就是：

> 「Account Model 的共享狀態」可能是某些 DeFi 設計的**便利條件**，但不能直接說它是「複雜智能合約的必要條件」。

Covenant、state machine、multi-UTXO coordination 等技術就是反例。

---

## 最後把整個邏輯濃縮成一句

你可以把 UTXO vs Account Model 記成：

> **Account Model：修改「同一份狀態」。**  
> **UTXO Model：消耗「舊狀態」，創造「新狀態」。**

因此：

> **UTXO 的問題不是「不能做狀態」，而是「共享狀態的表達方式不同」。**

而 Kaspa Covenant 真正有意思的地方，就是試圖讓：

> **「UTXO 的不可變性」＋「可程式化的狀態轉移」＋「多個 Covenant 在同一交易中的協調」**

組合起來。

所以它不是在消滅 UTXO 的限制，而是在問一個更有意思的問題：

> **能不能利用 UTXO 原本的特性，反過來建立一套足以支撐複雜應用的狀態機？**

這才是你前面那篇文章真正的技術論點。