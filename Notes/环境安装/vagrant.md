1. 下载&安装 VirtualBox https://www.virtualbox.org/，要开启 CPU 虚拟化

2.  https://www.vagrantup.com/downloads.html Vagrant 下载

3. 初始化镜像

   1. 初始化centos7

      1. ```bash
         Vagrant init centos/7
         ```

   2. 启动
   
      1. ```
         vagrant up
         ```
   
   3. 连接
   
      1. 默认vagrant/vagrant和root/vagrant
   
      2. ```
         vagrant ssh
         ```
   
   4. 设置可以密码登录
   
      1. ```
         vi /etc/ssh/sshd_config
         修改 PasswordAuthentication yes/no
         重启服务 service sshd restart
         ```
   
   5. 设置固定IP
   
      1. ```
         修改 Vagrantfile
         config.vm.network "private_network", ip: "192.168.56.10"
         ```
   
         
   
   6. 1





