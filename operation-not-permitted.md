
>非applestore下载文件执行被拒的问题，因为没有验证，所以osx系统添加了标记


#operation-not-permitted
*转载需加来源*

例如执行 plugname

`
zsh: operation not permitted: plugname
`

解决，先查看，再删除：（path为你执行的路径）

`
➜  /Users/path git:(master) xattr -l /usr/local/bin/plugname

com.apple.quarantine: 0086;583e9a2b;WeChat;

➜  /Users/path git:(master) xattr -d com.apple.quarantine /usr/local/bin/plugname

`

