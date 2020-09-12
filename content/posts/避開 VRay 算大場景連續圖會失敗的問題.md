Title: 避開 VRay 算大場景連續圖會失敗的問題
Slug: 避開 VRay 算大場景連續圖會失敗的問題
Date: 2014-07-06 10:34
Category: Note
Tags: Maya, VRay, render

在 Maya 算圖，如果沒有 Render Farm 的話，只能透過 Maya Render 一張一張慢慢算。

但是 VRay 這個白癡，算圖的時候會把整個場景先轉成 vrscene 到系統的 temp 再算圖。在簡單場景或張數很少的情況，這作法還算無感，但如果是大場景或超長的卡，可能算個圖要先等個半小時一小時，而且等待的時間可能會讓你覺得 Maya 壞掉而重開機。

就算你認命了決定等待，但也有可能 temp 爆掉讓 vrscene 產生失敗，最後圖還是算不出來。

當時同事提供了一個簡單的解法，簡單來說，就是用 maya 的 render command 一張一張算圖，以下是範例：

```c
{
    int $start = 101;
    int $end = 200;
    for (int $fr = $start; $fr < $end; $fr++) {
        setAttr "defaultRenderGlobals.startFrame" $fr;
        setAttr "defaultRenderGlobals.endFrame" $fr;
        RenderIntoNewWindow;
    }
}
```

因為每個 frame 都下一次 render 指令，所以 vray 就不會把 100 個 frames 的算圖資訊放在同一個 vrscene 裡面了。

是說我還是不懂 VRay 在想什麼...

## Refs

- http://download.autodesk.com/us/maya/2010help/Commands/render.html
