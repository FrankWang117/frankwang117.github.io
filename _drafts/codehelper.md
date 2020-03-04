在使用 macOS 的时候，电脑突然就风扇转个不停，声音是老烦人了。  
打开了 活动监视器，发现 codehelper 使用了大量的内存。  
这玩意肯定是 Visual Studio Code 的子程序，之前在 Linux 机子上碰到过。查了一下资料，
将 vscode 中的 followSymlinks 改为 false ，再次重启。即解决问题。