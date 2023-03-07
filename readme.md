

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