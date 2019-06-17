# MAC 重新注册用户
/sbin/mount -uaw 回车
rm /var/db/.applesetupdone 回车（注意“.”前没有空格）
reboot 回车

# OSX 安装 
安装 home brew 
`ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"`
`brew search [XXX]` 	# 搜索
`brew install [XXX]`	# 安装


# 软连接

## create sublime text3 
```
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl
$vim ~/.bashrΩ
alias sublime='subl'
$sublme ~/xxx. # use sublime open a file

```


## apache 

$ sudo httpd -k start
$ sudo httpd -k restart

```conf
<VirtualHost *:80>
    #ServerAdmin webmaster@xiaohua.com
    DocumentRoot "/Users/qlover/sites"
    ServerName localhost
    <Directory "/Users/qlover/sites>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride None
        Require all granted
    </Directory>
</VirtualHost>
```

// downlonad php7
$ curl -s http://php-osx.liip.ch/install.sh | bash -s 7.0
// downlonad mysql5
http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.10-osx10.10-x86_64.dmg
http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.10-osx10.10-x86_64.dmg


