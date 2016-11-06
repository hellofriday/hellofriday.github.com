---
title: 解决hexo d命令提交部署博客发生的TaskCanceledException异常
categories: 博客搭建
description: 今天使用hexo d命令部署博客的时候发生了TaskCanceledException异常：
---
### 起因
今天使用hexo d命令部署博客的时候发生了下面的异常：
``` 
    Fatal: TaskCanceledException encountered.
   ▒▒ȡ▒▒һ▒▒▒▒▒▒
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Fatal: TaskCanceledException encountered.
   ��ȡ��һ��������
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument

    at ChildProcess.<anonymous> (E:\gitRep\hellofriday.github.com\node_modules\hexo-util\lib\spawn.js:37:17)
    at emitTwo (events.js:87:13)
    at ChildProcess.emit (events.js:172:7)
    at ChildProcess.cp.emit (E:\gitRep\hellofriday.github.com\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:818:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:211:5)
```

### 解决
hexo d的作用是将博客静态页面提交到远程仓库，这个功能是通过hexo-deployer-git工具实现的，就想着可能是这个工具出了问题，试着卸载重装了这个工具，不过还是会报错。自然就想到了一个办法：在hexo g生成静态页面之后，不使用hexo d提交，而是直接通过git命令行提交。不过这个方法有点繁琐。每次hexo clean之后，都需要创建本地仓库，重新关联远程仓库。后来在hexo的github issues找着了一个和上面发生的错误相似的issue[Problem with deployment on GitHub #1495](https://github.com/hexojs/hexo/issues/1495),  受到歪果仁朋友的启发，将我的git仓库地址改为ssh格式的路径，使用hexo d命令就可以成功部署了。下面给出这两个办法的详细步骤
#### git命令行提交
先切换到publish目录下 依次执行下面的命令
```
    git init 
    git remote add origin https://github.com/hellofriday/hellofriday.github.com.git
    git add .
    git commit -m 'add blog'
    git push -f origin master:gh-pages
```

#### repository路径:

之前在_config.yml文件中配置的git远程仓库地址使用的是https路径：

```

	deploy:
	  type: git
	  repository: https://github.com/hellofriday/hellofriday.github.com.git
	  branch: gh-pages

```

将路径改成ssh格式

```

	deploy:
	  type: git
	  #repository: https://github.com/hellofriday/hellofriday.github.com.git
	  repository: git@github.com:hellofriday/hellofriday.github.com.git
	  branch: gh-pages

```