在更新完 macOS 为最新版的 Catalina 之后，打开命令行会提示使用 zsh 作为默认 shell 的命令，输入之后
会在下次重启使用例如 node 等命令的时候出现：
``` shell
env: node: No such file or directory
```
这是只需要在`~/.zshrc`最后新增一行`source ~/.bash_profile`，然后关闭终端，重新打开即可。