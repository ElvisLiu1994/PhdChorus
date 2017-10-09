# 升级OpenSSH

1. 升级zlib  
```
tar zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make && make install
```

2. 升级pam  
```
tar zxvf  Linux-PAM-1.3.0.tar.gz
cd Linux-PAM-1.3.0
./configure
make && make install
```

3. 编译安装openssl-1.0.2  
```
已经是最新版本，但是为了编译源码，还需要安装libssl-dev;  
sudo apt-get install libssl-dev
```

4. 编译安装openssh  
```
tar zxvf openssh-7.5p1.tar.gz
cd openssh-7.5p1
./configure --with-zlib --with-ssl-dir --with-pam --bindir=/usr/bin --sbindir=/usr/sbin --sysconfdir=/etc/ssh

  make clean

  make && make install
```

5. 复制脚本及配置文件  
```
cp ./contrib/redhat/sshd.init /etc/init.d/sshd   
sudo chmod u+x /etc/init.d/sshd  
sudo cp ssh_config /etc/ssh/ssh_config  
sudo cp -p sshd_config /etc/ssh/sshd_config  
ssh -V  
sudo /etc/init.d/sshd restart  
```

6. 错误  
执行`sudo /etc/init.d/sshd restart`时报错：  
```
/etc/init.d/sshd: line 16: /etc/rc.d/init.d/functions: No such file or directory
Stopping sshd:/etc/init.d/sshd: line 59: killproc: command not found  

  Starting sshd:/etc/init.d/sshd: line 50: success: command not found
  /etc/init.d/sshd: line 50: failure: command not found
```
将/etc/init.d/sshd文件中的`. /etc/rc.d/init.d/functions`修改为`. /lib/lsb/init-functions`后不再报错。  
<br/>
但是使用`service ssh restart`还是报超时错误。
使用`systemctl status sshd`查看服务进程状态没有错误信息
