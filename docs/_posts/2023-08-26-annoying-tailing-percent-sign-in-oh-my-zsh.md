---
layout: post
title:  'Oh My Zsh里面烦人的%'
date:   2023-08-26 23:39:00 +0800
categories: Shell
---

# 问题
在我的zsh shell里，我跑`echo -n "A sample string"`, 结果如图：
```
➜  ~ echo -n "A sample string"
A sample string%
➜  ~
```
在行尾有个烦人的`%`被加进去了。

# 解决方法
```
echo "export PROMPT_EOL_MARK=''" >> ~/.zshrc
source ~/.zshrc
```

# 效果
```
➜  ~ echo -n "A sample string"
A sample string
➜  ~
```

# 参考
- [why zsh adds "%" at the end of my output](https://stackoverflow.com/questions/36977990/why-zsh-adds-at-the-end-of-my-output)
