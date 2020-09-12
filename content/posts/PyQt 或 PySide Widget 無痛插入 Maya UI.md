Title: PyQt 或 PySide Widget 無痛插入 Maya UI
Slug: PyQt 或 PySide Widget 無痛插入 Maya UI
Date: 2014-07-22 03:00
Category: Note
Tags: Maya, PyQt, PySide, Python

雖然用 PyQt / PySide 寫的 Widget 可以爽爽跨平台跟跨軟體，但面對以前使用 maya.cmds 寫的 GUI，如果要全部換成 PyQt / PySide 應該會痛不欲生。若仍想例用 PyQt / PySide 添加新的介面及功能，卻又不打算修改舊有的 GUI 架構，該如何是好？！

為了讓新的 PyQt / PySide Widget 無痛插入舊有的 Maya 工具，可以利用下面這個 function 將 qt widget 轉成 maya widget。

```python
def to_maya_widget(qt_widget):
    layout = cmds.columnLayout(adjustableColumn=True)
    qtobj = shiboken.wrapInstance(long(mui.MQtUtil.findLayout(layout)), QtCore.QObject)
    qtobj.children()[0].layout().addWidget(qt_widget)
    cmds.setParent('..')
```

想法很簡單

1. maya.cmds 產生一個之後要拿來塞 Widget 的 layout
2. 利用 shiboken.wrapInstance / sip.wrapinstance 將 maya layout 轉成 QObject
3. 從 QObject 取得 qt layout，再透過 addWidget 加入 qt widget 就可以了！

完整測試程式在此

測試環境：Mac OSX 10.9.4 + Maya 2015 + PySide + shiboken

```python
from PySide import QtCore
from PySide import QtGui
import shiboken

import maya.cmds as cmds
import maya.OpenMayaUI as mui

def to_maya_widget(qt_widget):
    layout = cmds.columnLayout(adjustableColumn=True)
    qtobj = shiboken.wrapInstance(long(mui.MQtUtil.findLayout(layout)), QtCore.QObject)
    qtobj.children()[0].layout().addWidget(qt_widget)
    cmds.setParent('..')

def pyside_button(label, command):
    btn = QtGui.QPushButton(label)
    btn.clicked.connect(command)
    to_maya_widget(btn)

def maya_press_callback(*args):
    print 'Hello Maya'

def pyside_press_callback(*args):
    print 'Hello PySide'

win = cmds.window(width=150)
main_layout = cmds.columnLayout(adjustableColumn=True)
cmds.button(label='Maya Button', command=maya_press_callback)
pyside_button(label='PySide Button', command=pyside_press_callback)
cmds.button(label='Maya Button 2', command=maya_press_callback)
cmds.showWindow()
```

從程式碼可以看到，pyside_button 就這樣神不知鬼不覺的混進去了！只要介面再裝扮一下，一切就是那麼自然～

既然維護舊程式是逃避不了的宿命，那希望這個小程式可以帶來一點光（是說通常重寫比較爽...）

## Refs

- [PyQt Widget in Maya Mel UI problem](http://tech-artists.org/forum/showthread.php?2845-PyQt-Widget-in-Maya-Mel-UI-problem)
- [How to get maya main window pointer using PySide?](http://stackoverflow.com/questions/22331337/how-to-get-maya-main-window-pointer-using-pyside)
