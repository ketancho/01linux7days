Day4 のハンズオンで利用するコマンドを記載します。

# Day4-1: Linux におけるルートユーザーと管理者権限
```bash
# sudo <コマンド> のパターン
date
timedatectl set-timezone Asia/Tokyo # エラー
sudo timedatectl set-timezone Asia/Tokyo
date

# root ユーザーへのスイッチパターン
sudo su -
timedatectl set-timezone Asia/Tokyo
exit
```

# Day4-2: ユーザーを作成する
```bash
useradd test-user # エラー
sudo useradd test-user

sudo su - test-user
exit

cat /etc/passwd

id
```

# Day4-3: グループを作成する
```bash
sudo groupadd test-users
sudo usermod -aG test-users test-user
cat /etc/group

groups

sudo su - test-user
groups
sudo su - # エラー
exit

# おさらい演習
sudo useradd test-user2
sudo usermod -aG test-users test-user2
```

# Day4-4: Linux におけるパーミッションを理解する① - Owner x ディレクトリ操作
```bash
# 準備：owner である test-user のみ入れるディレクトリ作成
sudo su - test-user
ls -l /
ls -l /home

cd /tmp
mkdir permission-test
ls -l
chmod 700 permission-test/
chgrp test-users permission-test/
ls -l

exit

# Onwer = rwx の実験
cd permission-test/
touch permission-test-file.txt
mkdir permission-test-directory
rm -r permission-test-directory/
cd ..
ls -al permission-test/

# Onwer = rx の実験
chmod 500 permission-test/
cd permission-test/
touch permission-test-file2.txt # エラー
mkdir permission-test-directory # エラー
rm permission-test-file.txt # エラー
cd ..
ls -al permission-test/

# Onwer = r の実験
chmod 400 permission-test/
cd permission-test/ # エラー
ls -al permission-test/ # 一部表示されない

# Onwer = - の実験
chmod 000 permission-test/
ls -al permission-test/ # エラー
```

# Day4-5: Linux におけるパーミッションを理解する② - (Group, Other) x ディレクトリ操作
```bash
# 準備：Owner, Group, Other それぞれが異なる権限になるように
chmod 750 permission-test/

# Owner : test-user / rwx の実験
cd permission-test/
ls -al
touch permission-test-file2.txt

# Group: test-user2 / rx (w なし）の実験
## test-user2 に su
exit
sudo su - test-user2
cd /tmp

cd permission-test
ls -al
touch permission-test-file3.txt # エラー

# Other: ec2-user / - （rwx なし）の実験
## ec2-user に su
exit

cd /tmp
cd permission-test # エラー
```

# Day4-6: Linux におけるパーミッションを理解する③ - (Owner, Group, Other) x ファイル操作
```bash
# 準備：test-user でファイル作成 & グループ変更 & パーミッション変更
sudo su - test-user
touch permission-test-file.txt
chgrp test-users permission-test-file.txt
chmod 640 permission-test-file.txt

# test-user の場合
echo "test-user" >> permission-test-file.txt
cat permission-test-file.txt

# test-user2 の場合
exit
sudo su - test-user2
cd /tmp
echo "test-user2" >> permission-test-file.txt # エラー
cat permission-test-file.txt

# ec2-user の場合
exit
cd /tmp
echo "ec2-user" >> permission-test-file.txt # エラー
cat permission-test-file.txt # エラー
```

# Day4-7: パスワード認証の設定を行い、パスワードで SSH できるようにする
## 事前の設定
```bash
# パスワードを変更（作成）する
sudo passwd test-user

sudo su - test-user
passwd # 現在のパスワードの入力が求められるので入力
exit

# パスワードでの SSH はできないことを確認する
exit
ssh test-user@x.x.x.x # エラー
ssh -i 01linux7days.pem ec2-user@x.x.x.x

# SSH設定ファイルのバックアップ
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# SSH設定ファイルを編集
sudo vim /etc/ssh/sshd_config

============
# test-userのみパスワード認証を許可
Match User test-user
    PasswordAuthentication yes
    AuthenticationMethods password
============

# 設定ファイルの構文チェック
sudo sshd -t

# SSH サービス再起動
sudo systemctl restart sshd
```

## 疎通テスト (x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください)
```bash
ssh ex2-user@x.x.x.x # -> 接続できない
ssh test-user@x.x.x.x # -> パスワードが聞かれる
```

## 事後作業
```bash
ssh -i 01linux7days.pem ec2-user@x.x.x.x # x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください
sudo mv /etc/ssh/sshd_config.backup /etc/ssh/sshd_config

# 設定ファイルの構文チェック
sudo sshd -t

# SSH サービス再起動
sudo systemctl restart sshd

# test-user で SSH できないことを確認する
ssh test-user@x.x.x.x # エラー
```

# Day4-8: 公開鍵と秘密鍵を設定して SSH 接続する
```bash
#@Cloud Shell
ssh-keygen -t rsa -b 4096 -f ~/.ssh/test-user-key # パスワードは入れなくて OK

ls -al .ssh/
# → pub なしが秘密鍵、ありが公開鍵。公開鍵を Linux サーバーに送る
scp -i 01linux7days.pem ~/.ssh/test-user-key.pub ec2-user@54.178.106.70:/tmp/
ssh -i 01linux7days.pem ec2-user@54.178.106.70

#@EC2 Instance
# ec2-user 
sudo su - test-user

# test-user
mkdir .ssh
cp /tmp/test-user-key.pub .ssh/authorized_keys
chmod 700 .ssh/
chmod 600 .ssh/authorized_keys
## → 公開鍵が他のユーザーからは見えない設定にする必要がある

# ec2-user
exit
rm /tmp/test-user-key.pub
exit

#@CloudShell
ssh -i .ssh/test-user-key test-user@x.x.x.x
```

# Day4-9: 作成したユーザー・グループを削除する
```bash
## test-user の削除
# ユーザー test-user がいることを確認
sudo su - test-user

sudo userdel -r test-user
# → -r をつけないとホームディレクトリが残ってしまう

# su できないことを確認
sudo su - test-user

# 続いて、test-user2 も削除
sudo userdel -r test-user2

# グループも削除する
sudo groupdel test-users
```