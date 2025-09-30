Day3 のハンズオンで利用するコマンドを記載します。

# Day3-1: mkdir コマンドでディレクトリを作成する
```bash
man mkdir

ls -l

mkdir work-logs
ls -l

mkdir work-logs/2025
ls -l # 2025 ディレクトリは表示されない
ls -l work-logs/

mkdir work-logs/2026/04 # エラー
mkdir -p work-logs/2026/04
ls -l work-logs/
ls -l work-logs/2026/
```

# Day3-2: cd / pwd - カレントディレクトリの移動と確認
```bash
ls -l

cd work-logs/
ls -l

pwd

cd 2026
pwd

cd ..
pwd

cd ..
pwd

# cd - について
pwd
cd work-logs/2026/04/
cd -

# ホームディレクトリ ~ について
cd work-logs/2026/04/
cd ~

# カレントディレクトリ . について
cd ./work-logs/
cd -
cd work-logs/
```

# Day3-3: 相対パスと絶対パス
```bash
cd ~
cd work-logs
cd -
cd /home/ec2-user/work-logs/
cd -
```

# Day3-4: ディレクトリに対する mv / cp / rm 操作
```bash
cd work-logs/

# ディレクトリ間でファイルを mv
touch work-log-20250401.txt
ls -l
mv work-log-20250401.txt 2025/
ls -l 2025/

# ディレクトリ自体の mv
mkdir 01
ls -l
mv 01 2025/
ls -l 2025/

# ディレクトリのリネーム
ls -l
mv 2026 2027
ls -l

# ディレクトリのコピー
cp 2027 2026 # エラー
cp -r 2027 2026
ls -l
man cp

# ディレクトリの削除
rm -r 2027
```

# Day3-5: ln コマンドでリンクを作る

```bash
cd ~
vim work-logs/2025/work-log-20250401.txt

# ハードリンクの作成と確認
ln work-logs/2025/work-log-20250401.txt h-current-log
vim h-current-log
cat h-current-log
cat work-logs/2025/work-log-20250401.txt

# シンボリックリンクの作成と確認
ln -s work-logs/2025/work-log-20250401.txt s-current-log
vim s-current-log
cat s-current-log
cat h-current-log
cat work-logs/2025/work-log-20250401.txt

# ハードリンク、シンボリックリンクの違いの確認
ls -li work-logs/2025/
ls -li

# リンク先のファイルを削除
rm work-logs/2025/work-log-20250401.txt
ls -li
cat s-current-log
cat h-current-log
```

# Day3-6: alias コマンドで、長いコマンドに短い名前をつける
```bash
ls
man ls
alias
ls --color=auto
\ls

alias la='ls -al'
la
alias

unalias la

# alias が永続化されないことの確認
alias la='ls -al'
exit
ssh -i 01linux7days.pem ec2-user@x.x.x.x # x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください
la # エラー
```

# Day3-7: .bashrc を修正して、セッション開始時に設定される項目を定義する
```bash
vim ~/.bashrc
# alias la='ls -al' を追加

source ~/.bashrc

exit
ssh -i 01linux7days.pem ec2-user@x.x.x.x # x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください
la # 成功

vim ~/.bashrc
# alias la='ls -al' を削除
la # この時点では成功

exit
ssh -i 01linux7days.pem ec2-user@x.x.x.x # x.x.x.x は皆さまの EC2 インスタンスの「パブリック IPv4 アドレス」に置き換えてください
la # 失敗
```
