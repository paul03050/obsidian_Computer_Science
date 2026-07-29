在 Windows 的命令提示字元 (CMD) 中，最常用的 Python 相關指令可以分為==基本檢查、執行程式、套件管理==三大類： [1, 2]

## 🛠️ 基本檢查與進入互動模式

- `python --version` 或 `python -V`：檢查電腦安裝的 Python 版本。
- `python`：進入 Python 互動模式（畫面會出現 `>>>`，可直接輸入程式碼）。
- `exit()` 或 `Ctrl + Z` 再按 Enter：退出 Python 互動模式。 [1, 3, 4, 5]

## 🚀 執行 Python 檔案

- `cd 檔案路徑`：切換 CMD 視窗到你存放程式碼的資料夾路徑。
- `dir`：查看目前資料夾底下的所有檔案清單。
- `python 檔名.py`：執行指定的 Python 程式（例如 `python main.py`）。
- `Ctrl + C`：強制停止正在執行的 Python 程式。 [2, 6, 7]

## 📦 pip 套件管理指令（在 CMD 直接輸入）

- `pip install 套件名稱`：安裝第三方套件（例如 `pip install requests`）。
- `pip uninstall 套件名稱`：移除已安裝的套件。
- `pip list`：列出目前環境中所有已安裝的套件與版本。

💡 常見問題：如果輸入 `python` 卻跳出微軟商店或顯示「不是內部或外部命令」，代表你在安裝 Python 時忘記勾選 "Add python.exe to PATH"（加入環境變數），需要重新執行安裝檔並勾選該選項。 [1, 8]

請問你目前是想執行寫好的 `.py` 檔案，還是要安裝特定的 Python 套件呢？ [6]

  

[1] [https://www.lccnet.com.tw](https://www.lccnet.com.tw/lccnet/article/details/1706)

[2] [https://www.dotblogs.com.tw](https://www.dotblogs.com.tw/YiruAtStudio/2020/12/21/203231)

[3] [https://ithelp.ithome.com.tw](https://ithelp.ithome.com.tw/articles/10350865)

[4] [https://docs.python.org](https://docs.python.org/zh-tw/3.13/faq/windows.html)

[5] [https://developers.google.com](https://developers.google.com/edu/python/set-up?hl=zh-tw)

[6] [https://blog.csdn.net](https://blog.csdn.net/tyq_666/article/details/122010940)

[7] [https://www.reddit.com](https://www.reddit.com/r/learnpython/comments/111e0gu/what_is_the_simplest_way_to_run_a_a_python_script/?tl=zh-hant)

[8] [https://www.reddit.com](https://www.reddit.com/r/learnpython/comments/1cx2g9w/change_actual_pythonversion_in_cmd/?tl=zh-hant)