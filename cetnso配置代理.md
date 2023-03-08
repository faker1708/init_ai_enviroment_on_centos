


git 的代理设置和取消 


设置

git config --global http.proxy http://192.168.1.4:49766
git config --global https.proxy https://192.168.1.4:49766


成功了

gti clone 正常工作了

取消
git config --global --unset http.proxy
git config --global --unset https.proxy


