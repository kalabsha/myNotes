# 内核参数调优

- 禁止使用 swap 内存

    ```ini
    vm.swappiness = 0
    ```

- 启用 Magic SysRq key，1 为接受所有 SysRq 请求，它可以让内核忽略任何正在进行的任务来响应 SysRq key, 除非内核已经完全锁死

    ```ini
    kernel.sysrq = 1
    ```

- ARP 缓存表中 Stale 状态的过期时间，如果过期会被启动的垃圾回收器回收删除

    ```ini
    net.ipv4.neigh.default.gc_stale_time = 120
    ```

- 禁用反向路由过滤

    ```ini
    net.ipv4.conf.all.rp_filter = 0
    net.ipv4.conf.default.rp_filter = 0
    net.ipv4.conf.{{ network_name }}.rp_filter = 0
    ```

- 只响应目的 IP 地址为接收网卡上的本地地址的 ARP 请求，并且 ARP 请求的源 IP 必须和接收网卡同网段

    ```ini
    net.ipv4.conf.default.arp_announce = 2
    net.ipv4.conf.lo.arp_announce = 2
    net.ipv4.conf.all.arp_announce = 2
    ```

- 发送套接字缓冲区大小默认值

    ```ini
    net.core.wmem_default = 8388608
    ```

- 接收套接字缓冲区大小默认值

    ```ini
    net.core.rmem_default = 8388608
    ```

- 接收套接字缓冲区大小最大值

    ```ini
    net.core.rmem_max = 16777216
    ```

- 发送套接字缓冲区大小最大值

    ```ini
    net.core.wmem_max = 16777216
    ```

- somaxconn 是系统级套接字监听队列上限，TCP Accept 队列长度，超过这个数量就会导致链接超时或者触发重传机制

    ```ini
    net.core.somaxconn = 65536
    ```

- SYN Cookies 开关
  - 0 表示关闭 SYN Cookies；
  - 1 表示在新连接压力比较大时启用 SYN Cookies
  - 2 表示始终使用 SYN Cookies

    ```ini
    net.ipv4.tcp_syncookies = 1
    ```

- TCP SYN 队列长度，表示可以容纳多少等待连接的网络连接数，服务端收到 SYN 后，会把连接放入这个队列

    ```ini
    net.ipv4.tcp_max_syn_backlog = 81920
    ```

- SYNACK 的重传次数，服务端发送 SYNACK 后，如果没有收到客户端的 ACK 后，重发 SYNACK 的次数

    ```ini
    net.ipv4.tcp_synack_retries = 3
    ```

- SYN 的重传次数，客户端发送 SYN 后，如果没有收到服务端的 SYNACK 后，重发 SYN 次数

    ```ini
    net.ipv4.tcp_syn_retries = 3
    ```

- 处于 FIN_WAIT_2 连接状态的时间

    ```ini
    net.ipv4.tcp_fin_timeout = 30
    ```

- 当 keepalive 启用时，TCP 发送 keepalive 消息的频度

    ```ini
    net.ipv4.tcp_keepalive_time = 300
    ```

- 表示系统同时保持 TIME_WAIT 套接字的最大数量，如果超过此数，TIME_WAIT 套接字会被立刻清除并且打印警告信息

    ```ini
    net.ipv4.tcp_max_tw_buckets = 200000
    ```

- 表示允许将 TIME_WAIT sockets 重新用于新的 TCP 连接

    ```ini
    net.ipv4.tcp_tw_reuse = 1
    ```

- 路由缓冲的最大值，一旦缓冲满，就清除旧的条目

    ```ini
    net.ipv4.route.max_size = 5242880
    ```

- 0 表示不检查时间戳，如果开启会导致 Client 和 NAT 后面的服务器通讯出现问题

    ```ini
    net.ipv4.tcp_timestamps = 0    
    ```

- 开启 core-dump 后，一旦程序发生异常，会将进程导出到文件

    ```ini
    kernel.core_pattern = /var/log/corefiles/core_%e_%p_%t
    ```

- 可用端口范围

    ```ini
    net.ipv4.ip_local_port_range = 10000 65535
    ```
