在 shell 中执行： 
* screen -ls  列出当前所有的 session
* screen -r stack  回到 devstack 这个 session

在 screen 中执行：
* Ctrl+a+n 切换到下一个窗口
* Ctrl+a+p 切换到前一个窗口(与 Ctrl+a+n 相对)
* Ctrl+a+0..9 切换到窗口 0..9
* Ctrl+a+d 暂时断开（detach）当前 screen 会话，但不中断 screen 窗口中程序的运行