## 这项目没啥用,终止了

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
https://download.pytorch.org/whl/torch/


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

3.10.0-1160.83.1.el7.x86_64



./ --kernel-source-path=/usr/src/kernels/3.10.0-1160.83.1.el7.x86_64


sudo ./NVIDIA-Linux-x86_64-525.89.02.run --kernel-source-path=/usr/src/kernels/3.10.0-1160.83.1.el7.x86_64




 The NVIDIA proprietary driver is already installed in this system. It was
 installed through a 3d party repository.

 Please refer to the following page for additional information and to install
 optional driver components:

 http://negativo17.org/nvidia-driver/



nvcc 装好了.驱动还没装好.

最关键的,还是一个靠谱的教程...

而教程难找的原因,还是因为同样的需求的人少,或者说我确实不应该使用centos....而应该用ubuntu....

算了.我偏要用centos



安装驱动时

一直选择yes直到安装完成就行



 ERROR: Failed to run `/sbin/dkms install --no-depmod -m nvidia -v 525.89.02 -k 3.10.0-1160.83.1.el7.x86_64`:
         nvidia.ko.xz:
         Running module version sanity check.
         Module version 525.89.02 for nvidia.ko.xz
         exactly matches what is already found in kernel 3.10.0-1160.83.1.el7.x86_64.
         DKMS will not replace this module.
         You may override by specifying --force.

         nvidia-uvm.ko.xz:
         Running module version sanity check.
         Module version 525.89.02 for nvidia-uvm.ko.xz
         exactly matches what is already found in kernel 3.10.0-1160.83.1.el7.x86_64.
         DKMS will not replace this module.
         You may override by specifying --force.

         nvidia-modeset.ko.xz:
         Running module version sanity check.
         Module version 525.89.02 for nvidia-modeset.ko.xz
         exactly matches what is already found in kernel 3.10.0-1160.83.1.el7.x86_64.
         DKMS will not replace this module.
         You may override by specifying --force.

         nvidia-drm.ko.xz:
         Running module version sanity check.
         Module version 525.89.02 for nvidia-drm.ko.xz
         exactly matches what is already found in kernel 3.10.0-1160.83.1.el7.x86_64.
         DKMS will not replace this module.
         You may override by specifying --force.

         nvidia-peermem.ko.xz:
         Running module version sanity check.
         Module version 525.89.02 for nvidia-peermem.ko.xz
         exactly matches what is already found in kernel 3.10.0-1160.83.1.el7.x86_64.
         DKMS will not replace this module.
         You may override by specifying --force.
         Error! Installation aborted.



我c,装好了.装了tmd 三个小时.

真的值得吗???

https://blog.csdn.net/A15216110998/article/details/113402172


安装 torch

pip install 


cp33 means CPython 3.3




我们选centos 真的是个错误,就应该用 ubuntu 的.

妈的,已经装一个晚上的.全是些无聊至极的弱智问题.烦死了.


我是真的蠢啊.人家用unbuntu 只要装好系统 一切都好了.我非要用cetnos .搞了一个晚上四个小时,还是没搞好.


现在的问题是,如果安装一个能用pip的python 311??


我好像是已经安装好了openssl



先让一步.我的3.6是能用ssl的.




真的要气死了.smb又不能用了.


wget https://download.pytorch.org/whl/cu113/torch-1.10.2%2Bcu113-cp36-cp36m-linux_x86_64.whl


https://blog.csdn.net/qq_39715000/article/details/125009276



修复pip3 到 python3.6

cd /us


算了这样

python3.6 -m pip


sudo python3.6 -m pip install torch-1.10.2+cu113-cp36-cp36m-linux_x86_64.whl

python3.6 -m pip install torch-1.10.2+cu113-cp36-cp36m-linux_x86_64.whl


我现在是




现在基本的torch 已经安装 好了


现在的问题是
jupyter怎么开启到局域网??

    https://blog.csdn.net/qcyfred/article/details/82767965
smb 又挂了

不必要:
    vscode怎么调用远程内核 ?



没必要装connda 但真的太费劲 了 centos
早知道直接装unbuntu了


哦 torch vision 还没装..

我安装的torch版本
torch-1.10.2+cu113-cp36-cp36m-linux_x86_64.whl

 1553 MiB / 
11264 MiB |




1553
1927

1
10

374




15:49 2023/3/8

浪费一天时间研究这显卡到底行不行。

研究一天，也没啥成果。算了。以后不做这种事了。

gpuz说什么就是什么吧。不管了。

