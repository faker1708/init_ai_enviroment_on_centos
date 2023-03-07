

这是在centos上部署深度学习开发环境的指南

尝试一个小时吧,如果没思路,就放弃去装ubuntu

我现在有了 .run文件 是n卡驱动


流程{
    1   卸载自带的开源驱动 nouveau
    2   准备好驱动与cuda
    3   安装驱动  cuda
    4   安装 py3
    5   安装pytorch

}



#新建一个配置文件
sudo vim /etc/modprobe.d/blacklist-nouveau.conf
#写入以下内容
blacklist nouveau
options nouveau modeset=0
#保存并退出
:wq
#备份当前的镜像
sudo mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak
#建立新的镜像
sudo dracut /boot/initramfs-$(uname -r).img $(uname -r)
#重启
sudo reboot
#最后输入上面的命令验证
lsmod | grep nouveau
————————————————
版权声明：本文为CSDN博主「桐原因」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_36287702/article/details/122435148



################

sudo vi /etc/modprobe.d/blacklist-nouveau.conf


sudo mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak


############


cd /etc/modprobe.d
sudo vi blacklist-nouveau.conf

blacklist nouveau
options nouveau modeset=0



驱动 是 .run 400 MB
cuda 是 .rpm 4 GB
pytorch 用pip 安装





驱动与cuda及tensorflow对应关系



驱动 

https://www.nvidia.cn/geforce/drivers/



cuda 11.7

wget https://developer.download.nvidia.com/compute/cuda/11.7.0/local_installers/cuda-repo-rhel7-11-7-local-11.7.0_515.43.04-1.x86_64.rpm
sudo rpm -i cuda-repo-rhel7-11-7-local-11.7.0_515.43.04-1.x86_64.rpm
sudo yum clean all
sudo yum -y install nvidia-driver-latest-dkms cuda
sudo yum -y install cuda-drivers





https://pytorch.org/


基于cuda11.7

pip3 install torch torchvision torchaudio


在这里下载pytorch 安装包

参考

https://blog.csdn.net/qq_36287702/article/details/122435148




卸载掉
那个驱动 


然后安装 n官方驱动 与 cuda

sudo rpm cuda-repo-rhel7-11-7-local-11.7.0_515.43.04-1.x86_64.rpm


要先装gcc tmd




报错了


Unable to find the kernel source tree for the currently running kernel.  Please make sure you have installed
         the kernel source files for your kernel and that they are properly configured; on Red Hat Linux systems, for
         example, be sure you have the 'kernel-source' or 'kernel-devel' RPM installed.  If you know the correct
         kernel source files are installed, you may specify the kernel source path with the '--kernel-source-path'
         command line option.



无法找到当前正在运行的内核的内核源树。 请确保您已安装
内核的内核源文件，并正确配置它们； 在红帽Linux系统上，用于
例如，请确保已安装了“内核源”或“内核直角” rpm。 如果您知道正确的
内核源文件已安装，您可以使用“ -Kernel-source-path”指定内核源路径'
命令行选项。


是不是
sudo reboot 这个命令有问题啊.
下次不用了.

不是,好像是显卡把屏幕给熄 了...淦.

不对,屏幕熄 了凭什么ssh连不上.


sudo sh NVIDIA-Linux-x86_64-525.89.02.run --kernel-source-path



sudo sh NVIDIA-Linux-x86_64-525.89.02.run --kernel-source-path 3.10.0-1160.83.1.el7.x86_64.debug



http://mirror.centos.org/centos/7/os/x86_64/Packages/kernel-devel-3.10.0-1160.83.1.el7.x86_64.rpm

http://mirror.centos.org/centos/7/os/x86_64/Packages/kernel-devel-3.10.0-1160.el7.x86_64.rpm


http://vault.centos.org/7.9.2009/os/Source/SPackages/kernel-3.10.0-1160.el7.src.rpm




https://blog.csdn.net/A15216110998/article/details/113402172

要命了.装个cuda要命了


优先安装cuda

卡在144/179



cuda、cuda-11.7

export PATH=/usr/local/cuda-11.7/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.7/lib64:$LD_LIBRARY_PATH


sudo vi /etc/profile




终于安装好cuda了.真的是要命了.


