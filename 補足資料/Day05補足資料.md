Day5 のハンズオンで利用するコマンドを記載します。

# Day5-1: dnf を用いたパッケージ管理
```bash
dnf update
dnf search tree
dnf install tree # エラー
sudo dnf install tree # 途中で y を入力し、Enter

dnf list installed
dnf list installed | grep tree
tree

sudo dnf remove tree
dnf list installed | grep tree
tree # エラー

sudo dnf install -y tree
```

# Day5-2: git をインストールする
```bash
dnf search git
sudo dnf install -y git
git --help
git clone https://github.com/ketancho/01linux7days.git

ls -l 01linux7days
tree 01linux7days
```

# Day5-3: httpd をインストールし、起動する
```bash
sudo dnf install -y httpd

sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl status httpd
```

# Day5-4: httpd の停止と再起動
```bash
# 停止
sudo systemctl status httpd
sudo systemctl stop httpd
sudo systemctl status httpd

# 起動
sudo systemctl status httpd
sudo systemctl start httpd
sudo systemctl status httpd

# 再起動
sudo systemctl restart httpd

# OS が再起動するとどうなるかのテスト
sudo systemctl status httpd

# OS が再起動したときに、自動で起動する設定を行う
sudo systemctl enable httpd
sudo systemctl status httpd
## ここで OS 再起動
sudo systemctl status httpd
```

# Day5-5: httpd の設定ファイルの確認とポート番号
```bash
less /etc/httpd/conf/httpd.conf

# ポートの確認
sudo ss -tlnp

# 表示されている index.html はどこにあるのかを確認
ls -l /var/www/html/
tail /etc/httpd/conf/httpd.conf
cat /etc/httpd/conf.d/welcome.conf
cat /usr/share/httpd/noindex/index.html
```

# Day5-6: index.html ファイルを作成する
```bash
sudo vim /root/.vimrc
====
set tabstop=4
====

sudo vim /var/www/html/index.html

# 参考：Git から clone したファイルの利用方法
cat 01linux7days/Day05/index.html.Day05-06-end
sudo cp 01linux7days/Day05/index.html.Day05-06-end /var/www/html/index.html
```

# Day5-7: index.html ファイルを修正する
```bash
sudo vim /var/www/html/index.html

sudo mkdir /var/www/html/img
sudo cp 01linux7days/Day05/01linux7days.png /var/www/html/img/
```

# Day5-8: tail -f を使って httpd のログファイルを確認する
```bash
sudo tail -f /var/log/httpd/access_log

sudo tail -f /var/log/httpd/error_log
```

# Day5-9: curl / wget / ping - 外部との通信で頻繁に使うコマンド
```bash
# curl
curl http://x.x.x.x/ # x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください
curl -i http://x.x.x.x/
curl -v http://x.x.x.x/

# wget
wget http://x.x.x.x/ 
ls -l
more index.html

wget -O day6-index.html http://x.x.x.x/
ls -l
more day6-index.html

# ping （デモ）
ping x.x.x.x # エラー
## セキュリティグループの変更後
ping x.x.x.x
ping -c 3 x.x.x.x
```