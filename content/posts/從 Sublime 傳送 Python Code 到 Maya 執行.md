Title: 從 Sublime 傳送 Python Code 到 Maya 執行
Slug: 從 Sublime 傳送 Python Code 到 Maya 執行
Date: 2014-07-03 16:15
Category: Note
Tags: Sublime, Python, Maya

在 Maya 寫 Python 程式的派別不外乎：

* Script Editor 派
    * 打開 Maya 的 Script Editor，直接寫直接跑

* 其他文字編輯器派（vim、sublime、eclipse 等等）
    * 透過系統設定修改環境變數 PYTHONPATH，再經由 import/reload module 做測試。
    * 透過 Maya.env 修改環境變數 PYTHONPATH，再經由 import/reload module 做測試。
    * 透過 sys.path 加入工作目錄，再經由 import/reload module 做測試。

如果是使用 Sublime 為主力編輯器的朋友，其實可以透過 MayaSublime 這個 Sublime Package 來改善開發流程！

## 安裝步驟

* 安裝 Sublime Package Control，[官網](https://sublime.wbond.net/installation)有安裝的方法，大家可以參考。
* 透過 Package Control 安裝 MayaSublime。
* 打開 Maya，執行下面這段 Python 程式碼來開啟 Command Port。

```python
import maya.cmds as cmds
cmds.commandPort(name=":7002", sourceType="python")
```

* 用 Sublime 打開任何一個 Python 檔（或是開新檔後存成 Python 檔），在裡面打上一段測試的程式。此時按下 `Ctrl + Enter`，反白選取的程式碼就會被傳送到 Maya 內執行。如果沒有選取任何程式，則整個 Python 檔的內容都會被傳送到 Maya 內執行。

這邊要注意的是，通常大家會同時開好幾個 Maya 工作，但是只能有一個 Maya 跟 Sublime 溝通喔。

另外，如果預設的 Command Port 7002 和其他程式有衝突的話，可以在 Sublime 內到下面的位置做修改。

`Sublime -> Preferences -> Package Settings -> MayaSublime -> Settings - User`

加入下面這段設定，將 7002 改成期望的 port 後存檔即可。

```json
{
    "python_command_port" : 7002
}
```

其實 Mel 也可以這樣搞，但是基於私心我就不寫了，科科。

## Refs

* [MayaSublime](https://github.com/justinfx/MayaSublime)
* [Package Control](https://sublime.wbond.net/)
* [SEND MEL/PYTHON CODE FROM SUBLIME TEXT TO MAYA](http://fredrik.averpil.com/post/55507118045)
