Title: 在 Mac 上使用 Evernote，如何關掉單、雙引號的自動轉換？
Slug: 在 Mac 上使用 Evernote，如何關掉單、雙引號的自動轉換？
Date: 2015-07-12 17:22
Category: Note
Tags: MacOS, Evernote

這一年開始拿 Mac 作為工作機，在使用 Evernote 時常遇到煩人的狀況，就是每當在 Note 輸入雙引號（"），都會被自動換成（“），單引號（'）會被換成（‘）。因為常用的 bash command 會存在 Evernote 裡面，每次從 Evernote 剪下貼到 Terminal 的時候都會執行錯誤，實在覺得~~很靠北~~非常惱人。

本來以為是 Evernote 雞婆，上網查了一下才發現原來是 Mac 設定的問題，只要把 use smart quotes 關掉就好了。

`System Preferences -> Language and Region -> Keyboard preferences -> Text -> Uncheck "use smart quotes"`

## Refs

* [Evernote on Mac automagically and annoyingly replacing apostrophe with left single quote][1]

[1]: https://discussion.evernote.com/topic/52621-evernote-on-mac-automagically-and-annoyingly-replacing-apostrophe-with-left-single-quote/ "Evernote on Mac automagically and annoyingly replacing apostrophe with left single quote"
