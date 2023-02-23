# tools set

### 1. strace
system call trace 
记录系统调用过程
```sh
strace ls
system call history...

-o 指定输出文件
-f 在另一个窗口中执行
```

### 2. lsof
list open file
查看打开的文件
```sh
-p 按进程号查看打开的文件
-i 端口
```

### 3. watch
持续执行命令
```sh
-t 
-n 1
```

- 自制cpu主频 监控器
```sh
watch -n 1 "cat /proc/cpuinfo | grep MHz | awd '{print \$1 NR \$3 \$4 \$2}'"
```
- 打包特定文件上传到远端
```sh
find . -name "*.pdf" | xargs tar cj > a.
```

### 4. thefuck
 命令行纠错
 ```sh
$ wrong cmd
$ fuck
 ...
```

### 5. tmux
- 保存 session 到硬盘上:
>https://zhuanlan.zhihu.com/p/146544540


- 终端分屏复用软件
- 快捷键为 `ctrl-b` 前缀
- 使用 `C-b + ?` 进入提示菜单
- 
#### hot key
- pane
> -   `Ctrl+b d`：分离当前会话。
> -   `Ctrl+b s`：列出所有会话。
> -   `Ctrl+b $`：重命名当前会话。
> -   `Ctrl+b %`：划分左右两个窗格。
> -   `Ctrl+b "`：划分上下两个窗格。
> -   `Ctrl+b <arrow key>`：光标切换到其他窗格。`<arrow key>`是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键`↓`。
> -   `Ctrl+b ;`：光标切换到上一个窗格。
> -   `Ctrl+b o`：光标切换到下一个窗格。
> -   `Ctrl+b {`：当前窗格与上一个窗格交换位置。
> -   `Ctrl+b }`：当前窗格与下一个窗格交换位置。
> -   `Ctrl+b Ctrl+o`：所有窗格向前移动一个位置，第一个窗格变成最后一个窗格。
> -   `Ctrl+b Alt+o`：所有窗格向后移动一个位置，最后一个窗格变成第一个窗格。
> -   `Ctrl+b x`：关闭当前窗格。
> -   `Ctrl+b !`：将当前窗格拆分为一个独立窗口。
> -   `Ctrl+b z`：当前窗格全屏显示，再使用一次会变回原来大小。
> -   `Ctrl+b Ctrl+<arrow key>`：按箭头方向调整窗格大小。
> -   `Ctrl+b q`：显示窗格编号。

- window
> -   `Ctrl+b c`：创建一个新窗口，状态栏会显示多个窗口的信息。
> -   `Ctrl+b p`：切换到上一个窗口（按照状态栏上的顺序）。
> -   `Ctrl+b n`：切换到下一个窗口。
> -   `Ctrl+b <number>`：切换到指定编号的窗口，其中的`<number>`是状态栏上的窗口编号。
> -   `Ctrl+b w`：从列表中选择窗口。
> -   `Ctrl+b ,`：窗口重命名。
#### 常用指令
- 会话管理
```bash
- 创建一个新的回话
tmux new -s <session-name>

- 让当前会话后台运行
C-b + d

- 查看后台的窗口
tmux ls

- 接入会话
tmux attach -t 0(窗口号)
tmux attach -t <session-name> 

- 切换会话
tmux switch -t 0
... <session-name>

- 重命名会话
tmux rename-session -t old(数字也可以更名) new

```
- 分屏
```bash
- 上下分屏
tmux split-window 

- 左右分屏
tmux split-window -h

- 切换focus
tmux selsect-pane -U/D/L/R

- 交换pane顺序
tmux swap-pane -U  # 当前pane上移
tmux swap-pane -D  # 当前pane 下移
```
- 窗口管理
```bash
- 新建窗口
tmux new-windows {-n <windows name> }

- 切换窗口
tmux select-windows -t <number/name>

- 重命名current窗口
tmux rename-window <new-name>
```

