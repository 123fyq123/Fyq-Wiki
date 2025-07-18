## Tmux教程

### 功能

+   分屏
+   允许断开`Terminal`连接后，继续运行进程。

### 结构

 一个`tmux`可以包含多个`session`，一个`session`可以包含多个`window`，一个`window`可以包含多个`pane`。

**实例：**
```
    tmux:
        session 0:
            window 0:
                pane 0
                pane 1
                pane 2
                ...
            window 1
            window 2
            ...
        session 1
        session 2
        ...
```

### 操作

+   `tmux`：新建一个`session`，其中包含一个`window`，`window`中包含一个`pane`，`pane`里打开了一个`shell`对话框。
+   按下`Ctrl + a`后手指松开，然后按`%`：将当前`pane`左右平分成两个`pane`。
+   按下`Ctrl + a`后手指松开，然后按"（注意是双引号"）：将当前`pane`上下平分成两个`pane`。
+   `Ctrl + d`：关闭当前`pane`；如果当前`window`的所有`pane`均已关闭，则自动关闭`window`；如果当前`session`的所有`window`均已关闭，则自动关闭`session`。
+   鼠标点击可以选`pane`。
+   按下`ctrl + a`后手指松开，然后按方向键：选择相邻的`pane`。
+   鼠标拖动`pane`之间的分割线，可以调整分割线的位置。
+   按住`ctrl + a`的同时按方向键，可以调整`pane`之间分割线的位置。
+   按下`ctrl + a`后手指松开，然后按`z`：将当前`pane`全屏/取消全屏。
+   按下`ctrl + a`后手指松开，然后按`d`：挂起当前`session`。
+   `tmux a`：打开之前挂起的`session`。
+   按下`ctrl + a`后手指松开，然后按`s`：选择其它`session`。
    +   `方向键 —— 上`：选择上一项 `session/window/pane`
    +   `方向键 —— 下`：选择下一项 `session/window/pane`
    +   `方向键 —— 右`：展开当前项 `session/window`
    +   `方向键 —— 左`：闭合当前项 `session/window`
+   按下`Ctrl + a`后手指松开，然后按`c`：在当前`session`中创建一个新的`window`。
+   按下`Ctrl + a`后手指松开，然后按`w`：选择其他`window`，操作方法与(12)完全相同。
+   按下`Ctrl + a`后手指松开，然后按`PageUp`：翻阅当前`pane`内的内容。
+   鼠标滚轮：翻阅当前`pane`内的内容。
+   在`tmux`中选中文本时，需要按住`shift`键。（仅支持`Windows`和`Linux`，不支持`Mac`，不过该操作并不是必须的，因此影响不大）
+   `tmux`中复制/粘贴文本的通用方式：
    +   按下`Ctrl + a`后松开手指，然后按[
    +   用鼠标选中文本，被选中的文本会被自动复制到`tmux`的剪贴板
    +   按下`Ctrl + a`后松开手指，然后按]，会将剪贴板中的内容粘贴到光标处

---

## Vim教程

### 功能

+   命令行模式下的文本编辑器。
+   根据文件扩展名自动判别编程语言。支持代码缩进、代码高亮等功能。
+   使用方式：`vim filename`
    +   如果已有该文件，则打开它。
    +   如果没有该文件，则打开个一个新的文件，并命名为`filename`

### 模式

+   一般命令模式
    +   默认模式。命令输入方式：类似于打游戏放技能，按不同字符，即可进行不同操作。可以复制、粘贴、删除文本等。
+   编辑模式
    +   在一般命令模式里按下i，会进入编辑模式。
    +   按下`ESC`会退出编辑模式，返回到一般命令模式。
+   命令行模式
    +   在一般命令模式里按下`:/?`三个字母中的任意一个，会进入命令行模式。命令行在最下面。
    +   可以查找、替换、保存、退出、配置编辑器等。

### 操作

+   `i`：进入编辑模式
+   `ESC`：进入一般命令模式
+   `h` 或 `左箭头键`：光标向左移动一个字符
+   `j` 或 `向下箭头`：光标向下移动一个字符
+   `k` 或 `向上箭头`：光标向上移动一个字符
+   `l` 或 `向右箭头`：光标向右移动一个字符
+   `n<Space>`：`n`表示数字，按下数字后再按空格，光标会向右移动这一行的`n`个字符
+   `0` 或 功能键`[Home]`：光标移动到本行开头
+   `$` 或 功能键`[End]`：光标移动到本行末尾
+   `G`：光标移动到最后一行
+   `:n` 或 `nG`：`n`为数字，光标移动到第`n`行
+   `gg`：光标移动到第一行，相当于`1G`
+   `n<Enter>`：`n`为数字，光标向下移动`n`行
+   `/word`：向光标之下寻找第一个值为`word`的字符串。
+   `?word`：向光标之上寻找第一个值为`word`的字符串。
+   `n`：重复前一个查找操作
+   `N`：反向重复前一个查找操作
+   `:n1,n2s/word1/word2/g`：`n1`与`n2`为数字，在第`n1`行与`n2`行之间寻找`word1`这个字符串，并将该字符串替换为`word2`
+   `:1,$s/word1/word2/g`：将全文的`word1`替换为`word2`
+   `:1,$s/word1/word2/gc`：将全文的`word1`替换为`word2`，且在替换前要求用户确认。
+   `v`：选中文本
+   `d`：删除选中的文本
+   `dd`: 删除当前行
+   `y`：复制选中的文本
+   `yy`: 复制当前行
+   `p`: 将复制的数据在光标的下一行/下一个位置粘贴
+   `u`：撤销
+   `Ctrl + r`：取消撤销
+   大于号 `>`：将选中的文本整体向右缩进一次
+   小于号 `<`：将选中的文本整体向左缩进一次
+   `:w` 保存
+   `:w!` 强制保存
+   `:q` 退出
+   `:q!` 强制退出
+   `:wq` 保存并退出
+   `:set paste` 设置成粘贴模式，取消代码自动缩进
+   `:set nopaste` 取消粘贴模式，开启代码自动缩进
+   `:set nu` 显示行号
+   `:set nonu` 隐藏行号
+   `gg=G`：将全文代码格式化
+   `:noh` 关闭查找关键词高亮
+   `Ctrl + q`：当`vim`卡死时，可以取消当前正在执行的命令

### 异常处理

用`vim`编辑文件时，会自动创建一个`.filename.swp`的临时文件。

如果打开某个文件时，该文件的`swp`文件已存在，则会报错。此时解决办法有两种

+   找到正在打开该文件的程序，并退出
+   直接删掉该`swp`文件即可