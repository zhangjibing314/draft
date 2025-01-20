[toc]

# 在 qemu 中跑 linux kernel
在 qemu 中跑 linux kernel 可以选择通过 boot 加载的方式启动，也可以选择直接加载内核的方式启动，后者需要做的几件事：  
1. 编译 linux kernel  
2. 制作根文件系统
## 编译 linux kernel
交叉编译 arm64 架构：   
```shell
# 配置编译选项
make ARM=aarch64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig
# 编译
make ARM=aarch64 CROSS_COMPILE=aarch64-linux-gnu-
# 编译生成的镜像位置: ./arch/arm64/boot/
```
## 制作 根文件系统
### 通过 busybox 构建根文件系统
* 编译 busybox  
```shell
# 1. 下载 busybox 源码
# 2. 配置编译选项
make ARCH=aarch64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig
# 3. 编译
make ARCH=aarch64 CROSS_COMPILE=aarch64-linux-gnu-
# 4. 安装(默认在 ./_install/ 下)
make ARCH=aarch64 CROSS_COMPILE=aarch64-linux-gnu- install
```
* 制作 根文件系统

```shell
# 1. 创建目录 tempdir
mkdir tempdir

# 2. 将 _install/ 下的内容拷贝到 tempdir/
cp _install/* tempdir/

# 3. 在 tempdir 创建必要的目录和文件
mkdir dev 
sudo mknod dev/console c 5 1
sudo mknod dev/ram b 1 0 
touch init

# 4. 在 init 文件中写入内容:
#!/bin/sh
echo "INIT SCRIPT"
mkdir /proc
mkdir /sys
mount -t proc none /proc
mount -t sysfs none /sys
mkdir /tmp
mount -t tmpfs none /tmp
echo -e "\nThis boot took $(cut -d' ' -f1 /proc/uptime) seconds\n"
exec /bin/sh

# 5. 赋予 init 文件可执行权限
chmod +x init

# 6. 打包成内核可以使用的格式：
find ./tempdir | cpio -o --format=newc > rootfs.cpio
```

## 启动
```shell
qemu-system-aarch64 \
	-nographic \
	-machine virt \
	-cpu cortex-a57 \
	-smp 1 \
	-m 1G \
	-kernel ./linux/arch/arm64/boot/Image \
	-initrd ./busybox/_install/rootfs.cpio \
	--append "nokaslr root=/dev/ram init=/init"
```
