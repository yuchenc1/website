---
title: Zsh Config
date: 2019-08-26 19:44:43
tags: 
- Engineering
- Zsh
- Config
categories:
- Engineering
---

Life is short! Use Zsh!
First check if your system has zsh and what shell is your system is using.

``` bash
$ cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
$ echo $SHELL
/bin/bash
```

If your system has zsh already, we are fine. If not, you shall go to install zsh first.
https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH

``` bash
$ chsh -s /bin/zsh
```
Then you can logout and open a new terminal.

``` bash
$ echo $SHELL
/bin/zsh
```

Next, we are going to install oh-my-zsh. Check it on the github README, Basic Installation.
https://github.com/robbyrussell/oh-my-zsh

After install oh-my-zsh, we need to change the config  `~/.zshrc` to make your zsh more powerful. I feel the most useful is auto-completion. Here is my personal zsh config.

``` bash
# Antigen: https://github.com/zsh-users/antigen
ANTIGEN="$HOME/.local/bin/antigen.zsh"

# Install antigen.zsh if not exist
if [ ! -f "$ANTIGEN" ]; then
	echo "Installing antigen ..."
	[ ! -d "$HOME/.local" ] && mkdir -p "$HOME/.local" 2> /dev/null
	[ ! -d "$HOME/.local/bin" ] && mkdir -p "$HOME/.local/bin" 2> /dev/null
	[ ! -f "$HOME/.z" ] && touch "$HOME/.z"
	URL="http://git.io/antigen"
	TMPFILE="/tmp/antigen.zsh"
	if [ -x "$(which curl)" ]; then
		curl -L "$URL" -o "$TMPFILE" 
	elif [ -x "$(which wget)" ]; then
		wget "$URL" -O "$TMPFILE" 
	else
		echo "ERROR: please install curl or wget before installation !!"
		exit
	fi
	if [ ! $? -eq 0 ]; then
		echo ""
		echo "ERROR: downloading antigen.zsh ($URL) failed !!"
		exit
	fi;
	echo "move $TMPFILE to $ANTIGEN"
	mv "$TMPFILE" "$ANTIGEN"
fi


# Initialize command prompt
export PS1="%n@%m:%~%# "

# Initialize antigen
source "$ANTIGEN"

# Load local bash/zsh compatible settings
[ -f "$HOME/.local/etc/init.sh" ] && source "$HOME/.local/etc/init.sh"


# Initialize oh-my-zsh
antigen use oh-my-zsh


# default bundles
# visit https://github.com/unixorn/awesome-zsh-plugins
antigen bundle git
antigen bundle heroku
antigen bundle pip
antigen bundle svn-fast-info
# antigen bundle command-not-find

antigen bundle colorize
antigen bundle github
antigen bundle python
antigen bundle z

antigen bundle zsh-users/zsh-autosuggestions
antigen bundle zsh-users/zsh-completions
antigen bundle supercrabtree/k
antigen bundle Vifon/deer

# uncomment the line below to enable theme
antigen theme avit


# check login shell
if [[ -o login ]]; then
	[ -f "$HOME/.local/etc/login.sh" ] && source "$HOME/.local/etc/login.sh"
	[ -f "$HOME/.local/etc/login.zsh" ] && source "$HOME/.local/etc/login.zsh"
fi

# syntax color definition
ZSH_HIGHLIGHT_HIGHLIGHTERS=(main brackets pattern)

typeset -A ZSH_HIGHLIGHT_STYLES

# ZSH_HIGHLIGHT_STYLES[command]=fg=white,bold
# ZSH_HIGHLIGHT_STYLES[alias]='fg=magenta,bold'

ZSH_HIGHLIGHT_STYLES[default]=none
ZSH_HIGHLIGHT_STYLES[unknown-token]=fg=009
ZSH_HIGHLIGHT_STYLES[reserved-word]=fg=009,standout
ZSH_HIGHLIGHT_STYLES[alias]=fg=cyan,bold
ZSH_HIGHLIGHT_STYLES[builtin]=fg=cyan,bold
ZSH_HIGHLIGHT_STYLES[function]=fg=cyan,bold
ZSH_HIGHLIGHT_STYLES[command]=fg=white,bold
ZSH_HIGHLIGHT_STYLES[precommand]=fg=white,underline
ZSH_HIGHLIGHT_STYLES[commandseparator]=none
ZSH_HIGHLIGHT_STYLES[hashed-command]=fg=009
ZSH_HIGHLIGHT_STYLES[path]=fg=214,underline
ZSH_HIGHLIGHT_STYLES[globbing]=fg=063
ZSH_HIGHLIGHT_STYLES[history-expansion]=fg=white,underline
ZSH_HIGHLIGHT_STYLES[single-hyphen-option]=none
ZSH_HIGHLIGHT_STYLES[double-hyphen-option]=none
ZSH_HIGHLIGHT_STYLES[back-quoted-argument]=none
ZSH_HIGHLIGHT_STYLES[single-quoted-argument]=fg=063
ZSH_HIGHLIGHT_STYLES[double-quoted-argument]=fg=063
ZSH_HIGHLIGHT_STYLES[dollar-double-quoted-argument]=fg=009
ZSH_HIGHLIGHT_STYLES[back-double-quoted-argument]=fg=009
ZSH_HIGHLIGHT_STYLES[assign]=none

# load local config
[ -f "$HOME/.local/etc/config.zsh" ] && source "$HOME/.local/etc/config.zsh" 
[ -f "$HOME/.local/etc/local.zsh" ] && source "$HOME/.local/etc/local.zsh"

# enable syntax highlighting
antigen bundle zsh-users/zsh-syntax-highlighting

antigen apply

# setup for deer
autoload -U deer
zle -N deer

# default keymap
bindkey -s '\ee' 'vim\n'
bindkey '\eh' backward-char
bindkey '\el' forward-char
bindkey '\ej' down-line-or-history
bindkey '\ek' up-line-or-history
bindkey '\eu' undo
bindkey '\eH' backward-word
bindkey '\eL' forward-word
bindkey '\eJ' beginning-of-line
bindkey '\eK' end-of-line

bindkey -s '\eo' 'cd ..\n'
bindkey -s '\e;' 'lk\n'

bindkey '\e[1;3D' backward-word
bindkey '\e[1;3C' forward-word
bindkey '\e[1;3A' beginning-of-line
bindkey '\e[1;3B' end-of-line

bindkey '\ev' deer

alias lk='k --no-vcs'

# alias atom='/Applications/Atom.app/Contents/MacOS/Atom'
# alias subl='/Applications/SublimeText.app/Contents/SharedSupport/bin/subl'
# alias code='/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code'
```

You can directly copy it and use it. Also if you have some environment variables settings in your previous shell, for example bash. You may look at your bash config file, such as `~/.bash_profile` or `~/.bashrc` and copy them into your zsh config. You may feel it very convenient to directly add a source `~/.bash_profile` in your zsh config, but I suggest you do not do this.

***

Here is a problem I meet in my work. My work VM lost a lot of environment variables such as $JAVA_HOME when I switch to zsh, this is very strange because this environment variable is configured by IT and it should be automatically loaded despite which shell I am using.
I began searching and found it lives in a script in `/etc/profile.d/`.
If you look at `/etc/profile`, it will run the script in `/etc/profile.d/` to load all the personal setups.

``` bash
for i in /etc/profile.d/*.sh ; do
    if [ -r "$i" ]; then
        if [ "${-#*i}" != "$-" ]; then
            . "$i"
        else
            . "$i" >/dev/null 2>&1
        fi
    fi
done
```

When you login, `/etc/profile` should be started at first. However, zsh is unique, it will not load this. However, my colleague has never met this problem before. So we began searching why this happened. There must be a script zsh will run the scripts under `/etc/profile.d`.
After hours, we found the key problem. In `/etc/zshrc`, his file contains

``` bash
_src_etc_profile_d()
{
    #  Make the *.sh things happier, and have possible ~/.zshenv options like
    # NOMATCH ignored.
    emulate -L ksh


    # from bashrc, with zsh fixes
    if [[ ! -o login ]]; then # We're not a login shell
        for i in /etc/profile.d/*.sh; do
            if [ -r "$i" ]; then
                . $i
            fi
        done
        unset i
    fi
}
_src_etc_profile_d
```

However, my `/etc/zshrc` does not have this! Why?
Because my company is still using Centos 6. And I used 

``` bash
$ yum -y install zsh
```

Its version still begins with 4. Now the zsh version would begin with 5. After I downloaded the zsh with version 5, I could find `/etc/zshrc` has the function to load the scripts under `/etc/profile` and my problem was solved.

Life is short! Why use Centos 6!
