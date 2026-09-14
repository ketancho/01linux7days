exDay2 の実践演習の手順の例を記載します。

# free コマンド
free
free -h

# wc コマンド
cd ~/wc-hands-on

ls -l
cat app.log

wc app.log
wc -l app.log

grep ERROR app.log
grep ERROR app.log | wc -l
grep WARN app.log | wc -l


# sed コマンド
cd ~/sed-diff-hands-on
cat app.conf

cp app.conf app.conf.backup

sed 's/dev/prod/' app.conf
cat app.conf

sed 's/dev/prod/g' app.conf
cat app.conf

sed -i 's/dev/prod/g' app.conf
cat app.conf


# [演習4] diff コマンド
ls -l

diff app.conf.backup app.conf
diff -u app.conf.backup app.conf


# [演習5] journalctl コマンド
sudo systemctl status httpd

sudo journalctl -u httpd

sudo systemctl restart httpd
sudo systemctl status httpd

sudo journalctl -u httpd

sudo journalctl -u httpd -f
sudo systemctl restart httpd
# 終了
# Ctrl + C