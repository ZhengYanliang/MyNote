##ORA-12560:TNS:protocol adapter error
```
扫描服务器打开的端口
``` ##sqlplus不是内部或外部命令
```



首先，确认oracle安装路径下的根目录%oracle_home%/bin目录下的sqlplus.exe、imp.exe、exp.exe等可执行文件能否正常运行！如果不能运行，那oracle安装文件可能被破坏了，考虑重装oracle；如果可以，看第二步。
            接着，在windows环境变量下，添加%oracle_home%/bin路径。具体操作：右击“我的电脑”—>“高级”—>“环境变量”—>path；然后添加";%oracle_home%/bin"这样就OK了！
```