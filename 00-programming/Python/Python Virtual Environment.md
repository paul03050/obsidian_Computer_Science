要在 Python 中安裝 [openpyxl](https://hackmd.io/@howkii-studio/python_autoporcessing_xl) 且不要影響到全域環境，最正規、乾淨的做法是建立「虛擬環境」（Virtual Environment）。 [1]

請在你的專案資料夾內，依序執行以下步驟：

## 1. 建立虛擬環境

打開終端機（Terminal 或命令提示字元），切換到你的專案目錄，並執行以下指令建立一個名為 `venv` 的獨立環境：

```bash
python -m venv venv
```

## 2. 啟用虛擬環境

根據你的作業系統，執行對應的啟用指令：

- Windows (Command Prompt):
    
    ```cmd
    venv\Scripts\activate
    ```
    
- Windows (PowerShell):
    
    ```powershell
    .\venv\Scripts\activate
    ```
    
- macOS / Linux:
    
    ```bash
    source venv/bin/activate
    ```
    

_啟用成功後，你會看到終端機提示字元前方多了一個 `(venv)` 的標記。_

## 3. 在虛擬環境中安裝 openpyxl

在啟用狀態下執行 pip 安裝，此時套件只會下載並鎖定在該專案的 `venv` 資料夾中，完全不碰觸全域環境： [1]

```bash
pip install openpyxl
```

---

## 💡 快捷替代方案：單次區域安裝（不使用虛擬環境）

如果你不想建立虛擬環境資料夾，只想把 openpyxl 安裝在目前使用者的個人目錄下（不污染系統全域，但該電腦的其他個人專案仍吃得到），可以使用 `--user` 參數：

```bash
pip install --user openpyxl
```

你目前是用哪一套編輯器（例如 VS Code、PyCharm）來開發呢？如果需要，我可以接著告訴你如何在編輯器中切換成剛剛建立的虛擬環境！

  

[1] [https://www.reddit.com](https://www.reddit.com/r/learnpython/comments/p2b76c/installing_modules_pandas_openpyxl/?tl=zh-hant)