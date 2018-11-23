# Mac Install Guide

## Table of Contents

- [System Preferences](#system-preferences)
	- [Finder](#finder)
- [Xcode](#xcode)
- [Install Homebrew](#install-homebrew)
- [Install Applications](#install-applications)
	- [Essentials](#essentials)
	- [Recommended](#recommended)
	- [Quick Look Plugins](#quick-look-plugins)
- [Install Developer Packages](#install-developer-packages)
	- [Essential](#essential)
	- [Media](#media)
	- [Misc](#misc)
- [Iterm2](#iterm2)
- [Git Setup](#git-setup)
	- [Setup Your Identity](#setup-your-identity)
	- [Generating a New SSH Key](#generating-a-new-ssh-key)
	- [GitHub Authentication](#github-authentication)
	- [Cache Git Passwords in Keychain](#cache-git-passwords-in-keychain)
	- [Setting up Sublime Text as the Git Mergetool](#setting-up-sublime-text-as-the-git-mergetool)
- [Create Projects Directory](#create-projects-directory)
- [Install ZSH](#install-zsh)
	- [Install Oh My Zsh](#install-oh-my-zsh)
	- [env.sh](#envsh)
	- [Install the Pure Theme of ZSH](#install-the-pure-theme-of-zsh)
	- [Note on Virtual Environments](#note-on-virtual-environments)
	- [Custom ZSH Options](#custom-zsh-options)
	- [The Fuck](#the-fuck)
	- [Custom Aliases](#custom-aliases)
	- [Dev Command](#dev-command)
- [EditorConfig](#editorconfig)
- [Python](#python)
- [Node](#node)
	- [NVM Auto Activation](#nvm-auto-activation)
	- [Yarn](#yarn)
- [MySQL](#mysql)
- [Memcached](#memcached)
- [Redis](#redis)

## System Preferences

#### Trackpad

- Point & Click
	- Enable Tap to click with one finger
- More Gestures
	- Change Swipe between pages to swipe with three fingers
	
#### Security & Privacy

- General
    - Change to require a password immediately after the screen saver appears
    
#### Mission Control

- Unselect 'Automatically rearrange Spaces based on most recent use'

### Finder

- Show status bar (⌘/)
- Toolbar
	- Update to add path, new folder
- Sidebar
	- Add home and code directory
	
## Xcode

Install Xcode from the AppStore.

Install the Xcode command line tools.

```shell
xcode-select --install
```

## Install Applications

### Essentials

```shell
brew cask install diffmerge
brew cask install google-chrome
brew cask install iterm2
brew cask install sequel-pro
brew cask install sourcetree
brew cask install firefox
brew cask install postman
brew cask install skype
brew cask install slack
brew cask install spotify
brew cask install vagrant
brew cask install virtualbox
brew install git
brew install git-flow-avh
brew install ssh-copy-id
brew install wget
brew install imagemagick
brew install ffmpeg
brew install plantuml
```

### Quick Look Plugins

From https://github.com/sindresorhus/quick-look-plugins

```shell
brew cask install qlcolorcode qlstephen qlmarkdown quicklook-json qlprettypatch quicklook-csv betterzipql qlimagesize webpquicklook suspicious-package quicklookase qlvideo
```

## Git Setup

### Setup Your Identity

```shell
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```

### Generating a New SSH Key

You may need to generate a new SSH key and add it to the ssh-agent, see the documentation on [GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-mac).

## Install ZSH

```shell
brew install zsh zsh-completions zsh-syntax-highlighting
```

### Install Oh My Zsh

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Edit your `.zshrc` by opening the file in a text editor and edit the plugins lines with the following:

```
plugins=(git colored-man colorize github jira vagrant virtualenv pip python osx zsh-syntax-highlighting)
```

and add the following lines to the end of the file
 
```
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# Add env.sh
source ~/Projects/config/env.sh
```

### env.sh

Copy the following into `~/Projects/config/env.sh` replacing `YOUR NAME` with your username.


```
#!/bin/zsh

# PATH
export PATH=/usr/local/share/npm/bin:$PATH
export PATH=/usr/local/bin:/usr/local/sbin:$PATH
export EDITOR='subl -w'

# Owner
export USER_NAME="YOUR NAME"
eval "$(rbenv init -)"

# FileSearch
function f() { find . -iname "*$1*" ${@:2} }
function r() { grep "$1" ${@:2} -R . }

#mkdir and cd
function mkcd() { mkdir -p "$@" && cd "$_"; }

# Aliases
alias cppcompile='c++ -std=c++11 -stdlib=libc++'

# Use sublimetext for editing config files
alias zshconfig="subl ~/.zshrc"
alias envconfig="subl ~/Projects/config/env.sh"
```

### Custom ZSH Options

To disable the automatic tab completion behaviour of ZSH add the following to your `~/Projects/config/env.sh` file:

```
# Custom ZSH options
setopt noautomenu
setopt nomenucomplete
```

### The Fuck

[The Fuck](https://github.com/nvbn/thefuck) is an app which corrects your previous console command.

Install it with:

```shell
brew install thefuck
```

Then add the following the end of `~/Projects/config/env.sh`

```
eval "$(thefuck --alias)"
```

### Custom Aliases

Add the following the end of `~/Projects/config/env.sh`.

```
alias flushmc="echo 'flush_all' | nc localhost 11211"
alias flushredis="echo 'FLUSHALL' | nc localhost 6379"
alias flushdns="dscacheutil -flushcache; sudo killall -HUP mDNSResponder"

alias stripsvn="find . -type d -name '.svn' -exec rm -rf {} \;"
alias stripmac="find . -type d -name '__MACOSX' -exec rm -rf {} \;"
alias stripds="find . -type f -name '.DS_Store' -exec rm -rf {} \;"
alias strippyc="find . -type f -name '*.pyc' -exec rm -rf {} \; find . -name '__pycache__' -exec chflags hidden {} \;"
alias fixperms="find . -type f -perm -ugo+x -not -path \"./.git/*\" -print -exec chmod -x {} \;"

alias edithosts="subl /usr/local/etc/apache2/2.4/extra/httpd-vhosts.conf /etc/hosts"

alias fixopenwith="/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local -domain system -domain user; killall Finder"
```

### Dev Command

Add the following the end of `~/Projects/config/env.sh`.

```
export DEV_HOME="$HOME/Projects"
dev () {
    typeset dir="$1"
    cd $DEV_HOME

    # deactivate old virtual env
    if [[ "$VIRTUAL_ENV" != "" ]] ; then
        deactivate
    fi

    if [ ! "$dir" = "" ] ; then
        cd $dir

        # activate the virtualenv
        if [ -d "$WORKON_HOME/$dir" ] ; then
            workon "$dir"
        fi
    fi
}

#
# Set up tab completion.  (Adapted from Arthur Koziel's version at
# http://arthurkoziel.com/2008/10/11/virtualenvwrapper-bash-completion/)
#
cddev_show_options () {
    # NOTE: DO NOT use ls here because colorized versions spew control characters
    #       into the output list.
    # echo seems a little faster than find, even with -depth 3.
    # (for f in `ls -l $DEV| egrep '^d' | awk '{ print $9 }'`; do echo $f; done) 2>/dev/null | \sed 's|^\./||' | \sed 's|/bin/activate||' | \sort | (unset GREP_OPTIONS; \egrep -v '^\*$')
    (cd "$DEV_HOME"; for f in */; do echo $f; done) 2>/dev/null | \sed 's|^\./||' | \sed 's|/||' | \sort | (unset GREP_OPTIONS; \egrep -v '^\*$')
}
if [ -n "$BASH" ] ; then
    _cddev_dirs () {
        local cur="${COMP_WORDS[COMP_CWORD]}"
        COMPREPLY=( $(compgen -W "`cddev_show_options`" -- ${cur}) )
    }
    complete -o default -o nospace -F _cddev_dirs dev
elif [ -n "$ZSH_VERSION" ] ; then
    compctl -g "`cddev_show_options`" dev
fi
```

The `dev` command then be used to switch between projects, automatically activating their python virtual environments, e.g. `dev project1`.


## EditorConfig

To have a standard in case a project does not include it's own EditorConfig file create a `.editorconfig` file in your home directory with the following contents:

```
# http://editorconfig.org
root = true

[*]
indent_style = space
indent_size = 4
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
indent_size = 4
indent_style = tab

[*.js]
indent_size = 2

[*.json]
indent_size = 2

[*.html]
indent_style = tab

[*.yml]
indent_size = 2
```

## Python

We will use pyenv to install multiple versions of Python, this is preferable to Homebrew as it allows full control over minor versions.

```shell
brew install python pyenv
```

After installing, add the following to `~/Projects/config/env.sh`

```
# Python
export PYTHONIOENCODING=utf-8
eval "$(pyenv init -)"
```

Restart your shell so the path changes take effect. You can now begin using pyenv.

```shell
exec $SHELL
```

To list the all available versions of Python, including Anaconda, Jython, pypy, and stackless, use:

```shell
pyenv install --list
```

Then install the desired versions:

```shell
pyenv install 2.7.13
pyenv install 3.5.3
```

Use the `global` command to set global version(s) of Python to be used in all shells. For example, if you prefer 2.7.13 over 3.5.3:

```shell
pyenv global 3.5.3 2.7.13
pyenv rehash
```

The leading version takes priority. All installed Python versions can be located in `~/.pyenv/versions`. You can run `pyenv versions` to see the list of all the Python versions available, an asterisk `*` is displayed next to the currently active version.

#### Homebrew Warnings

The use of pyenv causes [brew to output warning](https://github.com/pyenv/pyenv/issues/106), to resolve this we use [randy3k's solution of creating an alias](https://github.com/pyenv/pyenv/issues/106#issuecomment-94921352).

Add the following to the bottom of your `~/Projects/config/env.sh`:

```
alias brew="env PATH=${PATH//$(pyenv root)\/shims:/} brew"
```


#### Virtualenv

As we're using `pyenv` we will use `pyenv-virtualenvwrapper`, install it with the following.

```shell
brew install pyenv-virtualenvwrapper
```

Make sure `virtualenvwrapper` is installed.

```shell
pip install virtualenvwrapper
```

Add the following to your `~/Projects/config/env.sh`:

```
export PYENV_VIRTUALENVWRAPPER_PREFER_PYVENV="true"
pyenv virtualenvwrapper
```

After restarting your shell you will be able to create virtual environments using the `mkvirtualenv` command.


## Node

Install NVM so that multiple versions of Node can be managed, to do this run the latest install script from [the README](https://github.com/creationix/nvm#install-script).

This install script automatically adds the nvm source strings to the bottom `.zshrc` which is in the wrong place for us as we need all the commands to execute in the right order. To correct this remove the following lines from your   `.zshrc` file.

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Add them to the to bottom of your `~/Projects/config/env.sh` file:

```
# Node
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Reload your shell

```shell
source ~/.zshrc
```

Check the nvm is installed, the following should output 'nvm' if the installation was successful.

```shell
command -v nvm
```

List all the node versions you can install

```shell
nvm ls-remote
```

Install most recent nodejs LTS version

```shell
nvm install --lts
```

List installed node version

```shell
nvm ls
```

Use latest LTS as current version

```shell
nvm use lts/*
```

Set the installed LTS version as the default node 

```shell
nvm alias default lts/*
```


### NVM Auto Activation

Add the following to your `~/Projects/config/env.sh` file so that `nvm use` will automatically be called whenever you enter a directory that contains an `.nvmrc` file with a string telling nvm which node to use. [See here](https://github.com/creationix/nvm#calling-nvm-use-automatically-in-a-directory-with-a-nvmrc-file).

```
# NVM auto activation
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

### Yarn

Install the Yarn package manager:

```shell
brew install yarn
```

## MySQL

Install:

```shell
brew install mysql
```

To have launchd start MySQL now and restart at login:

```shell
brew services start mysql
```

## Memcached

Install:

```shell
brew install memcached
```

To have launchd start memcached now and restart at login:

```shell
brew services start memcached
```

## Redis

Install:

```shell
brew install redis
```

To have launchd start redis now and restart at login:

```shell
brew services start redis
```

Install [Homebrew](https://brew.sh/) and [Cask](https://github.com/Homebrew/homebrew-cask)

Clone this repo
```sh
mkdir projects
cd projects
git clone https://github.com/thanhtoan1196/mac-setup.git && cd mac-setup
```

Install casks

```sh
while read app; do
  brew cask install "$app"
done < homebrew-cask/basic.txt
```



```sh
# Remove downloaded files and cleanup space
brew cask cleanup
```



Install Mac App Store apps

```sh
brew install mas # cli for the mac app store
```

```sh
app_ids_without_comments=$(grep -o '^[^#]*' mas/basic.txt | grep -v '^$')
echo $app_ids_without_comments | xargs mas install
```



Development



Install homebrews

```sh
while read tool; do
  brew install "$tool"
done < homebrew/dev.txt
```



Install casks

```sh
while read app; do
  brew cask install "$app"
done < homebrew-cask/dev.txt
```

Install [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh)

Thêm một stack tùy biến trên dock để chứa tập tin, ứng dụng vừa mở  
`defaults write com.apple.dock persistent-others -array-add '{"tile-data" = {"list-type" = 1;}; "tile-type" = "recents-tile";}'; killall Dock`

Thêm khoảng trắng trên dock  
`defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}'; killall Dock`

Reset thanh dock lại mặc định ban đầu  
`defaults delete com.apple.dock; killall Dock`

Làm quen với các phím tắt trên Mac với phần mềm CheatSheet
