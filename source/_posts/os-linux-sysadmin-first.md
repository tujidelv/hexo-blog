---
title: Linux 系统管理：磁盘、文件系统、RAID、LVM2、程序包
date: 2019-01-21 21:19:31
categories:
- 操作系统
tags:
- linux
---

# Linux 系统管理

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

与系统管理相关...
```
磁盘管理、文件系统管理
RAID基础原理、LVM2
网络管理：TCP/IP协议、Linux网络属性配置
程序包管理：rpm, yum
进程管理：htop, glance, tsar等
sed和awk
Linux系统开机流程
内核管理基础知识：编译内核、模块
Linux系统裁剪：kernel+busybox
```

## 正篇

### 磁盘管理

```
I/O设备：
    磁盘：用来实现本地数据的持久存储功能
    网卡：通过网络交换数据
    键盘、鼠标、显示器等
-----------------
I/O设备在硬件层次上有I/O Ports来标识,即当前主机上的I/O设备地址
I/O设备在操作系统层次上有设备文件来标识
-----------------
块设备：block device，存取单位“块”，支持以"block"为单位进行随机访问，磁盘
字符设备：character device，存取单位“字符”，支持以"character"为单位进行线性访问，键盘
```
- `设备文件`
    ```
    特殊文件
    只有元数据(inode)，而没有数据
    关联至一个设备驱动程序，进而能够跟与之对应硬件设备进行通信
    -----------------
    设备号：
        主设备号：major number, 标识设备类型，进而确定要加载的驱动程序
        次设备号：minor number, 标识同一类型下的不同设备
    -----------------
    设备文件的命名：由ICANN
        /dev/DEV_FILE
    ```
- `硬盘接口类型`
    ```
    并行：
        IDE：早期的个人桌面常用接口,后升级为SATA,133MB/s
        SCSI(Small Computer System Interface)：与IDE同时代的企业级接口,640MB/s
            使用时长,读写方面大概相当于IDE接口的4-8倍,它的rpm(转速,rotations per minute)很高
    串行：
        SATA：6Gbps/8
        SAS：6Gbps/8
        USB：480MB/s

    以上皆为接口速率,非硬件设备速率
    ```
- `磁盘设备的设备文件命名`
    ```
    CentOS5：
        IDE: /dev/hd[a-z]
        SCSI, SATA, SAS, USB: /dev/sd[a-z]
    CentOS6以后,全都统一为/dev/sd[a-z]
        不同设备：a-z
            /dev/sda, /dev/sdb, ...
        同一设备上的不同分区(每个分区分完以后都会被当作一个独立设备来管理)：1,2, ...
            主+扩展：/dev/sda[1-4]
            逻辑：/dev/sda[5]
    ```
- `机械式硬盘`
    ```
    磁道：track
        扇区：sector,512bytes,现在最大可以到4K
    柱面：cylinder
        分区根据柱面划分,一个柱面只能属于一个分区,多个柱面由外而内组合在一起当作一个分区来使用,靠外柱面的这些分区读写性能好一点
    -----------------
    0磁道0扇区：512bytes,被预留出来,不属于任何分区,常被成为MBR
        MBR: Master Boot Record,主引导记录,引导启动OS
            446bytes: bootloader, 引导加载器程序
            64bytes：分区表,记录分了哪几个区,每个区从哪开始从哪结束
                16bytes: 标识一个分区,所以一个磁盘最多只能有4个主分区
            2bytes: 55AA,MBR有效性标记

        4个主分区：
            3主分区+1扩展分区(切割1个或多个逻辑分区)
                扩展分区不能直接当空间使用,只是分区表的一个扩展指向
    -----------------
    现代的MBR机制无法识别2T以上的磁盘空间;
    现在的硬盘由UEFI进行引导并结合GPT机制以后,能支持更多的磁盘分区,能识别2T以上的磁盘空间
    ```
- `分区管理工具(fdisk, parted, sfdisk)`
    ```
    fdisk：历久弥坚的,各大Linux发行版都默认有提供,能满足大多数场景中的管理需要,对于一块硬盘来讲，最多只能管理15分区。
    ----------------------------
    查看指定设备磁盘分区信息：
        # fdisk -l [device...]
            后面不跟设备则显示所有设备的磁盘分区信息
    创建/管理分区：
        # fdisk device
            交互式界面，有许多子命令
                p: print, 显示磁盘已有分区
                n: new, 新建分区
                w: write, 保存(写入磁盘)并退出
                q: quit,  不保存(放弃更新)退出
                ------------
                m: 获取帮助
                d: delete，删除分区
                l: 列出所有分区ID
                t: 调整分区ID
    查看内核是否已经识别新的分区：
        # cat /proc/partitions
    通知内核重新读取硬盘分区表：
        对于已经有分区处于使用状态的磁盘来讲，新建分区后需要让内核重读其分区表
        CentOS 5:
            # partprobe [DEVICE]
        CentOS 6:
            # partx -a [DEVICE]
                -n M:N
            # kpartx -af [DEVICE]
    ```
    
### 文件系统管理

```
Linux支持众多类型的文件系统: ext2, ext3, ext4, xfs, btrfs, reiserfs, jfs, swap
    ext2,ext3,ext4: ext2,ext3在centos5上流行;ext4在centos6上流行,在centos7上依然可以使用
    xfs, btrfs: xfs在centos6上支持,centos7上流行;btrfs是centos7上自带的非常流行的非常强大的支持64位的空间可自动扩展的文件系统
    swap: 交换分区,缓解物理内存资源不够用的情况
    iso9660：光盘
Windows：fat32(在linux上被识别为vfat), ntfs(linux对ntfs的支持还不是特别好)
Unix: FFS, UFS, JFS2
网络文件系统：NFS, CIFS
集群文件系统：GFS2, OCFS2
分布式文件系统：ceph, moosefs, mogilefs, GlusterFS, Lustre
---------------------------
根据其是否支持"journal(日志)"功能：
    日志型文件系统: ext3, ext4, xfs, ...
        系统检测速度快,但每次文件的写操作都会多一次IO操作,因此性能上可能不如非日志型文件系统好,不过到现在为止这种差别是微乎其微的,但带来的好处却是显而易见的.
        推荐使用
    非日志型文件系统: ext2, vfat
---------------------------
文件系统的组成部分：
    内核中的模块：ext4, xfs, vfat, ...
        Linux内核是模块化的，这些模块支持动态装载和卸载；文件系统可能会被直接打包进内核，也可以被编译成内核模块；即任何一个文件系统想要被使用,对应的内核模块一定要被装载
        查看内核中已装载的所有模块：# lsmod
    用户空间的管理工具：mkfs.ext4, mkfs.xfs, mkfs.btrfs, mkfs.vfat, ...
Linux的虚拟文件系统：VFS
    程序员和各个文件系统的中间层
---------------------------
查看系统支持哪些文件系统类型：
    # cat /proc/filesystems
---------------------------
超级块用来存储整个分区当中的整个结构划分的,例如有多少个块组,每个块组有多少个块,每个块有多大,每个组当中有多少块被占用多少块是free的等等
为防止超级块损坏,需要对超级块做备份,通常格式化以后会显示超级块的位置(存放在块组当中的某些位置)
---------------------------
Linux启动过程：
    1.加电自检
    2.装载bootloader
    3.而后bootloader去装载用户所选定的操作系统的内核(应用程序)
    4.内核自身完成初始化以后,取得系统控制权,首先去装载根文件系统(rootfs)所在的分区,该分区跟内核所在的分区可以不是同一个分区
        /etc,/bin,/sbin,/lib,/lib64,/proc,/sys,/dev等都在该分区上,且这些目录不能单独分区
    5.内核启动/sbin/init程序,由它代替内核来完成一切跟用户相关的程序的启动
        # pstree
```
- `创建文件系统(格式化)`
    ```
    即需要用户空间的管理工具来调用内核中所提供的功能方能实现我们所期望拥有到的文件系统所谓的管理功能
    ---------------------------
    mkfs：build a Linux filesystem
        (1) # mkfs.FS_TYPE /dev/DEVICE
        (2) # mkfs -t FS_TYPE /dev/DEVICE

        -L 'LABEL': 设定卷标,将来可以根据卷标来对这个分区执行调用操作
        -f：强制
    mke2fs：ext系列文件系统专用管理工具
        -t {ext2|ext3|ext4}：指定文件系统 
        -b {1024|2048|4096}：指定块大小,块是分区格式化以后的一个单独存储单位,一般包含2个、4个或8个扇区,即块大小为1k,2k,4k;
        -L 'LABEL',设定卷标
        -j: journal,相当于 -t ext3
            mkfs.ext3 = mkfs -t ext3 = mke2fs -j = mke2fs -t ext3
        -i #: 为数据空间中每多少个字节创建一个inode；此大小不应该小于block的大小；一个块只能属于一个文件,一个文件至少包含一个块
        -N #：为数据空间创建个多少个inode；
        -m #: 为管理人员预留的空间占据的百分比；默认为5
        -O FEATURE[,...]：启用指定特性
            -O ^FEATURE：关闭指定特性
    mkswap：创建交换分区
        # mkswap [options] device
            -L 'LABEL'
        前提：调整其分区的ID为82；
    ```
- `其它常用工具`
    ```
    blkid：查看块设备属性信息
        # blkid [OPTION]... [DEVICE]
            -U UUID: 根据指定的UUID来查找对应的设备
            -L LABEL：根据指定的LABEL来查找对应的设备
    e2label：管理ext系列文件系统的LABEL
        # e2label DEVICE [LABEL]
            除了可以查看块设备的LABEL,还可以设定LABEL,避免将来需要修改LABEL时要重新格式化了
    tune2fs：重新设定ext系列文件系统的可调整参数的值(块大小无法调整)
        # tune2fs [OPTION]... DEVICE
            -l：查看指定文件系统超级块信息；super block
            -L 'LABEL'：修改卷标
            -m #：修预留给管理员的空间百分比
            -j: 将ext2升级为ext3
            -O [^]FEATURE: 文件系统属性启用或禁用
            -o [^]mount-options: 调整文件系统的默认挂载选项
            -U UUID: 修改UUID号
    dumpe2fs：查看ext系列文件系统的超级块信息,还能查看文件系统的组织结构信息(块组)
        # dumpe2fs [OPTION]... DEVICE
            -h：只查看超级块信息
    ```
- `文件系统检测`
    ```
    因进程意外中止或系统崩溃等情况导入写入操作非正常中止时，或者非法关机时,可能会导致文件损坏；此时，应该修复文件系统
    虽然开机以后会自动检测,但是自动检测会导致开机速度过慢,因此很多非关键性的文件系统建议开机时不自动检测,而是开机以后可以手动检测
    ---------------------------
    fsck: File System CheCk
        (1) # fsck.FS_TYPE /dev/DEVICE
        (2) # fsck -t FS_TYPE /dev/DEVICE

        -a: 自动修复错误
        -r: 交互式修复错误

        Note: FS_TYPE一定要与分区上已有的文件系统类型相同；
    e2fsck：ext系列文件专用的检测修复工具
        -y：自动回答为yes; 
        -f：强制进行检测修复；
    ```
- `文件系统挂载`
    ```
    将额外的文件系统(分区)与根文件系统上某现存的目录建立起关联关系，进而使得此目录做为其它文件系统的访问入口的行为(过程)称之为挂载；
        目录中(挂载点)的原有文件在挂载完成后会被临时隐藏
        即把要关联的设备关联至挂载点(另一个文件系统的访问入口)
    默认只有管理员才有权限；
    解除此关联关系的过程称之为卸载；
    根文件系统(rootfs)无需挂载,它是由内核探测来实现装载的；
    ---------------------------
    挂载：mount DEVICE MOUNT_POINT
        查看所有已经挂载的设备：
                # cat /etc/mtab
                # mount
                    通过查看/etc/mtab文件显示当前系统已挂载的所有设备
                # cat /proc/mounts
                    查看内核追踪到的已挂载的所有设备
        # mount [-fnrsvw] [-t vfstype] [-o options] device dir
            device：指明要挂载的设备；
                (1) 设备文件：例如/dev/sda5
                (2) 卷标：-L 'LABEL', 例如 -L 'MYDATA'
                (3) UUID, -U 'UUID'：例如 -U '0c50523c-43f1-45e7-85c0-a126711d406e'
                (4) 伪文件系统名称：proc, sysfs, devtmpfs, configfs
            dir：挂载点
                事先存在；建议使用空目录；

            常用命令选项：
                -t vfstype：指定要挂载的设备上的文件系统类型,可省略,会自动调用blkid命令来判断此目录的文件系统的类型；
                -r: readonly，只读挂载；
                -w: read and write, 读写挂载；
                -n: 挂载时，不更新/etc/mtab文件；
                -a：自动挂载所有支持自动挂载的设备；(定义在了/etc/fstab文件中，且挂载选项中有“自动挂载”功能)
                -L 'LABEL': 以卷标指定要挂载的设备；
                -U 'UUID': 以UUID指定要挂载的设备；
                -B, --bind: 绑定目录到另一个目录上；

            -o options：(文件系统挂载时所启用的特性)
                async：异步模式；性能高,安全可靠性低
                sync：同步模式；性能差,安全可靠性高
                atime/noatime：文件和目录被访问时是否更新最近一次的访问时间戳
                diratime/nodiratime：目录被访问时是否更新最近一次的访问时间戳
                auto/noauto：是否支持(mount的-a选项)自动挂载
                exec/noexec：是否支持将文件系统上应用程序运行为进程
                dev/nodev：是否支持在此文件系统上使用设备文件；
                suid/nosuid：是否支持在此设备的文件上使用suid
                remount：重新挂载，通常用于不卸载的情况下重新指定挂载选项
                ro：只读
                rw：读写
                user/nouser：是否允许普通用户挂载此设备
                acl：启用此文件系统上的acl功能，默认不支持

                注意：上述选项可多个同时使用，彼此使用逗号分隔；
                      默认挂载选项：defaults
                            rw, suid, dev, exec, auto, nouser, and async
                      eg：# mount -o remount,ro /dev/sda3
    ---------------------------
    卸载：
        # umount DEVICE
        # umount MOUNT_POINT
        
        进程正在使用中的设备无法被卸载;挂载点没有被进程访问时方可以卸载
        查看哪些进程正在访问指定文件系统：
            # fuser -v MOUNT_POINT
        终止所有正在访问指定的文件系统的进程：
            # fuser -km MOUNT_POINT
    ---------------------------
    挂载交换分区：
        启用：# swapon [OPTION]... [DEVICE]
                -a：激活所有的交换分区；
                -p PRIORITY：指定优先级；
        禁用：# swapoff [OPTION]... [DEVICE]
                -a：禁用所有；
    ---------------------------
    挂载光盘设备：
        光盘设备文件：
            IDE: /dev/hdc
                对于红帽系列,RHEL6或CentOs6以后,统一为sr0...
            SATA: /dev/sr0

            符号链接文件：/dev/cdrom,/dev/cdrw,/dev/dvd,/dev/dvdrw
        # mount -r /dev/cdrom /media/cdrom
        # umount /dev/cdrom
    ```
    ```
    自动挂载的设备的配置文件：/etc/fstab
         使用mount命令手动挂载的文件系统在系统重启以后会失效(不会被自动挂载),需要添加至/etc/fstab文件中才能自动挂载
         因为系统初始化过程的脚本会读取此文件(每行定义一个要挂载的文件系统),并尝试使用mount/swapon来实现额外文件系统的挂载
         ---------------------------
         UUID=3437f1a0-f850-4f1b-8a7c-819c5f6a29e4 /   ext4    defaults,discard,noatime      1 1
         UUID=ad1361f7-4ab4-4252-ba00-5d4e5d8590fb /boot   ext3    defaults     1 2
         
         6字段：
            要挂载的设备或伪文件系统：
               设备文件、LABEL(LABEL="")、UUID(UUID="")、伪文件系统名称(proc, sysfs)
            挂载点：
            文件系统类型：
            挂载选项：
                defaults,挂载选项可以有多个，彼此间使用逗号分隔；
            转储频率：
                0：从不转储(不做备份)
                1: 每天转储
                2: 每隔一天转储
            自检次序：
                0：不自检，一般来讲,额外创建的文件系统都无须自动自检,也不要转储
                1：首先自检，通常只有根文件系统(rootfs)需要首先自检
                2：次级自检，不同的设备可以使用同一个自检次序
                ...
    ```
- `磁盘空间查看工具`
    ```
    查看内存及交换分区的使用信息：
        # free [OPTION]
            -m: 以MB为单位
            -g: 以GB为单位
        注：其中"+/- buffer/cached"的userd和free的值为Mem的值+/-(buffers+cached)的值
    查看文件系统空间占用等信息：
        # df [OPTION]
            -h: human-readable,人类易读的
            -i：inodes instead of blocks,显示inode的使用信息而非默认的磁盘空间使用信息
            -P: 以Posix兼容的格式输出,每个设备信息显示在同一行,防止路径长的设备多行显示
    查看某目录总体空间占用状态
        # du [OPTION]... DIR
            -h: human-readable
            -s: summary,不加此选项默认显示DIR下的每个文件的大小,加上此选项显示此目录及目录下所有文件的大小之和
    ```
- `文件系统上的其它概念`
    ```
    存储空间：
        元数据区：
            元数据：存放文件的大小，时间戳，权限，属主、属组以及对应的数据存储在哪些磁盘块上
                注：文件名虽然是元数据,但并不包含在Inode当中
            Inode: Index Node, 索引节点
                每一个能存储单个文件的所有属性信息(元数据)并按照特定格式组织的这么一个存储空间,被称之为一个Inode
                每个文件系统上可能有N个Inode,为了方便引用某一个Inode,会给Inode进行编号,每个inode都有其编号,可通过ls -i来查看
            地址指针：
                直接指针：直接指向几个磁盘块，由于Inode在指向Block(引用Block编号)的空间是有限的，这样引用的块的数量较少
                间接指针：Inode存储的是指向另一块的区域,这样能引用更多的Block
                三级指针：
        数据区：
            数据：
    -----------------------------------
    bitmap：位图索引
        inode bitmap
            对位标识每个inode空闲与否的状态信息
        block bitmap
        
        所以每个文件系统格式化以后,就分了n个block,并将这些block分成了N个块组,块组中包含元数据区和数据区,元数据区存放了Inode 
        table(存放Inode)以及为Inode和Block做索引的位图,而每个块组中的布局结构信息信息是保存在各自块组的GDT(块组描述符)当中
            包括块组从第几个块开始到第几个块结束,其中哪一个块存的是Inode位图,哪一个块存的是Block位图,哪些块存的是Inode table的,剩余的哪些块是用来存放数据的
    -----------------------------------
    目录也是一个文件,在磁盘上也占据磁盘块,也包括元数据和数据,数据区存储的是（直接附属于此目录）文件名以及文件名对应的Inode编号
        相当于前面所说的"路径映射"
    -----------------------------------
    链接文件：
        硬链接：指向同一个inode的多个不同路径。创建文件的硬链接即为为inode创建新的引用路径，因此会增加其引用计数。
            不能够对目录进行,容易导致循环引用;但目录也是有硬链接的,不过是文件系统自带的固有属性(.和..)
            不能跨分区进行；
            
            通俗的讲就是两个路径的文件名指向同一个inode，此时，一个文件就称为另一个文件的硬链接
        软链接(符号链接)：指向的是另一个文件的路径。其链接文件的大小为指向的路径字符串的长度，不增加或减少目标文件inode的引用计数。
            能够对目录进行；
            能跨分区进行；
    # ln [-sv] SRC DEST   将源文件链接至哪个地方
        -s：symbolic link，不加-s就是硬链接,加-s就是创建符号链接
        -v: verbose，显示过程
    -----------------------------------
    文件管理操作对文件的影响：
        文件删除：inode被标记为空闲(可用状态从1改为0)，此inode指向的磁盘块被标记为空闲(可用状态从1改为0)。
            如果inode被引用了多次(Inode内部有计数器)，且此次删除未使得其引用计数降为0的话，Inode是不会被标记为未用的,这意味着文件被删除仅删除了一个访问路径。
        文件复制：创建一个新的空文件，并将原文件中的所有数据在新文件指向的磁盘块中再写一次的过程。
            所以这个操作包括读出数据,再写入数据的这么一个过程,所以速度比较慢,即使是在同一个分区中进行。
        文件移动：如果在同一个分区移动,移动文件仅是改变了文件访问路径；如果是跨分区移动,在新分区创建文件，从原分区上把数据复制过去，删除原分区数据。
            所以同分区移动速度快;跨分区移动速度慢,跟文件复制没太大区别。
    练习：
    -----------------------------------
        1、创建一个20G的文件系统，块大小为2048，文件系统ext4，卷标为TEST，要求此分区开机后自动挂载至/testing目录，且默认有acl挂载选项；
            (1) 创建20G分区：
            (2) 格式化：
                mke2fs -t ext4 -b 2048 -L 'TEST' /dev/DEVICE
            (3) 编辑/etc/fstab文件
                LABEL='TEST' 	/testing 	ext4 	defaults,acl 	0 0
            (4) 测试
                mount
        2、写一个脚本，完成如下功能：
            (1) 列出当前系统识别到的所有磁盘设备；
                fdisk -l /dev/[hs]da
            (2) 如磁盘数量为1，则显示其空间使用信息；否则，则显示最后一个磁盘上的空间使用信息；
                disks=$(fdisk -l $(fdisk -l /dev/[sh]d[a-z] | grep -o "^Disk /dev/[sh]d[a-z]" | wc -l)
                if [ $disks -eq 1 ]; then 
                    fdisk -l /dev/[hs]da
                else 
                    fdisk -l $(fdisk -l /dev/[sh]d[a-z] | grep -o "^Disk /dev/[sh]d[a-z]" | tail -1 | cut -d' ' -f2)
                fi
    ```
- `btrfs文件系统管理与应用`
    ```
    概述：
        btrfs(B-tree FS,Butter FS,Better FS)在CentOS7上是技术预览版.采用GPL授权,由Oracle在2007年研发的64位FS.
        主要设计目标是用来取代Linux的ext3/4系列FS.
    核心特性：
        多物理卷支持：支持将多个底层物理设备组织成同一个FS来直接使用；原生支持RAID功能，以及联机“添加”、“移除”、“修改”物理卷；
        写时复制更新机制(CoW)：修改文件时先复制一份,对目标新复制的一份进行更新,将文件名指向的原空间改为指向新空间,万一改错了还能恢复更新前的文件；
            这种机制在需要快速修复文件的场景中是极其有用的
        数据及元数据校验码机制(checksum)：存储文件时将数据以及元数据的校验码通过文件某些属性来保存下来,读取文件时方便快速去检测该文件是否受损了并进行自动修复；
            这种机制极大的保证了数据的可靠性
        子卷(sub_volume)：在一个卷上可以创建多个子卷,每一个子卷单独使用和挂载；
        快照：子卷的一个非完全副本,基于Cow机制来实现,与LVM中的快照基本相同,但它比LVM有个增强的特性(增量快照机制),支持快照的快照；
        透明压缩机制：当存储文件时想节约空间,能自动能够通过占据cpu的时钟周期来完成数据压缩以后存储,对用户来讲是不知道的,在读取时又能自动解压缩；
            这种机制会消耗更多的cpu时钟周期
    ```
    ```
    现在的工具大多数都是将多个命令(或功能)合并成一个命令,而后将每一个功能使用子命令来实现,btrfs命令就是这样的
    --------------------------------------
    文件系统创建：
        mkfs.btrfs [options...] <devices...>
            -L 'LABEL': 指定卷标
            -d <type>: 指明数据是如何存放的,支持raid0, raid1, raid5, raid6, raid10, single
            -m <profile>: 指明元数据是如何存放的,支持raid0, raid1, raid5, raid6, raid10, single, dup
            -O <feature>: 在格式化时直接开启btrfs的哪些特性,老版本内核未必支持所有特性
                -O list-all: 列出支持的所有feature；
        eg：mkfs.btrfs -L mydata /dev/sdb /dev/sdc
    文件系统挂载：
        mount -t btrfs /dev/sdb MOUNT_POINT
        
        透明压缩机制：mount命令在CentOS上支持的子选项'compress'
            mount -o compress={lzo|zlib} DEVICE MOUNT_POINT
            
        单独挂载btrfs的子卷时,需要先将btrfs的父卷umount,再挂载子卷;只挂载父卷时,子卷中的数据通过父卷后面的路径都能访问到
            umount /mydata
            mount -o subvol=logs /dev/sdb /mnt
            
            因此在CentOS7上指明要安装btrfs时,可以在btrfs上创建多个子卷,一个叫做root,用来挂载/,一个叫做usr,用来挂载/usr等等,那个时候可以只用来挂载子卷而不挂载父卷.

    将ext系列文件系统转换成btrfs：
        # btrfs-convert /dev/sdd1
    将btrfs重新降级回ext系列FS：     
        # umount /dev/sdd1
        # btrfs-convert -r /dev/sdd1
    --------------------------------------
    btrfs常用子命令：可用'man btrfs-子命令'查看说明
        btrfs subvolume：管理btrfs当中的子卷
            create：创建子卷,而非创建子目录
                eg：# btrfs subvolume create /mydata/logs
            delete：可通过其路径删除子卷,前提时先要挂载父卷
                eg：# btrfs subvolume delete /mydata/logs
            list：列出btrfs当中的所有子卷
                eg：# btrfs subvolume list /
                eg：# btrfs subvolume list /mydata
            show：查看btrfs当中某个子卷的详细信息
            snapshot：针对btrfs当中的子卷创建快照,快照卷与子卷必须在同一个父卷中
                eg：# btrfs subvolume snapshot /mydata/logs /mydata/logs_snapshot
                eg：# btrfs subvolume delete /mydata/logs_snapshot
        btrfs filesystem：管理btrfs自己的
            df：查看已挂载的btrfs的空间使用率情况
            show：查看已有的btrfs属性信息
            sync：将这个文件系统缓存的数据都同步到磁盘上去
            resize：调整大小
            label：指明或显示卷标
        btrfs device：管理btrfs的底层设备
            add：新增物理卷
            delete：移除物理卷,不会影响其中原有的数据,拆除时会自动移动这个设备上的数据其他设备上去,比LVM2强大
        btrfs balance：管理btrfs的数据均衡操作(重新均衡数据)
            start：启动balance
                -dconvert=single：修改数据的存储类型,移除物理卷时需要先做此操作
                -mconvert=raid1：修改元数据的存储类型,移除物理卷时需要先做此操作
            pause：暂停
            resume：继续
            status：
    ```

### RAID 基础原理

```
RAID全称Redundant Arrays of Inexpensive Drives,廉价冗余磁盘阵列
                           Independent 独立冗余磁盘阵列
Berkeley: A case for Redundant Arrays of Inexpensive Disks RAID
    这篇论文阐述了将多块廉价的硬盘(IDE接口)按照某种特别的结构给组织起来当作一个设备来使用,从而能够根据不同的组合方式来提高其IO能力或者提高其耐用性。

    提高IO能力：
        磁盘并行读写;将来去买RAID控制器时,尤其是带内存的,要检查有没有供电功能
    提高耐用性：
        磁盘冗余来实现
        不会因为磁盘损坏而导致业务终止或数据丢失,但是人为的损坏丢失跟磁盘损坏没关系,该备份的策略一个也不能少

    RAID实现的方式：
        外接式磁盘阵列(硬件实现)：通过扩展卡提供适配能力,在BIOS当中实现
        内接式RAID(硬件实现)：主板集成RAID控制器,在BIOS当中实现
        Software RAID：主机压根没有硬件设备支持,可通过软件的方式实现RAID
            在OS中看到的是多个基本的磁盘设备、磁盘分区等，而后将这多个块设备可以组织一个单独的设备使用，即为软RAID
```
```
级别：level,多块磁盘组织在一起的工作方式有所不同
    单一类型:
        RAID-0：0, 条带卷，strip; 将文件切割成chunk,分散到各个磁盘上
            读、写性能提升；
            无容错能力
            可用空间：N*min(S1,S2,...)
            最少磁盘数：2, 2+
        RAID-1: 1, 镜像卷，mirror;将文件切割成chunk,每个磁盘都要保存
            读性能提升、写性能略有下降；
            有冗余能力
            可用空间：1*min(S1,S2,...)
            最少磁盘数：2, 2+
        ...
        RAID-4: 至少3块盘,将文件chunk,第一块盘存chunk1,第二块盘存chunk2,第三块盘存前2块盘的chunk的校验码(异或操作:1101, 0110, 1011)
            读、写性能提升；
            有容错能力（最多坏一块磁盘）
            可用空间：(N-1)*min(S1,S2,...)
            最少磁盘数：3, 3+
        RAID-5：由于RAID-4有一块盘是做为集中校验盘,有可能会有性能瓶颈存在,所以在RAID-4的基础上改进,采用轮流做校验盘的机制
            读、写性能提升；
            有容错能力（最多坏一块磁盘）
            可用空间：(N-1)*min(S1,S2,...)
            最少磁盘数：3, 3+
        RAID-6: 在RAID-5的基础上,采用2块盘作为循环校验盘
            读、写性能提升；
            有容错能力（最多坏两块磁盘）
            可用空间：(N-2)*min(S1,S2,...)
            最少磁盘数：4, 4+
    混合类型: 
        RAID-10: 先将所有硬盘两两一组作为镜像卷,再在此基础上做成条带卷
            读、写性能提升；
            有容错能力：每组镜像最多只能坏一块；
            可用空间：N*min(S1,S2,...)/2
            最少磁盘数：4, 4+
        RAID-01: 先将所有硬盘分为2组,每组做成条带卷,再在此基础上做成镜像卷
        RAID-50:
        RAID-7: 某个公司的独有技术,是一个文件服务器,IO能力非常好,价格非常贵
        JBOD：Just a Bunch Of Disks
            功能：将多块磁盘的空间合并成一个大的连续空间使用；
            可用空间：sum(S1,S2,...)
    常用级别：RAID-0, RAID-1, RAID-5, RAID-10, RAID-50, JBOD
```
```
CentOS 6上的软件RAID的实现
    结合内核中的md(multi devices)模块
         md: 支持将任何块设备组织成RAID

    mdadm：模式化的工具
        命令的语法格式：mdadm [mode] <raiddevice> [options] <component-devices>
            <raiddevice>: /dev/md#,md设备的设备文件，一般在/dev目录下，以md开头，后跟一个数字来区别
            <component-devices>: 任意块设备,可以是整个分区或者整个磁盘
        支持的RAID级别：LINEAR(JBOD), RAID0, RAID1, RAID4, RAID5, RAID6, RAID10等等; 

        模式：
            创建：-C，创建RAID
            装配: -A，重新识别此前实现的RAID
                在某OS上创建的软件RAID，被迁移到其它主机上，并启动OS之后，Linux auto detect
            监控: -F
            管理：-f, -r, -a
        查看当前系统上所有已启用的软件RAID设备及其相关信息(观察md的状态): 
            # cat /proc/mdsta

        创建模式: -C,需先将块设备的分区ID调整为'fd'(Linux raid auto)
            -n #: 使用#个块设备来创建此RAID；
            -l #：指明要创建的RAID的级别；
            -a {yes|no}：自动创建目标RAID设备的设备文件；
            -c CHUNK_SIZE: 指明块大小；
            -x #: 指明空闲盘的个数；
            
            例如：创建一个10G可用空间的RAID0；
                # mdadm -C /dev/md0 -a yes -n 2 -l 0 /dev/sdb{1,2}
            例如：创建一个10G可用空间的RAID5；
                3*5G，6*2G：(n-1)*2G
                # mdadm -C /dev/md1 -a yes -n 3 -l 5 -x 1 /dev/sda{3,5,6} /dev/sdb3
                # cat /proc/mdsta
                # mke2fs -t ext4 /dev/md1
                # mount /dev/md1 /mydata
                # mount
                # df -lh
        管理模式：
            -f: 标记指定磁盘为损坏
                # mdadm /dev/md1 -f /dev/sda3
                # watch -n1 "cat /proc/mdsta"
                # mdadm -D /dev/md1
            -a: 添加磁盘
                # mdadm /dev/md1 -a /dev/sda3
            -r: 移除磁盘
                # mdadm /dev/md1 -r /dev/sda3
        显示指定的软RAID的详细信息：-D
            # mdadm -D /dev/md#
        停止md设备：-S
            # mdadm -S /dev/md#
        重新启用RAID：
            # mdadm -A /dev/md# /dev/DEVICE...
            mdadm的配置文件/etc/mdadm.conf
```

### LVM2
    
```
LVM: Logical Volume Manager(逻辑卷管理器)， Version: 2
    与软RAID很类似,使用纯软件的方式组织一个或多个底层硬件设备为一个抽象的逻辑设备来使用的解决方案
    主要作用是支持动态扩展或收缩,以及支持快照功能
结合内核中的dm(device mapper)模块
     dm: 将一个或多个底层块设备组织成一个逻辑设备的模块；
原理：
    1. 将底层硬盘设备(磁盘或者分区或者RAID)在逻辑卷的架构中创建成PV(物理卷)
    2. 将PV中所提供的存储空间划分成大小相同的N个存储单元PE(Physical Extent,物理盘区),并将存储单元合并在一个高层上,形成一个组件VG(卷组),卷组可以有多个物理卷
        注：PE大小是在PV加入到VG之后决定的;VG的大小也可以通过增加PV来扩展或收缩
    3. VG有点类似于扩展分区,没办法直接格式化使用,需要再次划分成逻辑分区
    4. 在VG的基础上再次创建多个LV(逻辑卷)组件,这才是真正意义上的逻辑卷,每一个LV都是一个独立的文件系统,可以被格式化挂载使用
        即指定特定数量的PE来组成一个逻辑存储空间的过程,逻辑卷的大小可以动态扩展或收缩而不影响到里面原先的数据
        一旦PE分配给逻辑卷使用,此时PE叫做LE,
    --------------------------------
    所以在安装红帽系列OS并选择自动创建分区时,它首先创建一个boot分区,将其他剩余的空间统统创建为一个PV,再将这个PV创建为一个VG,在VG当中创建多个LV,
    一个LV用来当/,一个LV用来当SWAP,一个LV用来当USR,一个LV用来当VAR等等
LV的访问路径：
    1. /dev/mapper/VG_NAME-LV_NAME
		/dev/mapper/vol0-root
	2. /dev/VG_NAME/LV_NAME
		/dev/vol0/root
    此两者均为符号链接，指向的文件为/dev/dm-#
```
```
pv管理工具：pvcreate, pvs, pvdisplay, pvremove, pvmove, pvscan
    # pvs：显示pv的简要信息
    # pvdisplay：显示pv的详细信息

    # pvcreate /dev/DEVICE: 创建pv
        需先将块设备的分区ID调整为'8e'(Linux LVM)
    # pvremove
        先做vgremove
vg管理工具：vgcreate, vgs, vgdisplay, vgremove, vgextend, vgreduce, vgscan
    # vgs
    # vgdisplay

    # vgcreate  [-s #[kKmMgGtTpPeE]] VolumeGroupName  PhysicalDevicePath [PhysicalDevicePath...]
    # vgextend  VolumeGroupName  PhysicalDevicePath [PhysicalDevicePath...]
    # vgreduce  VolumeGroupName  PhysicalDevicePath [PhysicalDevicePath...]
        先做pvmove,将pv上的pe移动至同一个卷组当中的其他pv上去
    # vgremove
        先做lvremove
lv管理工具：lvcreate, lvs, lvdisplay, lvremove, lvextend, lvreduce, lvscan, lvmconf
    # lvs
    # lvdisplay

    # lvcreate -L #[kKmMgGtTpPeE] -n LV_NAME VolumeGroupName
    # lvremove /dev/VG_NAME/LV_NAME
        如果已经挂载过,需要先卸载再remove
    
    扩展逻辑卷：逻辑卷支持在线修改大小
        # lvextend -L [+]#[kKmMgGtTpPeE] /dev/VG_NAME/LV_NAME
            扩展物理边界
            先确定扩展的目标大小；并确保对应的卷组中有足够的空闲空间可用；如原大小为2G, 目标为4G,则可写为'-L +2G'或者'-L 4G'
        # resize2fs /dev/VG_NAME/LV_NAME
            扩展逻辑边界
            重新修改逻辑卷的大小,没指定大小即使用这个分区上所有的可用大小
    缩减逻辑卷：缩减很危险！！！！,所以缩减要离线
        # umount /dev/VG_NAME/LV_NAME
            先卸载文件系统
        # e2fsck -f /dev/VG_NAME/LV_NAME
            执行强制检测并修复
        # resize2fs /dev/VG_NAME/LV_NAME #[mMgGtT]
            缩减逻辑边界
            先确定缩减后的目标大小；并确保对应的目标逻辑卷大小中有足够的空间可容纳原有所有数据；
        # lvreduce -L [-]#[kKmMgGtTpPeE] /dev/VG_NAME/LV_NAME
            缩减物理边界
        # mount /dev/VG_NAME/LV_NAME MOUNT_POINT
            重新挂载
            
    创建快照卷：snapshot
        工作机制：
            快照卷刚创建时没有任何数据,只是将逻辑卷数据的元数据备份一份,将快照卷挂载后,通过快照卷也能访问原卷中的数据,所以快照卷其实是
            原卷的另一个访问入口而已；同时立即创建逻辑卷元数据的一个监视器,随时监控着对方的元数据, 当监控到某一刻某个文件要变化
            时,先将这个文件复制一份到快照卷上,然后原卷的数据再变化,将来通过快照卷访问数据时,没变化的仍通过原卷找文件,而发生变化的则访问
            本卷中的数据;所以这样使得快照卷的空间体积很小,仅存储了快照创建以后原卷中发生变化的文件,没变化的仍找原卷.
        注：快照卷是对某逻辑卷进行的，因此必须跟目标逻辑卷在同一个卷组中（类似于硬链接不能跨分区进行）；无须指明卷组；
        
        # lvcreate -s -L #[kKmMgGtTpPeE] -n snapshot_lv_name -p r original_lv_name
            eg：lvcreate -s -L 512M -n mylv-snap -p r /dev/myvg/mylv
        # mount /dev/myvg/mylv-snap /mnt
```

### 程序包管理

```
API：Application Programming Interface
    POSIX：Portable(可移植) OS
    
    程序源代码(C, C++, perl等) --> 预处理 --> 编译(gcc) --> 汇编(将目标代码转换成二进制指令的过程) --> 链接 --> 执行
        动态(共享)编译：程序研发时调用了库函数，程序单独编译完后，明确指明库的调用入口(例如/lib或/lib64下的.so文件)，程序要运行必要要把库装在进内存
            能够大大节约内存资源
        静态编译：程序在编译时直接把要调用的库复制到程序内部来，程序体积比动态编译体积大
            程序挪到任何系统上都能运行，缺点是对内存和体积都不利
ABI：Application Binary Interface
    Windows与Linux不兼容,但可以使用库级别的虚拟化让二者兼容起来.
    库级别的虚拟化：能够使Windows系统上的应用程序(exe)运行在Linux系统上(elf),反之亦然.
        Linux: WINE(能够虚拟windows运行环境)
        Windows: Cywin(能够虚拟linux运行环境)
虽然通过库级别的虚拟化技术能使程序在多平台上运行,但库可能是个小众产品,其次稳定性还有待考证,因此并不建议这样使用;但很多时候应用程序在一次编译以后
期望能在各种平台上运行,可以提供更高级别的应用级别接口,例如JAVA的JVM. 
    这样用JAVA语言写的程序就不用直接跑在系统调用/库调用之上,只要跑在JVM之上就能执行.
因此应用程序的开发可分为：
    系统级开发：面对系统调用/库调用来实现,linux内核是由99%的c和1%的汇编写成的
        C,C++,
    应用级开发：面对特定应用程序自己专有的开发环境来实现
        Java,Python,Ruby,Php,Perl
```
```
linux应用程序的组成部分：二进制程序、库文件、配置文件、帮助文件
	二进制程序：可执行文件,一个应用程序可有多个二进制程序
		/bin,/sbin,/usr/bin,/usr/sbin,/usr/local/bin,/usr/local/sbin
	库文件：可执行文件,不能独立执行，只能被能独立运行的程序调用时执行,一般有2类,一类是多个程序之间的共享库,一类是自己能够被其他人做二次开发时调用的接口
		/lib,/lib64,/usr/lib,/usr/lib64,/usr/local/lib,/usr/local/lib64
	配置文件：可被查看其内容的文件
		/etc,/etc/DIRECTORY,/usr/local/etc
	帮助文件：可被查看其内容的文件
		/usr/share/man,/usr/share/doc,/usr/local/share/man,/usr/local/share/doc
	--------------------	
    查看二进制程序所依赖的库文件：
        # ldd /PATH/TO/BINARY_FILE
    管理及查看本机装载的库文件：
        # ldconfig 
            /sbin/ldconfig -p: 显示本机已经缓存的所有可用库文件名及文件路径映射关系；
        配置文件为：/etc/ld.so.conf, /etc/ld.so.conf.d/*.conf
        缓存文件：/etc/ld.so.cache
程序包管理器：
    将编译好的应用程序的各组成文件(包括二进制程序,库文件,配置文件,帮助文件)打包成一个或几个程序包文件，从而方便快捷地实现程序包的安装、卸载、查询、升级和校验等管理操作.
    不同的发行版所用的包管理器是不一样的.
    --------------------
    1、程序的组成清单 (每个包独有)
        文件清单
        安装或卸载时运行的脚本
    2、数据库(公共)
        程序包名称及版本
        依赖关系
        功能性说明
        安装生成的各文件的文件路径及校验码信息
    --------------------
    debian,ubuntu：
        包管理器：dpt
            dpkg-deb/apt-get：deb包管理器前端工具
        包格式：.deb
    rhel,fedora,centos,S.u.S.e：
        包管理器：rpm(Redhat Package Manager),RPM is Package Manager
            yum：rpm包管理器的前端工具(补充和丰富了rpm的功能,而不是取代rpm的)
            zypper: suse上的rpm包管理器的前端管理工具
            dnf: Fedora 22+上的rpm包管理器的前端管理工具
        包格式：.rpm
```
- `rpm包的简介`
    ```
    源代码：
        name-VERSION.tar.{gz,bz2,xz}
            VERSION: major.minor.release
    rpm包命名方式：
        name-VERSION-release.arch.rpm
            VERSION: major.minor.release，同源代码
            release：rpm包自身的发行号，与程序源码的发行号无关，仅用于标识对rpm包不同制作的修订；同时，release还包含此包适用的OS
                OS例如centos5,el7(RHEL7)等等
            arch：适用于的硬件平台
                x86: i386, i486, i586, i686等；
                x86_64: x64, x86_64, amd64
                powerpc: ppc
                noarch: 跟平台无关,依赖于虚拟机
                
             例如：
                bash-4.2.3-3.centos5.x86_64.rpm
                zlib-1.2.7-13.el7.i686.rpm
        一个程序有20个功能：常用功能有8个，特殊A:3个，特殊B：6个，二次开发相关功能：3
            分包机制：拆包
                核心包，主包：命名与源程序一致
                    bash-4.2.3-3.centos7.x86_64.rpm
                子包，支包：
                    bash-a-4.2.3-3.centos7.x86_64.rpm
                    bash-b-4.2.3-3.centos7.x86_64.rpm
                    bash-devel-4.2.3-3.centos7.x86_64.rpm
        包之间：存在依赖关系
            X, Y, Z
    获取rpm程序包的途径：
        (1) 系统发行版的光盘或官方服务器(例如CentOS镜像站点等)；
        (2) 项目官方站点
        (3) 第三方组织：很多第三方机构或个人制作并公开发布许多rpm包
            可靠的途径：来源合法性及完整性可以保证
                Fedora-EPEL
            搜索引擎：尽量不要使用,来源合法性及完整性难以保证
                http://pkgs.org
                http://rpmfind.net
                http://rpm.pbone.net
        (4) 自己制作
    rpm包来源合法性验正及完整性验正：
        完整性验正：SHA256(单向加密)
        来源合法性验正：RSA(公钥加密)
        --------------------
        包的制作者会在包制作完成后,使用单向加密算法计算并提取原始数据的特征码，而后使用自己的私钥加密这段特征码，最后附加在原始数据后面(数字签名)。
        验正过程：
            前提：必须有可靠机制获取到包制作者的公钥；
            1、使用制作者的公钥解密加密的特征码，能解密则意味着来源合法；
            2、使用与制作者同样的意向加密算法提取原始数据的特征码，并与解密出来的特征码作比对，相同，则意味着完整性没问题；
        
        具体做法之一：
            首先在当前系统上导入包的制作者的公钥：
                rpm --import /PATH/FROM/GPG-PUBKEY-FILE
                
                显示所有已经导入的gpg格式的公钥：# rpm -qa gpg-pubkey*
                显示密钥的详细信息：# rpm -qi gpg-pubkey-NAME
            自动检查包：
                安装过程中会自动执行
            手动检查包：
                rpm -K /path/to/package_file
                rpm --checksig /path/to/package_file
                    不检查包完整性：
                        rpm -K --nodigest
                    不检查来源合法性：
                        rpm -K --nosignature
 
            Tips：CentOS 7发行版光盘提供的密钥文件是`RPM-GPG-KEY-CentOS-7`
    管理rpm程序包的方式：
        使用包管理器：rpm
        使用前端工具：yum, dnf
    ```
- `rpm命令的使用`
    ```
    CentOS系统上rpm命令管理程序包：
        安装、升级、查询、卸载、校验、数据库维护(重建)
    ---------------------------
    安装：
        rpm {-i|--install} [install-options] PACKAGE_FILE ...
            -v: verbose
            -vv: 
            -h: 以#显示程序包管理执行进度；每个#表示2%的进度
 
            常用组合选项：-ivh

            [install-options]
                --test: 测试安装，但不真正执行安装过程；dry run模式；
                --nodeps：忽略依赖关系安装；能安装上，但有可能无法运行；
                --replacepkgs: 重新安装；如果原有配置文件作了修改，很有可能不执行替换，而是将应该安装生成的配置文件重命名为.rpmnew；

                --nosignature: 不检查来源合法性；
                --nodigest：不检查包完整性；

                --noscipts：不执行程序包脚本片断；
                    %pre: 安装前脚本； --nopre
                    %post: 安装后脚本； --nopost
                    %preun: 卸载前脚本； --nopreun
                    %postun: 卸载后脚本；  --nopostun
    升级：新版本替换老版本
        rpm {-U|--upgrade} [install-options] PACKAGE_FILE ...
        rpm {-F|--freshen} [install-options] PACKAGE_FILE ...
            upgrage：安装有旧版程序包，则“升级”；如果不存在旧版程序包，则“安装”;
            freeshen：安装有旧版程序包，则“升级”；如果不存在旧版程序包，则不执行升级操作；
 
            常用组合选项：
                1、升级或安装：-Uvh
                2、纯升级：-Fvh

            [install-options]
                --oldpackage：降级；
                --force: 强制升级；
        注意：
            (1) 不要对内核做升级操作,很有可能系统启动不了；Linux支持多内核版本并存，因此，直接安装新版本内核即可；
            (2) 如果原程序包的配置文件安装后曾被修改，升级时，新版本的提供的同一个配置文件并不会直接覆盖老版本的配置文件，而把新版本的文件重命名(FILENAME.rpmnew)后保留；
    查询：基于数据库的
        rpm {-q|--query} [select-options] [query-options]
            查询某包是否安装：# rpm -q package_name...
            [select-options]
                -a: 查询所有已经安装的包,可配合grep进行条件过滤
                -f /path/to/some_file: 查看指定的文件由哪个程序包安装生成
                -p /PATH/TO/PACKAGE_FILE：针对尚未安装的程序包文件做查询操作
     
                --whatprovides CAPABILITY：查询指定的CAPABILITY(功能)由哪个包所提供
                --whatrequires CAPABILITY：查询指定的CAPABILITY(功能)被哪个包所依赖
            [query-options]
                --changelog package_name：查询rpm包制作的changlog
                -c package_name: 查询某包安装生成了哪些配置文件
                -d package_name: 查询某包安装生成了哪些帮助文件
                -i package_name: information,查询包的描述信息
                -l package_name: 查询某包安装生成了哪些文件；
                --scripts package_name：查询程序包自带的脚本片断
                    preinstall：安装前脚本
                    postinstall: 安装后脚本
                    preuninstall: 卸载前脚本
                    postuninstall: 卸载后脚本
                -R: 查询指定的程序包所依赖的CAPABILITY；
                --provides: 列出指定程序包所提供的CAPABILITY；
                
            常用组合选项：
                -qi PACKAGE, -qf FILE, -qc PACKAGE, -ql PACKAGE, -qd PACKAGE
                -qpi PACKAGE_FILE, -qpl PACKAGE_FILE, ...
                -qa
    卸载：
        rpm {-e|--erase} [--allmatches] [--nodeps] [--noscripts] [--notriggers] [--test] PACKAGE_NAME ...
            --nodeps：忽略依赖关系卸载;能卸载，但依赖于此包的程序包可能会运行不正常；
        注意：
            如果包的配置文件安装后曾被改动过，卸载时，此文件将不会卸载，而是被重命名并保留，例如warning: /etc/zprofile saved as /etc/zprofile.rpmsave
    校验：用于检查包安装生成的文件属性是否发生变化
        rpm {-V|--verify} [select-options] [verify-options]
           S file Size differs
           M Mode differs (includes permissions and file type)
           5 digest (formerly MD5 sum) differs
           D Device major/minor number mismatch
           L readLink(2) path mismatch
           U User ownership differs
           G Group ownership differs
           T mTime differs
           P caPabilities differ
        注意：
            某属性无变化，显示为.
    数据库重建：
        数据库目录：/var/lib/rpm

        rpm {--initdb|--rebuilddb}
            initdb: 初始化
                如果事先不存在数据库，则新建之；否则，则不新建,即不执行任何操作；
            rebuilddb：重建
                无论当前存在与否，直接重新创建数据库来覆盖原有的数据库；
    ```
- `yum命令的使用`
    ```
    why：
        rpm包管理器在实现程序管理时会有一些问题,比如必须手动解决依赖关系,非常麻烦,虽然在安装时可以忽略依赖关系,但在使用时可能存在问题.
        所以需要有高于rpm或者deb这种本身底层的管理器,能实现在前端自动解决依赖关系,来完成程序的安装,解决用户的后顾之忧.所以yum等工具出现了.
        但yum虽然解决了依赖关系,但它的工作机制和设计体系是有一定问题的,比如安装过程当中半道终止的话,下次再安装时将会无法解决,它无法分析上次是否安装成功与否.
        因此dnf的出现主要是为了解决yum所面临的这个问题的.
    what：
        yum,全称Yellow dog Updater, Modified
        是一个基于RPM包管理的前端包管理器(工具),能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包.
        它是由Yellow dog研发的,并非是取代rpm的,必须依赖于rpm才能存在的.
    ---------------------------
    yum工作机制：yum自身并非cs架构,而是yum所依赖的这种文件访问机制是cs架构的.
        1.当用户使用yum命令要去安装一个程序包时,即yum客户端接受到安装程序包的指令,
        2.首先会尝试着根据yum配置文件去找指定的远程仓库,以获取仓库的元数据信息,并缓存至本地/var/cache/yum,
        3.yum客户端程序在本地分析元数据文件，并结合本地系统环境(已安装的包)做出要安装的程序包的决策(依赖关系等),
        4.根据决策联系Yum仓库，下载各程序包缓存于本地后，一并进行类似于rpm命令的安装操作.
        5.yum安装完成以后,会自动删除程序包缓存以节约空间,元数据文件一般会存在本地,下次安装时会先对比此文件本地与仓库的校验码,相同则不更新,不同则更新,以节约网络带宽.
    yum仓库：rpm包的文件服务器
        yum仓库的组成：
            1.各个rpm包；
            2.元数据文件(包名、版本信息、各包所包含的文件列表、依赖关系、包分组信息),放置于特定目录(repodata)下.
                centos5: xml, centos6,7: sqlite
                createrepo: 制作yum仓库元数据的工具
        yum仓库的协议：repodata目录所在父目录就是一个可用仓库
            ftp     ftp://server/path/to/repository
            http    http://server/path/to/repository
            nfs 	nfs://server/nfs_path
            file    file:///path/to/repository
        使用Centos镜像光盘搭建本地yum源(仓库)：
            (1) 将默认Yum源备份(可选)
                # mkdir /opt/centos-yum.bak
                # mv /etc/yum.repos.d/* /opt/centos-yum.bak/
            (2) 在虚拟机上挂载CentOS镜像文件至某目录，例如/media/cdrom
                # mount -r -t iso9660 /dev/cdrom /media/cdrom
            (3) 编写repo文件并指向镜像的挂载目录
                # vim /etc/yum.repos.d/local.repo
                [local]
                name=local
                baseurl=file:///media/cdrom
                gpgcheck=0
                enabled=1
            (4) 清除本地缓存
                # yum clean all
            (5) 虽然本地yum源安装软件速度快,但可能有些包没有,所以添加aliyun,163等yum源用作第二考虑yum源
                # cat /etc/redhat-release
                # wget -O /etc/yum/repos.d/CentOS{5,6,7}-Base-aliyun.repo http://mirrors.aliyun.com/repo/Centos-{5,6,7}.repo
                # wget -O /etc/yum/repos.d/CentOS{5,6,7}-Base-163.repo http://mirrors.163.com/.help/CentOS{5,6,7}-Base-163.repo
            (6) 修改Yum源的优先级(当既有本地源又有163,aliyun等源时,希望先用本地源去安装,本地找不到可用的包再使用163,aliyun等源去安装软件)
                (1) 查看系统是否安装了优先级的插件
                    # rpm -qa | grep "yum-plugin-"
                        看看有没有yum-plugin-priorities.noarch插件
                    # yum search yum-plugin-priorities
                        用search查看是否有此插件可用
                (2) 安装yum-plugin-priorities.noarch插件
                    # yum -y install yum-plugin-priorities.noarch
                (3) 查看插件是否启用
                    # cat /etc/yum/pluginconf.d/priorities.conf
                        1为启用,0为禁用
                (4) 修改本地Yum源优先使用
                    # vim /etc/yum.repos.d/local.repo
                        在原基础上加入priority=1,数字越小优先级越高
                        可以继续修改其他源的priority值,经测试仅配置本地源的优先级为priority=1就会优先使用本地源了
        自建yum源：
            (1) 安装httpd程序，并启动服务,并设置开机自启
                # yum install httpd
                # service httpd start
                # chkconfig httpd on
            (2) httpd的文档根目录为/var/www/html,创建子目录，存放某相关的所有rpm包
            (3) 为仓库生成元数据文件，以使能够作为仓库使用
                # yum install createrepo
                # createrepo /path/to/rpm_repo/
            (4) 配置yum客户端使用此仓库即可
    yum客户端：
        配置文件： 
            /etc/yum.conf：主配置文件（中心配置文件）,为所有仓库提供公共配置
                可使用whatis/man yum.conf查看帮助手册
            /etc/yum.repos.d/*.repo：一个或几个相关仓库的配置信息可保存为一个文件，文件名都以.repo结尾,为仓库的指向提供配置
        仓库指向的定义：
            [repositoryID]
            name=Some name for this repository
            baseurl=url://path/to/repository/
            enabled={1|0}
            gpgcheck={1|0}
            gpgkey=URL
            enablegroups={1|0}
            mirrorlist=URL to a file
                mirrorlist  Specifies a URL to a file containing a list of baseurls
            failovermethod={roundrobin|priority}
                默认为：roundrobin，意为随机挑选；
            cost={1..n}
                默认为1000，指定访问此仓库的开销
        yum的repo配置文件中可用的变量：
            $releasever: 程序的版本，对Yum而言指的是redhat-release版本；只替换当前OS的发行版的主版本号; 
            $arch: 系统架构(平台)
            $basearch: 系统基本架构(平台)，如i686，i586等的基本架构为i386；
            $YUM0-9: 在系统中定义的环境变量，可以在yum中使用；
    ```
    ```
    yum客户端命令的使用：
        yum [options] [command] [package ...]
    ---------------------------
    yum的options选项：
        --nogpgcheck：禁止进行gpg check；
        -y: 自动回答为“yes”；
        -q：静默模式；
        --disablerepo=repoidglob：临时禁用此处指定的repo；
        --enablerepo=repoidglob：临时启用此处指定的repo；
        --noplugins：禁用所有插件；
    ---------------------------
    1、列出所有可用repo列表
        yum repolist {enabled|disabled|all}
    2、列出rpm包(可配合less查看)
        yum list {all|installed|available}
        yum list KEYWORD*
    3、查询指定的特性(可以是某文件)是由哪个程序包安装生成的
        yum provides|whatprovides /path/to/somefile
    4、清理本地缓存
        yum clean {all|packages|metadata|expire-cache|rpmdb|plugins}
    5、安装程序包
        yum install package_name [package_name...]
            如果从其它处获得一个rpm包，且此包依赖于其它包（在仓库中）,也可以使用'yum install /path/to/packe_file'来安装本地程序包
        yum reinstall package_name [package_name...]   (重新/覆盖安装)
    6、升级程序包
        yum check-update: 检查可用的升级包
        yum update package_name
        yum downgrade package_name  (降级)
    7、查看程序包的描述信息
        yum info package_name
    8、卸载程序包
        yum remove|erase package_name
    9、以指定的关键字搜索程序包名及summary信息
        yum search string1 [string2] [...]
    10、查看指定包所依赖的capabilities：
        yum deplist package1 [package2] [...]
    11、查看yum事务历史(yum每一次的安装/更新操作都会开启yum事务)
        yum	history [info|list|packages-list|packages-info|summary|addon-info|redo|undo|rollback|new|sync|stats]
    12、包组管理的相关命令
        yum grouplist
        yum groupinfo "GROUP NAME"
        yum groupinstall "GROUP NAME"
        yum groupupdate "GROUP NAME"
        yum groupremove "GROUP NAME"
    ```
- `源程序包编译安装`
    ```
    源代码组织格式：
        多文件：文件中的代码之间，很可能存在跨文件依赖关系；

        C、C++：make (configure --> Makefile.in(makefile文件的模板) --> makefile)
        java：maven
     开源程序源代码的获取：
        官方自建站点：
            apache.org (ASF)
            mariadb.org
            ...
        代码托管：
            SourceForge
            Github.com
            code.google.com
    C代码编译安装三步骤：
        建议：安装前查看INSTALL，README
        前提：提供开发工具及开发环境
            开发工具：make(项目管理工具), gcc(gnu c complier,编译工具)等
                autoconf: 生成configure脚本,即生成编译环境检查及编译功能配置脚本
                automake：生成Makefile.in模板文件
            开发环境：开发库，头文件
                glibc：标准库
            -------------------------    
            通过“包组”提供开发组件
                CentOS 6: "Development Tools", "Server Platform Development",
        (1) ./configure脚本：
            (1) 通过选项传递参数，指定编译特性(启用特性、安装路径等)；执行时会参考用户的指定以及Makefile.in文件生成makefile；
            (2) 检查依赖到的外部环境；
            -------------------------
            ./configure脚本的使用：
                1、获取其支持使用的选项
                    ./configure --help
                2、较通用的一些选项
                    安装路径相关：
                        --prefix=/path/to/somewhere: 指定默认安装位置；默认为/usr/local/
                        --sysconfdir=/path/to/somewhere: 指定配置文件安装路径
                    指定启用/禁用的特性：
                        --enable-FEATURE: 例如--enable-fpm
                        --disable-FEATURE: 例如--disable-socket
                    指定所依赖的功能、程序或文件：
                        --with-FUNCTION[=/path/to/somewhere]
                        --without-FUNCTION
                        --with-PACKAGE[=ARG]
                        --without-PACKAGE
        (2) make：
            根据makefile文件并调用相应的编译器来构建应用程序；
        (3) make install：调用install命令来复制文件到指定位置当中去
    安装后的配置：
        程序运行：
            (1) 导出二进制程序目录至PATH环境变量中,(让二进制程序直接运行，而无须输入路径)
                编辑文件/etc/profile.d/NAME.sh
                    export PATH=/PATH/TO/BIN:$PATH
            (2) 导出帮助手册
                编辑/etc/man.config文件
                    添加一个MANPATH，路径为新安装的程序的man目录
        程序开发：如果其它应用程序依赖此程序的开发环境，或针对此程序做二次开发
            (1) 导出库文件路径
                编辑/etc/ld.so.conf.d/NAME.conf,指定让系统搜索定制的路径
                    添加新的库文件所在目录至此文件中；
                触发系统重新搜索所有的库文件并生成缓存
                    ldconfig [-v]
            (2) 导出头文件
                系统找头文件的路径是：/usr/include
                基于链接的方式实现：
                    ln -sv /usr/local/nginx/include /usr/include/nginx
    ```

## 参考链接

## 结束语

- 未完待续...
