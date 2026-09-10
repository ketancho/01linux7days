exDay1 の実践演習の手順の例を記載します。

# [演習1] Vim 操作
sudo vim /var/www/html/index.html

# Vim 内で操作
# i
# Linuz → Linux に修正
# :wq


# [演習2] ファイル検索
sudo find / -name "access_log"
sudo cat /var/log/httpd/access_log


# [演習3] 文字列検索
sudo grep 404 /var/log/httpd/access_log
sudo cat /var/log/httpd/access_log | grep 404


# [演習4] ログのリアルタイム確認
sudo tail -f /var/log/httpd/access_log

# 終了
# Ctrl + C


# [演習5] ファイル・ディレクトリ操作
sudo cat /var/www/html/index.html
sudo ls -l /var/www/html/

sudo mkdir /var/www/html/img
sudo ls -l /var/www/html/

sudo mv /var/www/html/01linux7days.png /var/www/html/img/
sudo ls -l /var/www/html/img
sudo ls -l /var/www/html/


# [演習6] パーミッション設定
cd ~

touch exDay01.sh
ls -l

chmod +x exDay01.sh
ls -l


# [演習7] シェルスクリプト作成
vim ~/exDay01.sh

# exDay01.sh の内容
# ------------------------------
#!/bin/bash

date > ~/exDay01.log
sudo systemctl status httpd >> ~/exDay01.log
sudo tail -5 /var/log/httpd/access_log >> ~/exDay01.log
# ------------------------------

# 実行・確認
./exDay01.sh
ls -l
cat exDay01.log