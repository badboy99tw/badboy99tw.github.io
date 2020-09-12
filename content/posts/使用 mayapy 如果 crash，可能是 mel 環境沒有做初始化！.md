Title: 使用 mayapy 如果 crash，可能是 MEL 環境沒有做初始化！
Slug: 使用 mayapy 如果 crash，可能是 MEL 環境沒有做初始化！
Date: 2014-09-24 17:58
Category: Note
Tags: Maya, mayapy, MEL

以下是一個會把 mayapy 搞爛的例子。

```python
import maya.standalone
maya.standalone.initialize(name='python')

import maya.cmds as cmds
cmds.loadPlugin('Mayatomr')
```
<!--more-->

但是神奇的事情來了，只要我有先 import pymel，
那之後再 load mentalray 就不會讓 mayapy 爛掉，
所以一定是 pymel 裡面做了什麼事情！

偷看一下原來是 pymel.internal.startup 裡面有個 initMEL 的 function，
程式會 source 一些分布在 maya 安裝目錄和 prefs 底下做 startup 的 mel file。

```python
def initMEL():
    if 'PYMEL_SKIP_MEL_INIT' in os.environ or pymel_options.get('skip_mel_init', False):
        _logger.info("Skipping MEL initialization")
        return

    _logger.debug("initMEL")
    mayaVersion = versions.installName()
    prefsDir = getUserPrefsDir()
    if prefsDir is None:
        _logger.error(
            "could not initialize user preferences: MAYA_APP_DIR not set")
    elif not os.path.isdir(prefsDir):
        _logger.error(
            "could not initialize user preferences: %s does not exist" % prefsDir)

    # TODO : use cmds.internalVar to get paths
    # got this startup sequence from autodesk support

    startup = [
        # 'defaultRunTimeCommands.mel',  # sourced automatically
        # os.path.join( prefsDir, 'userRunTimeCommands.mel'), # sourced
        # automatically
        'createPreferencesOptVars.mel',
        'createGlobalOptVars.mel',
        os.path.join(prefsDir, 'userPrefs.mel') if prefsDir else None,
        'initialStartup.mel',
        #$HOME/Documents/maya/projects/default/workspace.mel
        'initialPlugins.mel',
        # 'initialGUI.mel', #GUI
        # 'initialLayout.mel', #GUI
        # os.path.join( prefsDir, 'windowPrefs.mel'), #GUI
        # os.path.join( prefsDir, 'menuSetPrefs.mel'), #GUI
        # 'hotkeySetup.mel', #GUI
        'namedCommandSetup.mel',
        os.path.join(prefsDir, 'userNamedCommands.mel') if prefsDir else None,
        # 'initAfter.mel', #GUI
        os.path.join(prefsDir, 'pluginPrefs.mel') if prefsDir else None
    ]
    try:
        for f in startup:
            _logger.debug("running: %s" % f)
            if f is not None:
                if os.path.isabs(f) and not os.path.exists(f):
                    _logger.warning("Maya startup file %s does not exist" % f)
                else:
                    # need to encode backslashes (used for windows paths)
                    if isinstance(f, unicode):
                        encoding = 'unicode_escape'
                    else:
                        encoding = 'string_escape'
                    #import pymel.core.language as lang
                    #lang.mel.source( f.encode(encoding)  )
                    import maya.mel
                    maya.mel.eval('source "%s"' % f.encode(encoding))

    except Exception, e:
        _logger.error(
            "could not perform Maya initialization sequence: failed on %s: %s" % (f, e))

    try:
        # make sure it exists
        res = maya.mel.eval('whatIs "userSetup.mel"')
        if res != 'Unknown':
            maya.mel.eval('source "userSetup.mel"')
    except RuntimeError:
        pass

    _logger.debug("done running mel files")
```

## 結論

不想 import 肥滋滋的 pymel 的話，直接幹上面這個 initMEL 來用就好，
或是 import pymel.internal.startup 再執行 initMEL。

更懶的話可以只 source 底下幾個 MEL file。

```python
import maya
maya.mel.eval('source createPreferencesOptVars.mel')
maya.mel.eval('source createGlobalOptVars.mel')
maya.mel.eval('source initialStartup.mel')
```

好久沒寫文章了，這篇又靠複製貼上弄得好像文章很長，
似乎有點廢啊...
