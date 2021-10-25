---
# layout: post
title: Tricks for using Mac OS X
---

## System wide variables setting (including GUI application)
`/etc/launchd.conf` is the configuration file for setting system wide variables. Following is an example.

~~~bash
setenv JAVA_VERSION 1.6
setenv JAVA_HOME /System/Library/Frameworks/JavaVM.framework/Versions/1.6/Home
setenv GROOVY_HOME /Applications/Dev/groovy
setenv GRAILS_HOME /Applications/Dev/grails
setenv NEXUS_HOME /Applications/Dev/nexus/nexus-webapp
setenv JRUBY_HOME /Applications/Dev/jruby

setenv ANT_HOME /Applications/Dev/apache-ant
setenv ANT_OPTS -Xmx512M

setenv MAVEN_OPTS "-Xmx1024M -XX:MaxPermSize=512m"
setenv M2_HOME /Applications/Dev/apache-maven
~~~

> [Use ZSH instead](//2020/01/12/tricks-for-using-zsh.html)

## Bash setting
`~/.bash_profile` will be executed for every time an Terminal get started for the specific user.  
`/etc/profile` for all users.

~~~bash
export CLICOLOR=1
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

alias ll="ls -l"

export JAVA_HOME=
export M2_HOME=
~~~

> [Use ZSH instead](//2020/01/12/tricks-for-using-zsh.html)

## Useful command lines

### HOME and END key binding
Enable HOME and END key working like normal.
```
sudo bash 
mkdir -p ~/Library/KeyBindings ; cd ~/Library/KeyBindings
vim DefaultKeyBinding.dict
```

Content of the DefaultKeyBinding.dict
```
{
/* Remap Home / End keys */
/* Home Button*/
"\UF729" = "moveToBeginningOfLine:"; 
/* End Button */
"\UF72B" = "moveToEndOfLine:"; 
/* Shift + Home Button */
"$\UF729" = "moveToBeginningOfLineAndModifySelection:"; 
/* Shift + End Button */
"$\UF72B" = "moveToEndOfLineAndModifySelection:"; 
/* Ctrl + Home Button */
"^\UF729" = "moveToBeginningOfDocument:"; 
/* Ctrl + End Button */
"^\UF72B" = "moveToEndOfDocument:"; 
 /* Shift + Ctrl + Home Button */
"$^\UF729" = "moveToBeginningOfDocumentAndModifySelection:";
/* Shift + Ctrl + End Button*/
"$^\UF72B" = "moveToEndOfDocumentAndModifySelection:"; 
}
```
> Restart required.

### Finder
Display full path on the top of the Finder window or cancel this function.

~~~bash
defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE; killall Finder # enable
defaults delete com.apple.finder _FXShowPosixPathInTitle; killall Finder # disable
~~~

### Security & Privacy
Allow opening application from any sources. Some of the cracked application downloaded from third-party website, Mac OS will prevent opening it. After running following command, you can change this policy by choosing "System Properties" -> "Security & privacy" -> "General" -> "Anywhere".

~~~bash
sudo spctl --master-disable
~~~

## Userful software
- Itsycal: a smart calendar
- IINA: a excellent video player
- iShot: a full feature screenshot tool
- CopyClip: a paste manager tool

