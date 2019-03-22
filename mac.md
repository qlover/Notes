# MAC 重新注册用户
/sbin/mount -uaw 回车
rm /var/db/.applesetupdone 回车（注意“.”前没有空格）
reboot 回车

# OSX 安装 
安装 home brew 
`ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"`
`brew search [XXX]` 	# 搜索
`brew install [XXX]`	# 安装