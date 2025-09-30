Day1 のハンズオンで利用するコマンドを記載します。

# Day1-4: AWS CloudShell を使って Amazon Linux に SSH 接続する
```bash
# .pem ファイルのアップロードが成功していることを確認する（ls コマンドは Day1 で後ほど紹介します）
ls

# .pem ファイルのパーミッションを変更します（chmod コマンドは Day4 で紹介します）
chmod 400 01linux7days.pem

# Linux サーバーに ssh 接続する
ssh -i 01linux7days.pem ec2-user@x.x.x.x # x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください
exit
```

# Day1-5: プロトコルの基本と AWS のセキュリティーグループ
```bash
# CloudShell の IP アドレスを調べる
curl http://checkip.amazonaws.com/
```

# Day1-6: man コマンド、—help オプションを使って各コマンドを理解する
```bash
man ls
ls --help
ls -a
```

# Day1-7: コマンド実行を助けるコマンドと機能
```bash
clear
history
# "!数字" と入力すると、history の該当行を再実行できます
```

# Day1-8: Linux の停止と開始（再開）
```bash
shutdown
sudo shutdown
sudo shutdown -c
sudo shutdown -h now
sudo shutdown -r now
```
