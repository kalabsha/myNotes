# 我的 VirtualBox 使用经验

## 安装系统及配置

1. 使用 BT 软件（Transmission）或者 Jigdo[^jigdo] 下载 Debian 系统的镜像 iso 文件，
    - [官网](https://www.debian.org/CD/jigdo-cd/)
    - 国内源站：
        - [阿里云](https://mirrors.aliyun.com/debian-cd/current/amd64/jigdo-cd/)
        - [DLBD 版](https://mirrors.aliyun.com/debian-cd/current/amd64/jigdo-dlbd)

[^jigdo]:笔者近来倾向于使用 [Jigdo](http://atterer.org/jigdo/) ~~和 [Debian Testing](https://mirrors.ustc.edu.cn/debian-cdimage/weekly-builds/amd64/jigdo-dlbd/) 版本~~

2. 创建虚拟机：
    - 挂载全部两个 DLBD 虚拟光盘（最多可挂载 4 块虚拟光盘）
    - 适当调整 CPU 核数、内存大小、磁盘空间大小等参数
    - 添加两张网卡：
        - NAT
        - Hostonly（需要先手动创建一块 Hostonly 网卡：Network -> Create）

<!-- ![virtualbox](../images/virtualbox.png) -->

3. 安装好系统后，启用光盘作为软件安装源： `# apt-cdrom`

1. 安装系统时不设置 root 密码，则会自动安装 sudo，否则需要登入系统后自行安装

1. 更改默认编辑器

    ```bash
    $ sudo update-alternatives --config editor 
    There are 4 choices for the alternative editor (providing /usr/bin/editor).

    Selection    Path                Priority   Status
    ------------------------------------------------------------
    * 0            /bin/nano            40        auto mode
    1            /bin/nano            40        manual mode
    2            /usr/bin/code        0         manual mode
    3            /usr/bin/vim.basic   30        manual mode
    4            /usr/bin/vim.tiny    15        manual mode

    Press <enter> to keep the current choice[*], or type selection number: 
    ```

3. 配置 sudo 免密码，控制台 root 免密码登录（此举存在安全隐患，仅为方便在本地实验）

    ```bash
    $ sudo vim /etc/sudoers
    ...
    %sudo   ALL=(ALL:ALL) NOPASSWD: ALL
    ...

    $ head -n 1 /etc/shadow
    root::19489:0:99999:7:::

    # 若安装系统时不设置 root 密码，则 /etc/shadow 中本条记录则会类似如下所示：
    # root:!:19489:0:99999:7:::

    ```

4. 配置网络（systemd-networkd.service），并启用 Bonjour 协议（systemd-resolved.service）：

    ```bash
    % cat /etc/systemd/network/10-eth-dhcp.network
    [Match]
    Name=enp0s3

    [Network]
    DHCP=ipv4
    MulticastDNS=no

    [DHCP]
    UseDomains=false
    UseRoutes=true

    % cat /etc/systemd/network/20-eth-dhcp.network
    [Match]
    Name=enp0s8

    [Network]
    DHCP=ipv4
    MulticastDNS=yes

    [DHCP]
    UseDomains=true
    UseRoutes=false
    
    % systemctl enable systemd-networkd.service
    % systemctl enable systemd-resolved.service
    ```

5. 配置 SSH 密钥对，禁用密码远程登录
6. Headless 模式
1. 启用增强功能
7. 配置共享宿主机目录（需要启用增强功能）
    - Settings -> Shared Folders 
    - 进入系统后执行如下操作：
        ```bash
            sudo apt install dkms
            sudo usermod -a -G vboxsf $USER
        ```

## VDI 文件瘦身

- [How to compact VirtualBox's VDI file size?](https://superuser.com/questions/529149/how-to-compact-virtualboxs-vdi-file-size)
- [8.22. VBoxManage modifymedium](https://www.virtualbox.org/manual/ch08.html#vboxmanage-modifyvdi)

1. 登录虚拟机系统后执行以下命令，将系统空间用 /dev/zero 写满到一个临时文件，然后将其清除

    ```bash
    $ sudo apt install pv
    $ dd if=/dev/zero | pv | sudo dd of=/bigemptyfile bs=4096k; sudo rm -fv /bigemptyfile
    ```

2. 关机后，在宿主机上执行以下命令，将空间释放（~~2022-12-05：难过……宿主机为 Windows 时无效~~ 2022-12-06：注意到临时文件的位置位于 /tmp 目录，而操作机器中 /tmp 目录独占一个分区，所以无效。将临时文件改为 /bigemptyfile 即可。）

    ```zsh
    [16:38]davinci@Monterey:~
    % l ~/VMs/xubuntu/xubuntu.vdi
    -rw-------  1 davinci  staff    16G  7 Jul 16:39 /Users/davinci/VMs/xubuntu/xubuntu.vdi
    [16:39]davinci@Monterey:~
    % VBoxManage modifymedium --compact ~/VMs/xubuntu/xubuntu.vdi
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
    [16:45]davinci@Monterey:~
    % l ~/VMs/xubuntu/xubuntu.vdi
    -rw-------  1 davinci  staff   8.8G  7 Jul 16:45 /Users/davinci/VMs/xubuntu/xubuntu.vdi
    ```
