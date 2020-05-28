---
layout: post
img:
categories: JavaScript
title: How to config your Mac Terminal and Git status with colorful
description: |
date: 2019-10-04 18:00
permalink: archivers/blog4
---

## How to config your Mac Terminal and Git status with colorful

### Firstly, we should copy below code into your `.bash_profile` file.
- If this source file not exist, we can use `touch ~/.bash_profile` command create it into your home path.

### Secondly, if we want to make terminal full support git command with intelligent. we can download the git-completion and git-prompt file.

> Git Link: https://github.com/git/git/releases (download the source code with zip package.)

### Lastly, we shoul unzip the package and copy the file into your home directory. Then, we also need to rename the file.

```bash
copy ~/git-2.23.0/contrib/completion
```

> git-2.23.0/contrib/completion

```bash
alias ll='ls -lahFGp'

# config ls color for folder and file
export CLICOLOR=1
export LC_ALL=en_US.UTF-8
# if others, we can use this also.
# export LSCOLORS=GxFxCxDxBxegedabagaced

# if your terminal theme is PRO, we can use below config code.
export LSCOLORS=gxBxhxDxfxhxhxhxhxcxcx

# copy contrib/copmletion/git-completion.bash to your home directory and source it
# Linux users should add the line below to your .bashrc
. ~/.git-completion.bash

#copy contrib/completion/git-prompt.sh to your home directory and source it
#Linux users should add the line below to your .bashrc
. ~/.git-prompt.sh

# COLOR_RED="\033[0;31m"
# COLOR_YELLOW="\033[0;33m"
# COLOR_GREEN="\033[0;32m"
# COLOR_OCHRE="\033[38;5;95m"
# COLOR_BLUE="\033[0;34m"
# COLOR_WHITE="\033[0;37m"
# COLOR_RESET="\033[0m"

DEFAULT="\[\033[0;00m\]"
GREEN="\[\033[0;32m\]"
YELLOW="\[\033[0;33m\]"
CYAN="\[\033[0;36m\]"

export GIT_PS1_SHOWDIRTYSTATE=1
# export PS1='\w$(__git_ps1 " (%s)")\$'
# export PS1='\u:\W$(__git_ps1 " (%s)")\$ '
export PS1=$GREEN"\u@\h"$YELLOW" \w"$CYAN'$(__git_ps1 " (%s)")'$DEFAULT"\n\$ "
```