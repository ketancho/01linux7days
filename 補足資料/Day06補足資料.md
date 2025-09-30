Day6 のハンズオンで利用するコマンドを記載します。

# Day6-1: ps / kill / top - プロセス関連のコマンド
```bash
# ps コマンドの実行
ps
ps u
ps au
ps aux
ps aux | grep httpd

# バックグラウンド実行
sleep 3
sleep 30 # Ctrl + C でキャンセル
sleep 30 &
ps ux
ps ux

# kill コマンドの実行
sleep 30 &
ps ux
kill <プロセス番号>

# top コマンドの実行
top # q で停止
```

# Day6-2: df / du - ディスク関連のコマンド
```bash
df
df -h

du -h
du -h | sort
du -h | sort -hr
```

# Day6-3: find コマンドでファイルやディレクトリを検索する
```bash
find ~ -name work-log-20250401.txt

# ワイルドカード（*）を利用
find ~ -name "*work*"

find / -name "*log*”
sudo find / -name "*log*”

# -size を指定
sudo find / -name "*.log" -size +100k

# -type を指定
find ~ -name "*log*"
find ~ -name "*log*" -type d

# find コマンド演習の事前準備
cp -r ~/01linux7days/Day06/find-practice ~/
tree find-practice
```

# Day6-4: find コマンド演習
```bash
# 想定回答（これ以外のコマンドでも、結果が同じであれば問題ありません。回答の一例となります。）
## ① ~/find-practice 以下 にある、.txt で終わる ディレクトリ/ファイルを全て表示してください。
find ./find-practice -name "*.txt"

##② ~/find-practice 以下にある、名前に “report” が含まれるディレクトリ/ファイルを全て表示してください。
find ./find-practice -name "*report*"

## ③ ~/find-practice/config 以下にある、名前が .json で終わるディレクトリ/ファイルを全て表示してください。
find ./find-practice/config -name "*.json"

## ④ ~/find-practice 以下にある、ディレクトリのみを全て表示してください。
find ./find-practice -type d

## ⑤ ~/find-practice 以下にある、ファイル名に “log” が含まれる “ファイルのみ” を全て表示してください。
find ./find-practice -name "*log*" -type f

## ⑥ **~/find-practice 以下にある、10KB 以上のディレクトリ/ファイルを全て表示してください。
find ./find-practice -size +10k
```

# Day6-5: grep コマンドでテキストから文字列を検索する
```bash
sudo cat /var/log/httpd/access_log
sudo grep 404 /var/log/httpd/access_log
sudo cat /var/log/httpd/access_log | grep 404

sudo tail -f /var/log/httpd/access_log
sudo tail -f /var/log/httpd/access_log | grep 404
```

# Day6-6: ヒアドキュメント << を使って、複数行をコマンドに入力する
```bash
cat > here-doc.txt << 'EOF'
- find
- grep
EOF

cat here-doc.txt

date

# 変数を展開しない 'EOF' （シングルクォーテーションあり）
cat > here-doc.txt << 'EOF'
$(date)
- find
- grep
EOF

cat here-doc.txt

# 変数を展開する EOF（シングルクォーテーションなし）
cat > here-doc.txt << EOF
$(date)
- find
- grep
EOF

cat here-doc.txt
```
