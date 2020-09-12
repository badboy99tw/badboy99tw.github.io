Title: 從 Cygwin 執行 Windows 程式
Slug: 從 Cygwin 執行 Windows 程式
Date: 2014-07-01 14:58
Category: Note
Tags: Cygwin, Windows

工作了幾年，才漸漸接受自己必須在 Windows 底下做事的事實。

一開始比較逃避現實的解法是透過 VirtualBox 安裝 Ubuntu，繼續躲在 Linux 溫柔鄉。但現在的工作沒有權限修改 BIOS 來開啟硬體加速的支援，導致 VirtualBox 速度慢到無法接受！寫一個 PyQt 的程式，Widget 居然龜速到一個一個慢慢跳出來！

只好改變戰略投靠 Cygwin！

從 Cygwin 執行 Windows 程式其實很簡單，打程式的名稱就好。但如果透過 [cygpath](http://cygwin-lite.sourceforge.net/html/cygpath.html) 做路徑轉換，還可以做到無縫接軌！

以下是我用來執行 explorer 的 wrapper。

```bash
#!/bin/bash
explorer.exe "$(cygpath -pw $1)"
```

這是個非常簡單的 wrapper。從此可以在 Cygwin 無痛開啟資料夾，不必再打 explorer.exe /cygdrive/c/ooxx 這種囉嗦的路徑了！
