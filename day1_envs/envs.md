## ubuntu换源

ubuntu16:

```bash
cp /etc/apt/sources.list /etc/apt/sourcesbak.list
echo "deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
deb http://archive.canonical.com/ubuntu xenial partner
deb-src http://archive.canonical.com/ubuntu xenial partner
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse">/etc/apt/sources.list
sudo apt-get update
sudo apt-get upgrade
```


ubuntu18:

```bash
cp /etc/apt/sources.list /etc/apt/sourcesbak.list
echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse">/etc/apt/sources.list
sudo apt-get update
sudo apt-get upgrade
```



## 基础环境

- pip换源
- 32位libc环境
- gdb插件(pwndbg,peda)
- pwntools
- one_gadget
- libc-database

pwn_init.sh

```bash
#!/bin/bash
cd ~/
# change sourse to ustc
echo "I suggest you modify the /etc/apt/sources.list file to speed up the download."
echo "Press Enter to continue~"
read test
#sudo  sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
# change sourse —— deb-src 
sudo sed -i 's/# deb-src/deb-src/' "/etc/apt/sources.list"
# change pip source
mkdir ~/.pip
echo -e "[global]\nindex-url = https://pypi.tuna.tsinghua.edu.cn/simple\n[install]\ntrusted-host = pypi.tuna.tsinghua.edu.cn" >  ~/.pip/pip.conf
# support 32 bit
dpkg --add-architecture i386
sudo apt-get update
# sudo apt-get -y install lib32z1
sudo apt-get -y install libc6-i386
# maybe git？
sudo apt-get -y install git gdb
# install pwndbg
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
# install peda
git clone https://github.com/longld/peda.git ~/peda
# echo "source ~/peda/peda.py" >> ~/.gdbinit
echo "source ~/pwndbg/gdbinit.py" > ~/.gdbinit
# download the libc source to current directory(you can use gdb with this example command: directory ~/glibc-2.24/malloc/)
sudo apt-get source libc6-dev
# install pwntools
sudo apt-get -y install python python-pip
pip install pwntools
# install one_gadget
sudo apt-get -y install ruby
sudo gem install one_gadget
# download 
git clone https://github.com/niklasb/libc-database.git ~/libc-database
echo "Do you want to download libc-database now(Y/n)?"
read input
if [[ $input = "n" ]] || [[ $input = "N" ]]; then
	echo "you can cd ~/libc-database and run ./get to download the libc at anytime you want"
else
	cd ~/libc-database && ./get
fi
echo "========================================="
echo "=============Good, Enjoy it.============="
echo "========================================="

```


