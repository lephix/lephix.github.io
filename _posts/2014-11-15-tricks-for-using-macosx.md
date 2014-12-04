---
layout: post
title: Tricks for using Mac OS X
---

## Bash setting
`~/.bash_profile` Will be executed for every time an Terminal get started for the specific user.  
`/etc/profile` for all users.

~~~bash
export CLICOLOR=1
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

alias ll="ls -l"

export JAVA_HOME=
export M2_HOME=
~~~

## Useful command lines

### Apache2 
~~~bash
sudo apachectl -k start|stop|restart
~~~

### Mysql
Mysql will be installed at `/usr/local/mysql` through `.DMG` file.

~~~bash
sudo /usr/local/mysql/bin/mysqld_safe
~~~
