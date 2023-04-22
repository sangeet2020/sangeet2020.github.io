---
layout: post
title:  Ideal bashrc
date:   2022-06-01 16:40:16
description: An ideal bashrc is a myth.
tags: formatting links
categories: tech-post
---

## Setting up your bashrc
This post will discuss several features that are useful to have in your bashrc. These include:
- Creating a log of local history, which records all commands entered in the terminal and stores them in a file named `.history-sagar`.
- Defining useful aliases for commands to make them easier to type and remember.
- Adding attractive colors to the prompt for better readability and aesthetic appeal.
- Enabling color support for the ls command to make it easier to distinguish different file types.
- Adding the path to locally installed packages without the need for sudo permissions.

### Keeping a Log of Local History
- First install `gawk`
```bash
apt-get -y install gawk
```
- Next insert this into your `.bashrc`

    ```bash
    GAWK_PATH="/usr/bin/"
    myLocalHistory() {
    if test -w .
    then
        history 1 | ${GAWK_PATH}/gawk '($2 !~ "^[mr]?cd[0-9a-z]?$") {$1="_T="strftime("%Y%m%d_%H:%M:%S_") PROCINFO["ppid"] "\t"; $2=gensub("^_T=[-_0-9:]*[ \t]* *", "", 1, $2); $2=gensub("^_P=[^ \t]* *", "", 1, $2); print;}' >> .history-$LOGNAME
    else
        history 1 | ${GAWK_PATH}/gawk '($2 !~ "^[mr]?cd[0-9a-z]?$") {$1="_T="strftime("%Y%m%d_%H:%M:%S_") PROCINFO["ppid"] "_PWD="  ENVIRON["PWD"] "\t"; $2=gensub("^_T=[-_0-9:]*[ \t]* *", "", 1, $2); $2=gensub("^_P=[^ \t]* *", "", 1, $2); print;}' >> ~/.history-all-$LOGNAME
    fi
    }

    export PROMPT_COMMAND="myLocalHistory 2> /dev/null"


    grepLocalHistory() {
    grep "$1" .history-$LOGNAME
    }
    alias h=grepLocalHistory

    ```
    Now everytime your enter `h` in terminal you get list of all commands entered along with time-stamps.

### Defining Useful Aliases
```bash
alias rm='rm -i'
alias lsn='ls'
alias ls='ls --color'
alias ll='ls -hltr'
alias ltr='ls -ltr'
alias more='less'
alias cds='cd /home/sangeet'
alias cdn='cd /home/sangeet/sagar'
alias cdw='cd /mnt/New-Volume/work'
alias sb='source /home/sangeet/sagar/thesis/sb_env/bin/activate'
alias bashrc='vim ~/.bashrc'
alias lsd='ls -d */' # list only dirs
alias rmr='rm -rf *.out *.err *.logs'
alias start_int_gpu='/home/sagar/bin/start_int_gpu.sh $1 $2 $3'

alias condalist="conda info -e"
alias qgpu="srun --pty --mem=16gb -n 1 --gres=gpu:1 /bin/bash"
alias nsmi="watch -n 1 nvidia-smi"
alias nvsmi="watch --color -n 1 nvidia-smi"
alias nsmil='nvidia-smi'
alias stat='squeue -u sagar -o "%.6i %9P %15j %.6u %.10T %.10M %.10l %.6D %10R %s %.10g"'
```

### Adding Attractive Prompt Coloring
```bash
LS_COLORS=$LS_COLORS:'di=1;35:' ; 
export LS_COLORS
export LS_COLORS=$LS_COLORS:"*.py=0;32":"*.txt=0;36"
export LS_COLORS=$LS_COLORS:"*.sh=0;32":"*.wav=0;33"
export LS_COLORS='di=1;36:fi=0:ln=1;35:pi=5;33:so=5;35:bd=5;37:cd=5;37:or=31;47;1:ex=1;32'
```

### Language support
```bash
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
export LANGUAGE=en_US.UTF-8
```

### Enabling Color Support for ls
```bash
#LS_COLORS=$LS_COLORS:'di=1;35:' ; export LS_COLORS
#export LS_COLORS=$LS_COLORS:"*.py=0;32":"*.txt=0;36"
#export LS_COLORS=$LS_COLORS:"*.sh=0;32":"*.wav=0;33"
#export LS_COLORS='di=1;36:fi=0:ln=1;35:pi=5;33:so=5;35:bd=5;37:cd=5;37:or=31;47;1:ex=1;32'
LS_COLORS=$LS_COLORS:'di=1;35:' ; export LS_COLORS
export LS_COLORS=$LS_COLORS:"*.py=0;32":"*.txt=0;36"
export LS_COLORS=$LS_COLORS:"*.sh=0;32":"*.wav=0;33"
```

### Adding Path to Locally Installed Packages without Sudo

```bash
export PATH=$PATH:"/home/sagar/.localpython/bin"
export PATH=$PATH:"/netscratch/sagar/usr/share/yasm/bin":
```