## Demo Server

初始化一个实例后

### ping

```bash
ansible -i hosts production ping -u root --ask-pass
```

### Server Setup 

```bash
ansible-playbook server_setup/production.yml -i hosts --ask-pass
```


### Server Config

```bash
ansible-playbook server_config/production.yml -i hosts
```


### Rails Config

```bash
ansible-playbook server_rails/production.yml -i hosts
```

### Common Config

修改 public.yml

```bash
ansible-playbook public.yml -i hosts
```

### app monit config

```bash
bundle exec cap production puma:monit:config
bundle exec cap production sidekiq:monit:config
```



### If Use Local Mysql
```
$ sudo mysql -u root
mysql> USE mysql;
mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
mysql> FLUSH PRIVILEGES;
mysql> exit;

$ sudo service mysql restart

$ sudo mysql -u root

mysql> USE mysql;
mysql> CREATE USER 'deploy'@'localhost' IDENTIFIED BY '';
mysql> GRANT ALL PRIVILEGES ON * . * TO 'deploy'@'localhost';
mysql> UPDATE user SET plugin='auth_socket' WHERE User='deploy';
mysql> FLUSH PRIVILEGES;
mysql> exit;

$ sudo service mysql restart

$ mysqladmin variables | grep socket
```

### SSH Config
```
vim ~/.ssh/config

# carrera server
 Host app_prod
 user deploy
 hostname ip
 
 Host app_prod_02
 hostname ip
 user deploy
 ProxyCommand ssh app_prod nc %h %p
```

### Rails Console

```bash
vim ~/.zshrc

alias rc_app_prod="ssh app_prod -t 'sudo su - deploy -c \"cd ~/apps/app/current && RAILS_ENV=production bundle exec rails c\"'"
alias rc_app_prod_o2="ssh app_prod_02 -t 'sudo su - deploy -c \"cd ~/apps/carrera/current && RAILS_ENV=production bundle exec rails c\"'"

source ~/.zshrc
```


```
TODO: 
1. 脚本执行 install ruby 会有问题 可能还是需要到机器上执行 rbenv install 2.3.1
2. 需要先到机器上执行一次git clone
3. 有可能需要先跑一次bundle
```

### TODO
nginx-master for production

### ssh连接远程主机执行脚本的环境变量问题
http://feihu.me/blog/2014/env-problem-when-ssh-executing-command-on-remote/

```
$sudo vim /etc/ssh/sshd_config;
add:
PermitUserEnvironment yes;
$sudo service ssh restart;

vim ~/.ssh/environment
加入各种需要的环境变量等
```
