Title: 安裝 Windows 版 pylint 和 Sublime Pylinter
Slug: 安裝 Windows 版 pylint 和 Sublime Pylinter
Date: 2014-08-01 16:50
Category: Note
Tags: Windows, Pylint, Sublime

最近在整理寫程式的環境，打算使用 Sublime 下的 [Pylinter](https://sublime.wbond.net/packages/Pylinter) 和 [Python PEP8 Autoformat](https://sublime.wbond.net/packages/Python%20PEP8%20Autoformat) 做程式碼的規範。但因為工作環境已經把 python 執行檔及 modules 分開了，所以預設的安裝方式及設定是無法使用的，Pylinter 會找不到 python 執行檔跟 pylint，只好手動安裝。

## 安裝 pylint / Sublime Pylinter

1. 安裝 pylint 和需要用到的 modules
    - [pylint](https://pypi.python.org/pypi/pylint)
    - [logilab-common](https://pypi.python.org/pypi/logilab-common)
    - [astroid](https://pypi.python.org/pypi/astroid)
2. 安裝 [Pylinter](https://sublime.wbond.net/packages/Pylinter)
3. 修改 Pylinter 設定

```json
{
    "python_bin": "/path/to/python.exe",
    "python_path": [
        "/path/to/pylint",
        "/path/to/logilab-common",
        "/path/to/astroid"],
    "pylint_path": "/path/to/pylint/lint.py",
}
```
目前已知問題是 Pylinter 抓不到 pylint 的版本，可能要 trace code 研究一下，但執行起來好像沒什麼問題。

## Pylinter 其他設定

- verbose
    - 開啟後 console 會印出 pylint 執行的訊息，可惜的是打分數那段沒看到，不知道為什麼。
- use_icons
    - 開啟後 sublime 左邊顯示 error 的圖案會因檢查的結果有所差別，不開的話就只是個白點。
- run_on_save
    - 開啟後每次存檔會跑 pylint，其實有點花時間，建議手動執行 Pylinter 就好。

## Pylinter 熱鍵

- ctrl+alt+z
    - 執行 Pylinter
- ctrl+alt+x
    - 開啟 / 關閉 Pylinter 結果
- ctrl+alt+c
    - 條列 Pylinter 結果
- ctrl+alt+i
    - 忽略游標所在該行的錯誤

附帶一提，只有 ctrl+alt+z 會重跑 Pylinter，其他熱鍵只會改變顯示方式。
