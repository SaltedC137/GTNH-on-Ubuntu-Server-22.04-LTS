

# 安装系统

## 下载镜像
### Ubuntu Server 

<ul class="p-inline-list">
          <li class="p-inline-list__item">
            <a href="/download/server/thank-you?version=24.04.2&amp;architecture=amd64" class="p-button--positive" onclick="dataLayer.push({'event' : 'GAEvent', 'eventCategory' : 'Download', 'eventAction' : 'Server', 'eventLabel' : '24.04 Server-cn', 'eventValue' : undefined });">下载 Ubuntu Server 24.04.2 LTS</a>
          </li>
          <li class="p-inline-list__item"><a href="#downloads">其他下载&nbsp;›</a></li>
          <li class="p-inline-list__item"><a href="#architectures">其他架构&nbsp;›</a></li>
        </ul>

<div class="col-4 p-card">
        <h3 class="p-card__title">
          Ubuntu Server 22.04 LTS
        </h3>
        <p>上一个 Ubuntu Server 长期支持版本，包括承诺的支持，至 2027 年 4 月。</p>
        <p><a class="p-button" href="https://releases.ubuntu.com/22.04/ubuntu-22.04.5-live-server-amd64.iso">下载 Ubuntu Server 22.04.5 LTS</a>
        </p>
      </div>
### Centos 

x86_64
<a class="btn btn-primary" href="https://mirrors.centos.org/mirrorlist?path=/10-stream/BaseOS/x86_64/iso/CentOS-Stream-10-latest-x86_64-dvd1.iso&amp;redirect=1&amp;protocol=https" role="button"><i class="fa-solid fa-up-right-from-square"></i> Mirrors</a>

## 启动盘制作

使用 Rufus 制作启动盘

![[Pasted image 20250805172901.png]]


## 安装说明

进入 GNU 界面选择 Try or Install Ubuntu Server
![[Pasted image 20250805175350.png]]
***
![[Pasted image 20250805175625.png]]
***
![[Pasted image 20250805175654.png]]
***
![[Pasted image 20250805175742.png]]

### IP 设置
![[Pasted image 20250805175835.png]]
***
选择 Manual 将 IP 固定下来
![[Pasted image 20250805175917.png]]
***
镜像源建议改成阿里的源
![[Pasted image 20250805180234.png]]
### 分区设置
![[Pasted image 20250805180348.png]]
![[Pasted image 20250805200000.png]]
#### EFI 系统分区（/boot/efi）
（1）大小：300 MB – 1 GB
（2）类型：主分区
（3）文件系统：FAT 32
（4）挂载点：/boot/efi
（5）说明：
该分区是 UEFI BIOS 引导模式下（搭配 GPT 分区表）Linux 系统（如 Ubuntu）的核心引导分区。其核心参数与特性如下：
文件系统要求：必须格式化为 FAT 32。这是 UEFI 固件的标准要求，因为 FAT 32 具有跨平台兼容性，可被 Windows、Linux、macOS 等主流操作系统识别，确保引导程序能被多系统环境下的 UEFI 固件正确读取。
分区类型标识：在 GPT 分区表中，该分区的类型标识为 EFI 系统分区（类型 GUID：C 12 A 7328-F 81 F-11 D 2-BA 4 B-00 A 0 C 93 EC 93 B），与文件系统类型（FAT 32）是两个独立概念。
大小方面：一般是 300 MB/512 MB 就够了，但如果需要后续引导多系统就要 1 GB 或者大一些。
核心功能：
* 存储 UEFI 引导程序（如 GRUB）、内核镜像及启动配置文件，是系统启动的 “入口”。
* 支持 UEFI 固件直接访问引导文件，确保系统快速启动和安全启动功能正常运行。
操作注意事项：
* 严禁删除或格式化该分区，否则会导致系统启动失败（如出现 “Invalid Partition Table” 错误）。
* 挂载点必须设置为 **/boot/efi**，而非传统 BIOS 模式下的 “/boot”，以确保 UEFI 固件能正确识别引导路径。
* 若手动分区，需使用工具（如 gdisk）明确指定分区类型为 EFI 系统分区，并格式化为 FAT 32。
#### Swap 交换分区（Swap Space）
（1）大小：
常规建议：通常为物理内存的 1-2 倍，但需结合实际场景调整。例如：
* 内存 ≤ 4 GB：建议设置为 4-8 GB；
* 内存 4-16 GB：建议设置为 8-16 GB；
* 内存 ≥ 16 GB：可适当减少（如 2-4 GB），甚至可关闭（但需禁用休眠功能）。
特殊需求：若需启用休眠（Hibernate）功能，Swap 大小需 ≥ 物理内存，以确保休眠文件能完整写入磁盘。
（2）类型：逻辑分区 (如果是 GPT 硬盘则无此概念，直接创建即可)
* MBR 分区表：通常作为逻辑分区存在于扩展分区中（受限于 MBR 的 4 主分区限制）。
* GPT 分区表：可设置为主分区，无主分区数量限制。
分区类型标识：
MBR 中需设置为 Linux Swap（类型代码 8200）；
GPT 中需设置为 Swap 类型 GUID（0657 FD 6 D-A 4 AB-43 C 4-84 E 5-0933 C 84 B 4 F 4 F）。
（3）文件系统：选择“交换空间 (linux swap partition)”
（4）挂载点：（通过系统配置自动激活，无需挂载到目录）
（5）说明：
* 核心功能：当物理内存不足时，Swap 分区作为虚拟内存扩展，临时存储不活跃的内存数据，避免系统因内存耗尽崩溃。
* 设置原则：
* 性能权衡：Swap 空间过大会增加磁盘 I/O 负担，影响系统响应速度；过小则无法有效缓解内存压力。
* 休眠依赖：若需使用休眠功能（将内存状态保存到磁盘），Swap 大小必须 ≥ 物理内存，否则休眠功能无法启用。
分区工具建议：
* MBR 环境下，使用 fdisk 或 parted 工具创建逻辑分区并设置类型为 8200；
* GPT 环境下，使用 gdisk 工具创建主分区并指定 Swap 类型 GUID。
系统配置：
* 通过/etc/fstab 文件自动激活 Swap 分区（例如：/dev/sdaX swap swap defaults 0 0）；
* 可通过 swapon -a 命令手动启用，或 swapoff -a 关闭。
现代趋势：对于内存充足的系统（如 ≥ 16 GB），Swap 分区可适当缩小甚至省略，但需通过 sysctl 调整 swappiness 参数（默认 60，表示内存使用率达 60% 时启用 Swap），以优化内存使用策略。
类型方面：逻辑分区 (如果是 GPT 硬盘则无此概念，直接创建即可)，我这里表达的是传统 Legacy  BIOS 下的 MBR 分区表情况下是逻辑分区；但是 GPT 分区表情况下就没有限制了没有逻辑分区概念了就全是主分区。
#### 根目录 （/）
（1）大小：50 GB – 100 GB（建议至少 50 GB，对于开发者建议 80 GB-100 GB）
最小需求：至少 10 GB（适用于最小化安装的服务器或轻量级系统）。
推荐配置：
个人桌面 / 工作站：20 GB–50 GB（需预留系统更新、软件安装及临时文件空间）。
服务器 / 高负载环境：50 GB 以上（根据日志、数据库等数据量调整，如 Web 服务器建议 100 GB+）。
动态扩展：若使用 LVM（逻辑卷管理），可通过扩容卷组动态调整根目录大小，避免空间不足问题。
（2）类型：逻辑分区 (如果是 GPT 硬盘则无此概念，直接创建即可)
* MBR 分区表：可作为主分区或逻辑分区（受限于 MBR 的 4 主分区限制，通常与扩展分区配合使用）。
* GPT 分区表：直接创建为主分区，无主分区数量限制。
* GPT 类型标识：在 GPT 分区表中，根目录的类型 GUID 为 0 FC 63 DAF-8483-4772-8 E 79-3 D 69 D 8477 DE 4（Linux 文件系统通用类型）。
（3）文件系统：ext 4（兼容性强，稳定性高，适合大多数场景）
其他选项：
* XFS：适合大文件和高吞吐量场景（如数据库服务器）。
* Btrfs：支持快照、校验和及弹性扩展，适合需要高级功能的系统。
* ZFS：提供数据冗余和压缩功能，但需额外内核模块支持。
（4）挂载点：/（固定挂载点，不可更改）
（5）说明：
核心功能：
* 根目录是 Linux 文件系统的起点，包含所有系统文件、用户文件、应用程序及配置文件（如/etc、/usr、/var 等）。
* 直接影响系统稳定性，若空间不足可能导致服务异常（如日志无法写入、软件更新失败）。
分区策略：
独立分区建议：若追求精细化管理，可将/home、/var、/tmp 等目录单独分区，避免根目录被单一目录占满。
LVM 优势：通过 LVM 将根目录创建为逻辑卷，可灵活扩展空间（如添加新硬盘后通过 vgextend 和 lvextend 命令扩容）。
操作注意事项：
* 避免在根目录下直接存储大量用户数据，建议将数据存放在独立分区（如/home）。
* 格式化时需指定正确的文件系统类型（如 mkfs. Ext 4 /dev/sdaX），并通过/etc/fstab 配置自动挂载。
* 若使用 GPT 分区表，需确保根目录分区类型 GUID 正确设置，否则可能导致系统无法识别。
性能优化：
* 对 SSD 硬盘，可启用 discard 选项（在/etc/fstab 中添加 discard 参数）以提升性能并延长寿命。
* 定期清理临时文件（如/tmp 目录），避免空间碎片化。
补充说明：

根目录的大小需根据实际需求动态调整。例如，安装图形界面（如 GNOME/KDE）可能需要额外 5 GB–10 GB 空间；运行容器或虚拟机时，建议预留更多空间。
在多系统环境中，根目录应独立于其他系统分区，避免文件冲突。若使用 LVM，可通过卷组共享物理磁盘资源，但需注意不同系统对 LVM 的支持差异。
####      用户目录（/home）
（1）大小：剩余所有空间
（2）类型：逻辑分区 (如果是 GPT 硬盘则无此概念，直接创建即可)
GPT 分区表：直接创建为主分区（无主分区数量限制，无需依赖扩展分区）。
MBR 分区表：通常作为逻辑分区（受限于 4 主分区限制，需先创建扩展分区后再划分）。
（3）文件系统：ext 4
（4）挂载点：/home
（5）说明：/home 是 Linux 系统中存储用户个人数据的核心分区，包含所有用户的文档、下载文件、桌面数据、浏览器缓存、应用程序配置（如. Bashrc、. Config 目录）等。

将 /home 独立分区的核心价值在于：

数据安全：重装系统或升级根目录（/）时，可直接保留 /home 分区，避免用户数据丢失；
管理灵活：可单独对 /home 进行扩容、备份（如通过 btrfs 快照）或修复，不影响系统核心文件；
权限隔离：用户数据与系统文件分离，降低误操作（如删除系统文件）对个人数据的影响。
尤其是对于作为服务器来说，因为需要经常被访问可能导致系统文件出现问题，而独立分区的/home 出来的用户配置数据不会被“污染”，可以保留下来用于重载系统时用上之前的配置。

### 账户设置
账号密码务必记牢，本服务器搭建旨在通过 Sakura Frp 内网穿透进行，故不需要强密码
![[Pasted image 20250805181256.png]]
### SSH 安装（务必安装）
![[Pasted image 20250805181658.png]]
## SSH 设置 (慎用)
找到密码相关直接关闭
![[Pasted image 20250805182551.png]]
***
![[Pasted image 20250805182730.png]]

### SSH 连接
使用 vscode 进行连接
![[Pasted image 20250805184650.png]]

## 安装 GT_New_Horizons_2.7.4_Server_Java_8

### 下载

<ul>
			<h1>/ServerPacks</h1>
			<li><a href="/">
	<span>📁</span>
	<span>..</span>
	<span></span>
	<span></span></a></li><li><a href="/ServerPacks/betas/">
	<span>📁</span>
	<span>betas</span>
	<span>2025-07-27 11:12:20</span>
	<span></span></a></li>
			<li><a href="/ServerPacks/GT_New_Horizons_2.4.0_Server.zip">
	<span></span>
	<span>GT_New_Horizons_2.4.0_Server. Zip</span>
	<span>2023-09-03 17:15:50</span>
	<span>351.07 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.4.0_Server_Java_17-20.zip">
	<span></span>
	<span>GT_New_Horizons_2.4.0_Server_Java_17-20. Zip</span>
	<span>2023-09-03 17:15:23</span>
	<span>355.29 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.5.0_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.5.0_Server_Java_17-21. Zip</span>
	<span>2023-12-17 22:42:27</span>
	<span>356.26 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.5.0_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.5.0_Server_Java_8. Zip</span>
	<span>2023-12-17 22:42:49</span>
	<span>351.14 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.5.1_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.5.1_Server_Java_17-21. Zip</span>
	<span>2024-04-16 19:02:13</span>
	<span>357.69 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.5.1_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.5.1_Server_Java_8. Zip</span>
	<span>2024-04-16 19:02:43</span>
	<span>352.56 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.6.0_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.6.0_Server_Java_17-21. Zip</span>
	<span>2024-04-28 22:41:16</span>
	<span>392.52 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.6.0_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.6.0_Server_Java_8. Zip</span>
	<span>2024-04-28 22:42:33</span>
	<span>380.74 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.6.1_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.6.1_Server_Java_17-21. Zip</span>
	<span>2024-05-21 22:43:15</span>
	<span>394.06 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.6.1_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.6.1_Server_Java_8. Zip</span>
	<span>2024-05-21 22:43:53</span>
	<span>379.52 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.0_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.0_Server_Java_17-21. Zip</span>
	<span>2024-12-08 11:02:14</span>
	<span>422.06 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.0_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.0_Server_Java_8. Zip</span>
	<span>2024-12-08 11:01:40</span>
	<span>405.46 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.1_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.1_Server_Java_17-21. Zip</span>
	<span>2024-12-20 12:47:22</span>
	<span>422.03 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.1_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.1_Server_Java_8. Zip</span>
	<span>2024-12-20 12:47:51</span>
	<span>405.44 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.2_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.2_Server_Java_17-21. Zip</span>
	<span>2024-12-31 16:20:48</span>
	<span>422.13 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.2_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.2_Server_Java_8. Zip</span>
	<span>2024-12-31 16:20:03</span>
	<span>405.53 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.3_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.3_Server_Java_17-21. Zip</span>
	<span>2025-03-02 15:15:52</span>
	<span>422.10 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.3_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.3_Server_Java_8. Zip</span>
	<span>2025-03-02 15:16:34</span>
	<span>405.49 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.4_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.4_Server_Java_17-21. Zip</span>
	<span>2025-04-13 13:08:52</span>
	<span>422.19 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_2.7.4_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_2.7.4_Server_Java_8. Zip</span>
	<span>2025-04-13 13:09:17</span>
	<span>405.58 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_Aprils_Fool_2025_Edition_Server_Java_17-21.zip">
	<span></span>
	<span>GT_New_Horizons_Aprils_Fool_2025_Edition_Server_Java_17-21. Zip</span>
	<span>2025-04-04 17:40:07</span>
	<span>425.66 MB</span></a></li><li><a href="/ServerPacks/GT_New_Horizons_Aprils_Fool_2025_Edition_Server_Java_8.zip">
	<span></span>
	<span>GT_New_Horizons_Aprils_Fool_2025_Edition_Server_Java_8. Zip</span>
	<span>2025-04-04 17:39:38</span>
	<span>409.05 MB</span></a></li>
		</ul>

### 安装

#### 安装 java 环境
#### **Java环境的搭建**

GregTech:New Horizons客户端以及服务端都基于Minecraft Java Edition 1.7.10进行搭建，而Minecraft Java Edition的运行又需要计算机中的Java环境。所以，我们配置Linux GTNH服务端的时候首先要配置好Java环境。


##### Centos/RHEL:

1. yum -y update
2. yum -y install java-1.8.0-openjdk 


##### Ubuntu/Debian:

1. apt-get update
2. apt-get -y install openjdk-8-jre

  

##### 注意事项：

如有可能请使用新装系统执行，如系统内存在多个版本的Java则需要额外设置环境变量！

通过 Windows 的 scp 命令将包上传至服务器

![[Pasted image 20250805190349.png]]

安装解压工具:

```bash
sudo apt install unzip
```


解压压缩包启动 Server



## 内网穿透设置

提示

这篇指南假设您以 `root` 用户进行安装，请先使用 `sudo -i` 或 `su -` 切换到 root 账户  
如果您不需要安装到系统级，请参考 [Linux (无 Root 桌面)](https://doc.natfrp.com/launcher/usage.html#linux) 标签

### [脚本安装](https://doc.natfrp.com/launcher/usage.html#linux-script-install)

对于使用 systemd 的用户，可使用一键安装脚本快速安装：

```
sudo bash -c ". <(curl -sSL https://doc.natfrp.com/launcher.sh)"

# 或者使用 wget, 脚本会自动通过包管理器安装 curl
sudo bash -c ". <(wget -O- https://doc.natfrp.com/launcher.sh)"

# 如果需要绕过 Docker 检测强制安装到系统中
sudo bash -c ". <(curl -sSL https://doc.natfrp.com/launcher.sh) direct"
```

此脚本于 [PR#526](https://github.com/natfrp/wiki/pull/526) 中由用户 [ssdomei232](https://github.com/ssdomei232) 贡献并经我们修改。

### [脚本卸载](https://doc.natfrp.com/launcher/usage.html#linux-script-uninstall)

对于使用 [一键脚本](https://doc.natfrp.com/launcher/usage.html#linux-script-install) 安装的用户，可以使用脚本的卸载功能来快速卸载：

```
sudo bash -c ". <(curl -sSL https://doc.natfrp.com/launcher.sh) uninstall"
```

### [手动安装步骤](https://doc.natfrp.com/launcher/usage.html#linux-server-manual)

注意

手动安装流程 **操作复杂，不适合新手使用**，如非特殊情况，请务必 **使用自动安装脚本** 或 [Docker](https://doc.natfrp.com/launcher/usage.html#docker) 进行安装

1. 由我们分发的压缩包采用 [zstd](https://github.com/facebook/zstd) 进行压缩，如果您还没有 `zstd`，请先在系统上安装。
    
2. 出于安全考虑，`natfrp-service` 默认不允许以 root 权限运行，创建一个新账户：
    
    ```
    useradd -r -m -s /sbin/nologin natfrp
    ```
    
3. 下载由我们分发的 `.tar.zst` 文件，将其安装到系统中：
    
    ```
    # 您可以将其安装到任意目录，这里直接装到 HOME 是为了简化操作
    # 对路径出警的 Issue 或 PR 可能不会得到处理
    cd /home/natfrp/
    
    # 打开 https://www.natfrp.com/tunnel/download
    # 复制对应的架构的 Linux / FreeBSD 启动器链接并下载
    # curl -LO https://nya.globalslb.net/natfrp/client/launcher-unix/....
    
    # 解压
    tar -I zstd -xvf natfrp-service_*.tar.zst
    rm natfrp-service_*.tar.zst
    
    # 设置权限
    chmod +x frpc natfrp-service
    chown natfrp:natfrp frpc natfrp-service .
    ```
    
4. 参考发行版的相关教程配置您的初始化系统来启动 `natfrp-service --daemon`。
    
    以 systemd 为例，建立一个 Unit 文件即可。如果需要进行高级配置请参考 [启动器用户手册](https://doc.natfrp.com/launcher/manual.html)。
    
    这是一个简单的示例文件，您可以直接把它复制到 `/etc/systemd/system/natfrp.service`：
    
    ```
    [Unit]
    Description=SakuraFrp Launcher
    After=network.target
    
    [Service]
    User=natfrp
    Group=natfrp
    
    Type=simple
    TimeoutStopSec=20
    
    Restart=always
    RestartSec=5s
    
    ExecStart=/home/natfrp/natfrp-service --daemon
    
    [Install]
    WantedBy=multi-user.target
    ```
    
5. 配置完成后，启动并停止一次 `natfrp-service` 来生成配置文件：
    
    ```
    # 请确保您的工作目录正确
    cd /home/natfrp/
    
    systemctl start natfrp.service
    sleep 3
    systemctl stop natfrp.service
    
    # 确认 config.json 已生成
    ls -ls .config/natfrp-service/
    ```
    
6. 修改配置文件，填入访问密钥。同时另准备一个启动器远程管理密码：
    
    ```
    # 请确保您的工作目录正确
    cd /home/natfrp/
    
    # 生成处理后的远程管理密码，复制输出的 Base64 字符串备用
    # 注意命令中的启动器远程管理密码是您自己在此处设定的
    # 注意如果结尾有等号 (=) 出现，请一起复制，它们也是 Base64 的一部分
    ./natfrp-service remote-kdf <您的启动器远程管理密码>
    
    # 编辑配置文件, 以 vim 为例
    vim .config/natfrp-service/config.json
    ```
    
    找到并修改 `token`、`remote_management`、`remote_management_key` 三项：
    
    ```
    {
       "token": "SakuraFrp 访问密钥，在管理面板找到",
       "remote_management": true,
       "remote_management_key": "处理后的远程管理密码",
    
       "log_stdout": true, // 推荐开启 log_stdout 让 systemd 管理日志
    }
    ```
    
7. 启用服务
    
    ```
    systemctl enable --now natfrp.service
    systemctl status natfrp.service
    
    # 查看日志，确认看到 "远程管理连接成功" 的输出
    journalctl -u natfrp.service -f
    ```
    
8. 最后，打开 [远程管理](https://www.natfrp.com/remote/v2)，连接您的服务器并启用隧道：
    
    ![](https://doc.natfrp.com/assets/remote-v2-connect-2-m5wm_fJv.png)
    

### [手动卸载步骤](https://doc.natfrp.com/launcher/usage.html#linux-manual-uninstall)

如果您是按照教程安装的，把安装过程中进行的操作反过来进行一遍即可。

```
# 停用并删除 Unit 文件
systemctl disable --now natfrp.service
rm /etc/systemd/system/natfrp.service

# 删除用户和 HOME 目录
userdel -r natfrp
```


## 密码设置

```bash
# 启用空密码登录
PermitEmptyPasswords yes

# 启用密码认证
PasswordAuthentication yes

# 禁用公钥认证
PubkeyAuthentication no

# 允许root登录（如果使用root）
PermitRootLogin yes

```

设置完成后建议 reboot 重启

```bash
# 创建新会话
tmux new -s minecraft

# 启动服务器
java -Xmx1024M -jar minecraft_server.jar

# 分离会话
Ctrl+B → D

# 重新连接
tmux attach -t minecraft

```

## Tmux 命令使用手册

Tmux（Terminal Multiplexer）是一个强大的终端复用工具，允许你在单个终端窗口中管理多个终端会话。以下是详细的使用指南：

---

### **基础操作**
| 快捷键 | 功能描述 |
|--------|----------|
| `Ctrl+b` | Tmux 命令前缀（所有操作先按此组合） |
| `Ctrl+b ?` | 查看所有快捷键帮助（按 `q` 退出） |
| `Ctrl+b d` | 分离当前会话（会话后台运行） |
| `Ctrl+b :` | 进入命令模式（输入高级命令） |
| `exit` 或 `Ctrl+d` | 关闭当前窗格/窗口 |

---

### **会话管理**
| 命令 | 功能 |
|------|------|
| `tmux` | 创建新会话（无名） |
| `tmux new -s <name>` | 创建命名会话 |
| `tmux ls` | 列出所有会话 |
| `tmux attach -t <name>` | 连接指定会话 |
| `tmux kill-session -t <name>` | 终止指定会话 |
| `Ctrl+b $` | 重命名当前会话 |

---

### **窗口管理**
| 快捷键 | 功能 |
|--------|------|
| `Ctrl+b c` | 创建新窗口 |
| `Ctrl+b &` | 关闭当前窗口 |
| `Ctrl+b p` | 切换到上一个窗口 |
| `Ctrl+b n` | 切换到下一个窗口 |
| `Ctrl+b <0-9>` | 切换到指定编号窗口 |
| `Ctrl+b ,` | 重命名当前窗口 |
| `Ctrl+b w` | 窗口列表（方向键选择） |

---

### **窗格管理**
| 快捷键 | 功能 |
|--------|------|
| `Ctrl+b "` | **水平分割**窗格 |
| `Ctrl+b %` | **垂直分割**窗格 |
| `Ctrl+b 方向键` | 切换窗格 |
| `Ctrl+b z` | 最大化/恢复当前窗格 |
| `Ctrl+b x` | 关闭当前窗格 |
| `Ctrl+b Space` | 切换窗格布局 |
| `Ctrl+b Alt+方向键` | 调整窗格大小 |
| `Ctrl+b {` | 向左交换窗格 |
| `Ctrl+b }` | 向右交换窗格 |

---

### **高级功能**
1. **复制模式**
   - `Ctrl+b [`：进入复制模式
   - 方向键移动光标，`Space` 开始选择，`Enter` 复制
   - `Ctrl+b ]`：粘贴内容

2. **会话共享**
   ```bash
   # 用户A创建会话
   tmux new -s shared
   
   # 用户B加入会话
   tmux attach -t shared
   ```

3. **保存会话**
   ```bash
   # 保存所有会话
   tmuxp freeze > all_sessions.yaml
   
   # 恢复会话
   tmuxp load all_sessions.yaml
   ```

4. **鼠标支持（在 `~/.tmux.conf` 添加）**
   ```ini
   set -g mouse on
   bind -n WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
   ```

---

### **配置文件示例（~/.tmux.conf）**
```ini
# 基础设置
set -g base-index 1          # 窗口编号从1开始
set -g renumber-windows on   # 关闭窗口后重新编号

# 状态栏优化
set -g status-bg colour235
set -g status-right "%Y-%m-%d %H:%M"

# 快捷键重映射
unbind '"'                   # 取消默认水平分割
bind | split-window -h       # 用 | 垂直分割
bind - split-window -v       # 用 - 水平分割

# 启用真彩色
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",*256col*:Tc"
```

---

### **常用场景示例**
1. **开发环境部署**
   ```bash
   tmux new -s dev
   # 窗口1: 代码编辑
   # 窗口2: 服务运行 (Ctrl+b c 创建)
   # 窗格: 水平分割监控日志 (Ctrl+b " 分割)
   ```

2. **长时间任务**
   ```bash
   tmux new -s backup
   tar -czvf backup.tar.gz /data
   # 按 Ctrl+b d 分离，任务后台继续运行
   # 恢复查看: tmux attach -t backup
   ```

3. **多服务器监控**
   ```bash
   tmux new -s servers
   # 垂直分割窗格 (Ctrl+b %)
   ssh user@server1  # 左窗格
   ssh user@server2  # 右窗格
   ```

---

### **实用技巧**
1. **快速创建窗格组**
   ```bash
   tmux split-window -h 'htop'  # 右侧运行htop
   tmux split-window -v 'nvidia-smi' # 右下角监控GPU
   ```

2. **同步输入到所有窗格**
   - 先按 `Ctrl+b :` 进入命令模式
   - 输入 `setw synchronize-panes`

3. **会话恢复工具**
   ```bash
   # 安装 tmux-resurrect
   git clone https://github.com/tmux-plugins/tmux-resurrect ~/.tmux/plugins/tmux-resurrect
   # 在 .tmux.conf 添加：
   run-shell ~/.tmux/plugins/tmux-resurrect/resurrect.tmux
   ```
   - 保存会话: `Ctrl+b Ctrl+s`
   - 恢复会话: `Ctrl+b Ctrl+r`

> 提示：熟练使用 Tmux 可大幅提升终端工作效率，建议从基础会话管理开始逐步掌握窗格操作。



如果您的 Ubuntu 系统没有安装任何显示管理器（如 GDM、LightDM 等），那么您可能使用的是纯命令行环境（无图形界面）或最小化安装。这种情况下，我们可以直接配置控制台自动登录：

## 纯命令行环境自动登录解决方案

#### 方法 1：使用 systemd 配置 TTY 自动登录

1. **创建 systemd 覆盖配置文件**：
   ```bash
   sudo mkdir -p /etc/systemd/system/getty@tty1.service.d
   sudo nano /etc/systemd/system/getty@tty1.service.d/override.conf
   ```

2. **添加以下内容**（替换 `yourusername` 为您的实际用户名）：

   [Service]
   ExecStart=
   ExecStart=-/sbin/agetty --autologin yourusername --noclear %I $TERM
   ```

3. **保存并应用配置**：
   ```bash
   sudo systemctl daemon-reload
   ```

4. **重启系统测试**：
   ```bash
   sudo reboot
   ```

#### 方法 2：修改 inittab（旧版系统）

如果您的系统使用 sysvinit 而不是 systemd：

1. **编辑 inittab 文件**：
   ```bash
   sudo nano /etc/inittab
   ```

2. **找到类似以下的行**：
   ```
   1:2345:respawn:/sbin/getty 38400 tty1
   ```

3. **修改为**（替换 `yourusername`）：
   ```
   1:2345:respawn:/sbin/getty --autologin yourusername 38400 tty1
   ```

4. **保存并重启**：
   ```bash
   sudo reboot
   ```

### 可选：启动后自动运行命令或进入图形界面

如果您希望登录后自动执行某些操作（如启动图形界面）：

1. **编辑用户配置文件**：
   ```bash
   nano ~/.bash_profile
   ```

2. **添加以下内容**（根据需求选择）：

   **启动图形界面**（如果安装了桌面环境）：
   ```bash
   if [[ -z $DISPLAY ]] && [[ $(tty) = /dev/tty1 ]]; then
     exec startx
   fi
   ```

   **自动运行特定命令**：
   ```bash
   if [[ $(tty) = /dev/tty1 ]]; then
     echo "自动登录成功！"
     # 在此添加您想自动运行的命令
   fi
   ```

### 验证配置是否生效

```bash
# 检查 systemd 配置
systemctl cat getty@tty1.service

# 检查 agetty 进程参数
ps aux | grep agetty | grep tty1
```

### 恢复手动登录

```bash
# 删除覆盖配置文件
sudo rm /etc/systemd/system/getty@tty1.service.d/override.conf
sudo systemctl daemon-reload

# 对于 inittab 方法，恢复原始配置
sudo nano /etc/inittab
```

### 注意事项

1. **安全警告**：控制台自动登录意味着任何人都能物理访问您的机器时获得完整访问权限
2. **用户名替换**：所有命令中的 `yourusername` 必须替换为您的实际用户名
3. **多个 TTY**：此配置仅影响 tty 1（第一个虚拟终端），其他 TTY 仍需要登录
4. **登录后操作**：`.bash_profile` 中的命令会在每次登录时执行

如果您能提供更多系统信息，我可以给出更精确的建议：
```bash
# 请分享以下命令输出：
lsb_release -a
echo $DESKTOP_SESSION
systemctl --version | head -1
```