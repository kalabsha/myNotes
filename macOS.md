2020-10-10: refactor

本文记录笔者的 macOS 系统配置及常用软件
0. 使用 TimeMachine 进行备份。加速小技巧：`sudo sysctl debug.lowpri_throttle_enabled=0`
1. 重装系统，恢复到电脑自带版本，如： macOS Sierra (10.12)，请谨慎选择，编者在初始化过程中发现很多软件已经不支持此版本，于是重新升级到了 macOS Big Sur（11.4）。
>
在基于 Intel 的 Mac 上：如果您在启动过程中使用 Shift-Option-Command-R，您会获得 Mac 自带的 macOS 或最接近且仍在提供的版本。如果您在启动过程中使用 Option-Command-R，则在大多数情况下，您会获得与您的 Mac 兼容的最新版 macOS。否则，您会获得 Mac 自带的 macOS 或最接近且仍在提供的版本。

1. 重命名主机名：~~**System Preferences -> Sharing -> Computer Name: macbook**~~ 使用 Migration Assistant 仅恢复 Time Machine 的网络配置即可
1. 设置触发角： 
    - **System Preferences -> Mission Control -> Hot Corners -> 左上角： Start Screen Saver **
    - **System Preferences -> Security & Privacy -> General -> Require password: immediately **
1. 设置屏幕保护：**System Preferences -> Desktop & Screen Saver： Word of the Day | √ Show with clock **
1. 交换 Capital 和 Control 键：**System Preferences -> Keyboard -> Modifier Keys -> Caps Lock Key: Control -> Control Key: Caps Lock**
1. 调整键盘响应速度： **System Preferences -> Keyboard -> Key Repeat: Fast -> Delay Until Repeat: Short**
1. 设置轻点触控板： **System Preferences -> Trackpad -> Tap to click √**
1. 取消 Spotlight： **System Preferences -> Keyboard -> Shortcuts -> Spotlight -> 取消勾选 Show Spotlight Search**
1. macOS 小技巧：`Command + Shift + .` 可以显示/隐藏 被隐藏的文件或者目录
1. macOS 小技巧：`Shift + Command + V` 可以取消要粘贴的文本格式，仅保留纯文本
1. macOS 小技巧：自带录屏工具 `Command + Shift + 5`, 在下方选择录屏，即可使用 QuickTime Player 进行屏幕录制。
1. 输入法切换快捷键改为 `Command + Space` / `Control + Command + Space`： **System Preferences -> Keyboard -> Shortcuts -> Input Source**
1. 配置 sudo 免密码，使用 vim 编辑完成后执行 `:x!` 强制保存并退出： 
    ```bash
    # sudo vim /etc/sudoers
    root ALL=(ALL) ALL
    %admin ALL=(ALL) NOPASSWD: ALL
    ```
1. [Homebrew](https://mirrors.ustc.edu.cn/help/brew.git.html)
    ```bash
    export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
    export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
    export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"

    /bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/Homebrew/install@HEAD/install.sh)"

    # 使用中科大的 cask 源
    brew tap --custom-remote --force-auto-update homebrew/cask https://mirrors.ustc.edu.cn/homebrew-cask.git
    brew tap --custom-remote --force-auto-update homebrew/cask-versions https://mirrors.ustc.edu.cn/homebrew-cask-versions.git
    ```
1. 使用 [brew bundle dump](https://docs.brew.sh/Manpage#bundle-subcommand) 生成已安装软件列表
    ```bash
    # cd ~/Backup/brew && brew bundle dump
    ```
1. 使用 [brew bundle](https://docs.brew.sh/Manpage#bundle-subcommand) 恢复软件安装
    ```bash
    # cd ~/Backup/brew && brew bundle [install]
    ```
1. [KeePassXC](https://keepassxc.org/) 
    ```bash
    # 最好使用 brew bundle 恢复安装，如需手动安装，执行如下命令
    brew install --cask keepassxc
    # 备份的账号密码文件位于 ~/Backup/KeePassDB
    ```
1. [Alfred 3](https://cachefly.alfredapp.com/Alfred_3.8.6_972.dmg)
    > 密钥可查阅 KeePassXC

1. [iTerm](https://iterm2.com/downloads.html) 
    ```bash
    # 最好使用 brew bundle 恢复安装，如需手动安装，执行如下命令
    brew install --cask iterm2
    ```
1. Zsh (系统自带) + [Oh My Zsh](https://ohmyz.sh/) + [my.zsh-theme](https://github.com/bougenville/zsh-theme)
1. [Vimrc](https://github.com/bougenville/vimprofile)
1. [中文输入法 - 鼠须管](https://rime.im/)：macOS Big Sur 自带了中文五笔输入法，但是不好用，不支持通过 Shift 键切换成英文、自定义组词等。下载安装完成后，需要退出、重新登录。[我的 Rime 配置](https://github.com/bougenville/Rime)
    ```bash
    # 最好使用 brew bundle 恢复安装，如需手动安装，执行如下命令
    brew install --cask squirrel
    # 备份文件位于 ~/Backup/RimeSync
    ```
1. SSH configs
1. [Docker Desktop](http://docker.com/)
    1. 资源分配：4 核 8 G
    2. Docker Engine (`/etc/docker/daemon.json`)
        ```bash
              "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/"]
        ```
