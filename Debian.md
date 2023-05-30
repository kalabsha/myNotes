# 安装前的准备

1. 下载 [firmware-11.6.0-amd64-netinst.iso](http://mirrors.ustc.edu.cn/debian-cdimage/unofficial/non-free/images-including-firmware/current/amd64/iso-cd/firmware-11.6.0-amd64-netinst.iso) 镜像文件
1. 制作 U 盘安装盘
    ```bash
    $ sudo dd if=~/Downloads/firmware-11.6.0-amd64-netinst.iso of=/dev/sdb status=progress
    ```

# 安装时的注意事项

1. 不设置 `root` 密码，则会安装 `sudo`，但要注意：安装完成进入系统后，记得设置 `root` 密码，否则如果系统故障，无法通过 **Recovery** 模式进行修复。

# 安装完成后

## 初始化配置

1. 网络未配置时，如果需要安装软件，可以使用 CDROM 的形式
    ```bash
    $ sudo apt-cdrom add # 在提示等待挂载 /media/cdrom 时，切到另外的窗口执行如下命令，然后再回来按下回车
    $ sudo mount /path/of/iso-file /media/cdrom
    ```
1. Console 字体设置：
    - `/etc/default/console-setup`
    - `/usr/share/console-setup/console-setup`

    ```
    $ cat /etc/default/console-setup

    # CONFIGURATION FILE FOR SETUPCON

    # Consult the console-setup(5) manual page.

    ACTIVE_CONSOLES="/dev/tty[1-6]"

    CHARMAP="UTF-8"

    CODESET="Uni3"
    FONTFACE="Terminus"
    FONTSIZE="28x14"

    VIDEOMODE=

    # The following is an example how to use a braille font
    # FONT='lat9w-08.psf.gz brl-8x8.psf'
    #
    #KEYMAP=us
    #FONT=Uni3-Terminus28x14
    ```

1. 安装软件：
    <!-- 网络 -->
    - `wpasupplicant resovlconf`
    <!-- 系统 -->
    - `ansible git openssh-server p7zip-full rsync tmux tree vim`
    <!-- 桌面 -->
    - `bspwm feh lightdm light-locker polybar rofi tilix`
    <!-- 输入法 -->
    - `ibus-rime librime-data-wubi`
    <!-- 字体 -->
    - `fonts-font-awesome fonts-noto-cjk`
    <!-- LFS -->
    - `bison build-essential gawk m4 texinfo`
    <!-- BLFS -->
    - `xml-core libxml2-utils xsltproc docbook-xml docbook-xsl`
    <!-- 系统性能分析、调优 -->
    - `apache2-utils bcc-tools build-essential curl fio hping3 iperf3 linux-perf linux-tools nmap procps strace sysstat systemtap systemtap-runtime tcpdump trace-cmd wireshark`


## 网络

1. 修复 iwlwifi BUG 导致的无法识别无线网卡问题
    ```
    $ sudo vi /etc/modprobe.d/iwlwifi.conf
    options iwlwifi enable_ini=N
    ```

1. 如果是离线安装，安装时网络未配置，需要安装 `resovleonf`

1. systemd network
    - link
        - eth0
        - wlan0
    - network
        - eth0
        - wlan0
        - vboxnet0
    ```bash
    $ cat /etc/systemd/network/10-wifi-dhcp.network 
    [Match]
    Name=wlan0

    [Network]
    DHCP=ipv4
    MulticastDNS=no

    [DHCP]
    UseDomains=false
    UseRoutes=true

    $ cat /etc/systemd/network/20-eth-dhcp.network 
    [Match]
    Name=eth0

    [Network]
    DHCP=ipv4
    MulticastDNS=no

    [DHCP]
    UseDomains=false
    UseRoutes=true

    $ cat /etc/systemd/network/30-vboxnet0.network 
    [Match]
    Name=vboxnet0

    [Network]
    Address=192.168.56.1
    MulticastDNS=yes
    DNS=192.168.56.1
    ```

1.  使用 wpa_supplicant 管理无线网

    - `wpa_supplicant.conf`
    ```
    $ cat /etc/wpa_supplicant/wpa_supplicant-wlan0.conf
    # allow frontend (e.g., wpa_cli) to be used by all users in 'netdev' group
    # ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    #
    # home network; allow all valid ciphers
    network={                          
     ssid="www.nslib.cn"              
     key_mgmt=NONE                              
     priority=-999                              
    }
    ```
    - `sudo systemctl status wpa_supplicant@wlan0.service`
