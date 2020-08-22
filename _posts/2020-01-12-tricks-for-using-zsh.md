---
title: Tricks for using Zsh
---

## Introduction
Zsh is a terminal command like bash and sh. 

It is so popular for developers who would love to using terminal other than graphic interface.

There is a project named oh-my-zsh, which contains a lot of well configured profiles for Zsh. Users could beautify the terminal within a minute. This article records how to install and config Zsh in my environment.

## Environment
I am using WSL Ubuntu 18.04 in Win10. After 5 years using Mac, I got a chance back to use Windows again. I'd like to use a Mac, but the price is a little bit higher than what I think a computer should take. And after 2 months using Windows, I think it much better than before.

WSL is short for Windows Subsystm for Linux, which lets user have a pure Linux environment within Windows. Differ than Virtualization solution like VirtualBox, the WSL could be launched in 5 secondes, users could get almost totally same terminal experiences like in Linux or Mac.

Bazinga~~ I am using a Macbook Pro again.

Never mind, ZSH still be my terminal choice with oh-my-zsh.

## Installation

* install zsh
For Mac: `brew install zsh`

* install oh-my-zsh
`curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh; zsh`

* plugins
  After a new plugin installed, add the name of the plugin into `.zshrc` within `plugins=(git)`.

  * zsh-syntax-highlighting 

  `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

  * zsh-autosuggestions

  `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`

  * (fzf)[https://github.com/junegunn/fzf]. Fuzzy Finder in your terminal. 

  `git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && ~/.fzf/install` 

  Answer "y" to all questions. `CTRL + R` and see all your history command. `CTRL + T` can find your file in your terminal.

  * powerlevel10k

  A very geek style theme, make zsh start faster. https://github.com/romkatv/powerlevel10k. Follow the guide to Install `MesloLGS NF font`, change prompt style and so on.



