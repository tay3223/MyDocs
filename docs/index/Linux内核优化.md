# Linux内核优化

<br>

参见：[我的git仓库](https://gitee.com/tay3223/neihe/blob/master/002---%E5%86%85%E6%A0%B8%E4%BC%98%E5%8C%96.sh)

<br>

或粘贴以下地址到浏览器进行访问：

```
https://gitee.com/tay3223/neihe/blob/master/002---%E5%86%85%E6%A0%B8%E4%BC%98%E5%8C%96.sh
```

<br>

脚本内容如下：

```
#!/bin/bash


# ============================
# 路径声明
# ============================
    url_001="/etc/sysctl.conf"
    url_002="/etc/security/limits.conf"





# ============================
# 安全措施
# ============================
    mkdir -p ~/bakup
    cp ${url_001} ~/bakup
    cp ${url_002} ~/bakup




# ============================
# 修改内核参数方法（两种）:
# ============================
# 1、使用echo value方式直接追加到文件里
#       例如：echo "1" >/proc/sys/net/ipv4/tcp_syn_retries，
#	    但这种方法设备重启后又会恢复为默认值
#
# 2、把参数添加到/etc/sysctl.conf中，
#	    然后执行sysctl -p使参数生效，永久生效





# ============================
# 开始优化
# ============================

#关闭ipv6
echo "net.ipv6.conf.all.disable_ipv6 = 1                    " >> ${url_001}
echo "net.ipv6.conf.default.disable_ipv6 = 1                " >> ${url_001}

# 避免放大***
echo "net.ipv4.icmp_echo_ignore_broadcasts = 1              " >> ${url_001}

# 开启恶意icmp错误消息保护
echo "net.ipv4.icmp_ignore_bogus_error_responses = 1        " >> ${url_001}

#关闭路由转发
echo "net.ipv4.ip_forward = 0                               " >> ${url_001}
echo "net.ipv4.conf.all.send_redirects = 0                  " >> ${url_001}
echo "net.ipv4.conf.default.send_redirects = 0              " >> ${url_001}



#关闭sysrq功能
echo "kernel.sysrq = 0   					                " >> ${url_001}

#core文件名中添加pid作为扩展名
echo "kernel.core_uses_pid = 1   				            " >> ${url_001}


#修改消息队列长度
echo "kernel.msgmnb = 65536   					            " >> ${url_001}
echo "kernel.msgmax = 65536   					            " >> ${url_001}


#timewait的数量，默认180000
echo "net.ipv4.tcp_sack = 1   					            " >> ${url_001}

#网络接口接收数据包的速率比内核处理这些包的速率快时，允许送到队列的数据包的最大数目,默认是300
echo "net.core.netdev_max_backlog = 262144   			    " >> ${url_001}

#防止简单的DoS,系统所能处理不属于任何进程的TCP sockets最大数量
echo "net.ipv4.tcp_max_orphans = 3276800   			        " >> ${url_001}


#内核放弃建立连接之前发送SYNACK 包的数量
echo "net.ipv4.tcp_synack_retries = 1   			        " >> ${url_001}

#内核放弃建立连接之前发送SYN 包的数量
echo "net.ipv4.tcp_syn_retries = 1   				        " >> ${url_001}


#开启重用。允许将TIME-WAIT sockets 重新用于新的TCP 连接
echo "net.ipv4.tcp_mem = 94500000 915000000 927000000   	" >> ${url_001}
echo "net.ipv4.tcp_fin_timeout = 1   				        " >> ${url_001}


#允许系统打开的端口范围
echo "net.ipv4.ip_local_port_range = 1024    65000   		" >> ${url_001}




#====================================================================================

# 内核panic时，1秒后自动重启
echo "kernel.panic = 1                                      " >> ${url_001}



# 允许更多的PIDs (减少滚动翻转问题); may break some programs 32768
echo "kernel.pid_max = 32768                                " >> ${url_001}



# 内核所允许的最大共享内存段的大小（bytes）
echo "kernel.shmmax = 4294967296                            " >> ${url_001}



# 在任何给定时刻，系统上可以使用的共享内存的总量（pages）
echo "kernel.shmall = 1073741824                            " >> ${url_001}



# 设定程序core时生成的文件名格式
echo "kernel.core_pattern = core_%e                         " >> ${url_001}



# 当发生oom时，自动转换为panic
echo "vm.panic_on_oom = 1                                   " >> ${url_001}



# 表示强制Linux VM最低保留多少空闲内存（Kbytes）
echo "vm.min_free_kbytes = 1048576                          " >> ${url_001}



# 该值高于100，则将导致内核倾向于回收directory和inode cache
echo "vm.vfs_cache_pressure = 250                           " >> ${url_001}



# 表示系统进行交换行为的程度，数值（0-100）越高，越可能发生磁盘交换
echo "vm.swappiness = 20                                    " >> ${url_001}



# 仅用10%做为系统cache
echo "vm.dirty_ratio = 10                                   " >> ${url_001}



# 增加系统文件描述符限制 2^20-1
echo "fs.file-max = 1048575                                 " >> ${url_001}



# 网络层优化

# listen()的默认参数,挂起请求的最大数量，默认128
echo "net.core.somaxconn = 1024                             " >> ${url_001}



# 增加Linux自动调整TCP缓冲区限制
echo "net.core.wmem_default = 8388608                       " >> ${url_001}
echo "net.core.rmem_default = 8388608                       " >> ${url_001}
echo "net.core.rmem_max = 16777216                          " >> ${url_001}
echo "net.core.wmem_max = 16777216                          " >> ${url_001}






# 开启SYN洪水***保护
echo "net.ipv4.tcp_syncookies = 1                           " >> ${url_001}



# 开启并记录欺骗，源路由和重定向包
echo "net.ipv4.conf.all.log_martians = 1                    " >> ${url_001}
echo "net.ipv4.conf.default.log_martians = 1                " >> ${url_001}



# 处理无源路由的包
echo "net.ipv4.conf.all.accept_source_route = 0             " >> ${url_001}
echo "net.ipv4.conf.default.accept_source_route = 0         " >> ${url_001}



# 开启反向路径过滤
echo "net.ipv4.conf.all.rp_filter = 1                       " >> ${url_001}
echo "net.ipv4.conf.default.rp_filter = 1                   " >> ${url_001}



# 确保无人能修改路由表
echo "net.ipv4.conf.all.accept_redirects = 0                " >> ${url_001}
echo "net.ipv4.conf.default.accept_redirects = 0            " >> ${url_001}
echo "net.ipv4.conf.all.secure_redirects = 0                " >> ${url_001}
echo "net.ipv4.conf.default.secure_redirects = 0            " >> ${url_001}






# TTL
echo "net.ipv4.ip_default_ttl = 64                          " >> ${url_001}



# 增加TCP最大缓冲区大小
echo "net.ipv4.tcp_rmem = 4096 87380 8388608                " >> ${url_001}
echo "net.ipv4.tcp_wmem = 4096 32768 8388608                " >> ${url_001}



# Tcp自动窗口
echo "net.ipv4.tcp_window_scaling = 1                       " >> ${url_001}



# 进入SYN包的最大请求队列.默认1024
echo "net.ipv4.tcp_max_syn_backlog = 8192                   " >> ${url_001}



# 打开TIME-WAIT套接字重用功能，对于存在大量连接的Web服务器非常有效。 
echo "net.ipv4.tcp_tw_recycle = 1                           " >> ${url_001}
echo "net.ipv4.tcp_tw_reuse = 0                             " >> ${url_001}



# 表示是否启用以一种比超时重发更精确的方法（请参阅 RFC 1323）来启用对 RTT 的计算；为了实现更好的性能应该启用这个选项
echo "net.ipv4.tcp_timestamps = 0                           " >> ${url_001}









# 减少TCP KeepAlive连接侦测的时间，使系统可以处理更多的连接。 
# 如果某个TCP连接在idle 300秒后,内核才发起probe.如果probe 2次(每次2秒)不成功,内核才彻底放弃,认为该连接已失效.
echo "net.ipv4.tcp_keepalive_time = 300                     " >> ${url_001}
echo "net.ipv4.tcp_keepalive_probes = 2                     " >> ${url_001}
echo "net.ipv4.tcp_keepalive_intvl = 2                      " >> ${url_001}






# 系统同时保持TIME_WAIT套接字的最大数量，如果超过这个数字，TIME_WAIT套接字将立刻被清除并打印警告信息。
echo "net.ipv4.tcp_max_tw_buckets = 20000                   " >> ${url_001}



# arp_table的缓存限制优化
echo "net.ipv4.neigh.default.gc_thresh1 = 128               " >> ${url_001}
echo "net.ipv4.neigh.default.gc_thresh2 = 512               " >> ${url_001}
echo "net.ipv4.neigh.default.gc_thresh3 = 4096              " >> ${url_001}
























# =====================================
# /etc/security/limits.conf的相关设置
# =====================================
#
#
# 系统性能一直是一个受关注的话题，如何通过最简单的设置来实现最有效的性能调优，如何在有限资源的条件下保证程序的运作，ulimit 是我们在处理这些问题时，经常使用的一种简单手段。ulimit 是一种 linux 系统的内键功能，它具有一套参数集，用于为由它生成的 shell 进程及其子进程的资源使用设置限制。本文将在后面的章节中详细说明 ulimit 的功能，使用以及它的影响，并以具体的例子来详细地阐述它在限制资源使用方面的影响。
#
#
# 假设有这样一种情况，当一台 Linux 主机上同时登陆了 10 个人，在系统资源无限制的情况下，这 10 个用户同时打开了 500 个文档，而假设每个文档的大小有 10M，这时系统的内存资源就会受到巨大的挑战。而实际应用的环境要比这种假设复杂的多，例如在一个嵌入式开发环境中，各方面的资源都是非常紧缺的，对于开启文件描述符的数量，分配堆栈的大小，CPU 时间，虚拟内存大小，等等，都有非常严格的要求。资源的合理限制和分配，不仅仅是保证系统可用性的必要条件，也与系统上软件运行的性能有着密不可分的联系。这时，ulimit 可以起到很大的作用，它是一种简单并且有效的实现资源限制的方式。
#
#
# ulimit 用于限制 shell 启动进程所占用的资源，支持以下各种类型的限制：所创建的内核文件的大小、进程数据块的大小、Shell 进程创建文件的大小、内存锁住的大小、常驻内存集的大小、打开文件描述符的数量、分配堆栈的最大大小、CPU 时间、单个用户的最大线程数、Shell 进程所能使用的最大虚拟内存。同时，它支持硬资源和软资源的限制。
#
#
# 作为临时限制，ulimit 可以作用于通过使用其命令登录的 shell 会话，在会话终止时便结束限制，并不影响于其他 shell 会话。而对于长期的固定限制，ulimit 命令语句又可以被添加到由登录 shell 读取的文件中，作用于特定的 shell 用户。
#
#
# 如何使用 ulimit
# ulimit 通过一些参数选项来管理不同种类的系统资源。
# ulimit 命令的格式为：ulimit [options] [limit]
# 具体的 options 含义以及简单示例可以参考以下表格。
#
#
# 选项[options]		含义								                    例子
# ======================================================================================================================================================
# -H			设置硬资源限制，一旦设置不能增加。				            ulimit – Hs 64；	限制硬资源，线程栈大小为 64K。
# -S			设置软资源限制，设置后可以增加，但是不能超过硬资源设置。	ulimit – Sn 32；	限制软资源，32 个文件描述符。
# -a			显示当前所有的 limit 信息。					                ulimit – a；		显示当前所有的 limit 信息。
# -c			最大的 core 文件的大小， 以 blocks 为单位。			        ulimit – c unlimited； 对生成的 core 文件的大小不进行限制。
# -d			进程最大的数据段的大小，以 Kbytes 为单位。			        ulimit -  d unlimited；	对进程的数据段大小不进行限制。
# -f			进程可以创建文件的最大值，以 blocks 为单位。			    ulimit – f 2048；	限制进程可以创建的最大文件大小为 2048 blocks。
# -l			最大可加锁内存大小，以 Kbytes 为单位。				        ulimit – l 32；	限制最大可加锁内存大小为 32 Kbytes
# -m			最大内存大小，以 Kbytes 为单位。				            ulimit – m unlimited；	对最大内存不进行限制
# -n			可以打开最大文件描述符的数量。					            ulimit – n 128；	限制最大可以使用 128 个文件描述符。
# -p			管道缓冲区的大小，以 Kbytes 为单位。				        ulimit – p 512；	限制管道缓冲区的大小为 512 Kbytes。
# -s			线程栈大小，以 Kbytes 为单位。					            ulimit – s 512；	限制线程栈的大小为 512 Kbytes。
# -t			最大的 CPU 占用时间，以秒为单位。				            ulimit – t unlimited；	对最大的 CPU 占用时间不进行限制。
# -u			用户最大可用的进程数。						                ulimit – u 64；	限制用户最多可以使用 64 个进程。
# -v			进程最大可用的虚拟内存，以 Kbytes 为单位。			        ulimit – v 200000；	限制最大可用的虚拟内存为 200000 Kbytes。
#
#
# 我们可以通过以下几种方式来使用 ulimit：
# 	1，在用户的启动脚本中.可以在用户的目录下的 .bashrc 文件中，加入 ulimit – u 64，来限制用户最多可以使用 64 个进程.
# 	2，在应用程序的启动脚本中,如果用户要对某个应用程序 myapp 进行限制，可以写一个简单的脚本 startmyapp。
# 		例如：
# 		echo "ulimit – s 512	" >> startmyapp 
# 		echo "myapp		        " >> startmyapp
# 		./startmyapp
# 		以后只要通过脚本 startmyapp 来启动应用程序，就可以限制应用程序 myapp 的线程栈大小为 512K。
# 	3，直接在控制台输入
# 		user@tc511-ui:~>ulimit – p 256
# 		限制管道的缓冲区为 256K。
#
#
# 用户进程的有效范围
# 	ulimit 作为对资源使用限制的一种工作，是有其作用范围的。那么，它限制的对象是单个用户，单个进程，还是整个系统呢？
# 	事实上，ulimit 限制的是当前 shell 进程以及其派生的子进程。
# 		举例来说：
#		如果用户同时运行了两个 shell 终端进程，只在其中一个环境中执行了 ulimit – s 100，
# 		则该 shell 进程里创建文件的大小收到相应的限制，
#		而同时另一个 shell 终端包括其上运行的子程序都不会受其影响：
#
#
# 那么是否有针对某个具体用户的资源加以限制的方法呢？
# 	答案是有的，方法是通过修改系统的 /etc/security/limits 配置文件。
# 	该文件不仅能限制指定用户的资源使用，还能限制指定组的资源使用。该文件的每一行都是对限定的一个描述，
#
# 	格式如下：
# 	<domain> <type> <item> <value>
#	
#	<domain>：表示用户或者组的名字，还可以使用 * 作为通配符。
#	<Type>：有（soft，hard，-）这三个值，	
# 		soft：指的是当前系统生效的设置值。
# 		hard：表明系统中所能设定的最大值。soft 的限制不能比har 限制高。
# 		 -  ：用 - 就表明同时设置了 soft 和 hard 的值。
#	
#	<Item>：则表示需要限定的资源，可以有很多候选值，如 stack，cpu，nofile 等等，分别表示最大的堆栈大小，占用的 cpu 时间，以及打开的文件数。通过添加对应的一行描述，则可以产生相应的限制。
# 		<Item>的候选值：
# 		core - 100	    限制内核文件的大小
# 		date - 100	    最大数据大小
# 		fsize - 100	    最大文件大小
# 		memlock - 100	最大锁定内存地址空间
# 		nofile - 100	打开文件的最大数目
# 		rss - 100	    最大持久设置大小
# 		stack - 100	    最大栈大小
# 		cpu - 100	    以分钟为单位的最多 CPU 时间
# 		noproc - 100	进程的最大数目（系统理论最大值，4090个进程，默认是无上限的）
# 		as - 100	    地址空间限制
# 		maxlogins - 100	此用户允许登录的最大数目
#
#
# 现在已经可以对进程和用户分别做资源限制了，看似已经足够了，其实不然。
# 很多应用需要对整个系统的资源使用做一个总的限制，这时候我们需要修改 /proc 下的配置文件。
# /proc 目录下包含了很多系统当前状态的参数，
# 例如 /proc/sys/kernel/pid_max，/proc/sys/net/ipv4/ip_local_port_range 等等，
# 从文件的名字大致可以猜出所限制的资源种类。由于该目录下涉及的文件众多，在此不一一介绍。



```
















<br><br><br><hr><br><br><br>

#### 帮助作者改进文档
如果您喜欢这篇文档，想让它变得更好，您可以：

- 推荐这篇文档，让更多的人知道。
- 给作者反馈和建议：*_<tianye3223@gmail.com>_*

<br><br><br><br><br>