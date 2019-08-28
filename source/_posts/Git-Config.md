---
title: Git Config
date: 2019-08-21 21:28:00
tags: 
- Engineering
- Git
- Config
categories:
- Engineering
---

When I started my first full-time software engineer job at Snowflake Computing, I felt that the speed of typing terminal commands is very annoying. Sometimes it took more than 2 seconds to finish the command. Finally I found the reason is that I was using zsh. Zsh has the git plugin and it will scan the dirty change after running each command. Snowflake Computing is using a big shared repository, so it is very expensive to scan the whole repo. The only thing I need to do is:

``` bash
$ git config --global --add oh-my-zsh.hide-dirty 0
# --global is added for chaning the config for all git repos
```

Below is some personal shortcut of git usage. This may help save some time.

``` bash
$ git config --global alias.br branch
$ git config --global alias.a add
$ git config --global alias.d diff
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.s show
$ git config --global alias.pl pull
$ git config --global alias.ps push
$ git config --global alias.rb rebase
$ git config --global alias.cp cherry-pick
$ git config --global alias.l log
$ git config --global alias.co checkout
```
