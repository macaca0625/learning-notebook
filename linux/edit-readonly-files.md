記錄一下上次遇到需要修改某些唯獨的檔案，如果以一般的方法對他下 vim 指令 

```
$vim file.php
```

是可以進入編輯畫面的，但是當想要 :wq 儲存修改的內容時，會出現警告提示

> **E45: 'readonly' option is set \(add ! to override\)**

然後就只能摸摸鼻子 :q! 強制退出，網路上之前有查到一招 `:wq !sudo tee %` ，但是會觸發另一個警告

> **E172: Only one file name allowed**

到最後發現最一勞永逸的解法還是就直接在 vim 他的時候就同時 sudo 去要權限，像這樣

```
$sudo vim file.php
```

這樣在 :wq 的時候就可以順暢的儲存對檔案的修改了。

___


參考資料

[http://feihu.me/blog/2014/vim-write-read-only-file/](http://feihu.me/blog/2014/vim-write-read-only-file/)

