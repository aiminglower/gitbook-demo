# Section2.2


### 2019年02月05日23:54:53

### 测试阶段

1. 主要是测试脚本

2. 第二次测试：
nohup ... & 命令是负责将进程挂起的，但是他的日志还是会输出到nohup.out
加上 > /dev/null 则不保存输出，之后可以更改为一个日志文件，将日志保存下来

3. 我的一个猜测是，上面的改变还是会有报错，原因是，git hook 拒绝让一个命令一直在机器运行，他只接受那些马上执行然后马上结束的命令，而不是使用了nohup这种会在后台继续运行的命令。

4. 把 nohup 这段删掉就知道是不是因为这一句导致的了，之前的脚本打印记录虽然是到ps -ef ，但是已经执行到kill 那一步了，现在让gitbook继续运行，不杀掉进程，也不启动，看看问题出在哪里

5. gitbook 好像是有自动reloading的，只要加上参数就可以了；如果是这样的话，只要git pull了。或者gitbook build了就行（后面build的方法确实是可以的，只要build过程中不报错，serve就不会停止运行；如果编译出错了其实也不怕，因为他会在本地git push 的时候打印出来）。

gitbook help 的 serve 部分内容：
> serve [book] [output]       serve the book as a website for testing
>         --port                  Port for server to listen on (Default is 4000)
>         --lrport                Port for livereload server to listen on (Default is 35729)
>         --[no-]watch            Enable file watcher and live reloading (Default is true)
>         --[no-]live             Enable live reloading (Default is true)
>         --[no-]open             Enable opening book in browser (Default is false)
>         --browser               Specify browser for opening book (Default is )
>         --log                   Minimum log level to display (Default is info; Values are debug, info, warn, error, disabled)
>         --format                Format to build to (Default is website; Values are website, json, ebook)

--watch 加上后可能会随着file文件的改变，自动改变

--live 默认是true，所以build之后，serve就自动更新了