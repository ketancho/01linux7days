Day7 のハンズオンで利用するコマンドや情報を記載します。

# Day7-1: シェルとシェルスクリプト
```bash
touch sample.sh
vim sample.sh
```

この時点での sample.sh
```bash
#!/bin/bash

ls -l /home/ec2-user/
```

実行
```bash
chmod +x sample.sh
./sample.sh
```

# Day7-2: シェルスクリプトの変数と引数を使ってみる
※ ここからの `vim sample.sh` は省略します。
## 変数の実験（失敗編）
この時点での sample.sh
```bash
#!/bin/bash

DIR="/home/ec2-user/"

ls -l DIR
```

実行
```bash
./sample.sh
```

## 変数の実験（成功編）
この時点での sample.sh
```bash
#!/bin/bash

DIR="/home/ec2-user/"

ls -l $DIR
```

実行
```bash
./sample.sh
```

## 引数の実験
この時点での sample.sh
```bash
#!/bin/bash

MONTH=9

echo "今日は $MONTH 月です！"
```

実行
```bash
./sample.sh
```

# 引数化する
この時点での sample.sh
```bash
#!/bin/bash

MONTH=$1

echo "今日は $MONTH 月です！"
```

実行
```bash
./sample.sh 9 
```

# Day7-3: シェルスクリプトの制御構造を使ってみる
```bash
./sample.sh # 引数なしで実行できてしまう
```
## if 文を使う
この時点での sample.sh
```bash
#!/bin/bash

if [ -n "$1" ]; then
    echo "今日は $1 月です！"
fi
```

実行
```bash
./sample.sh 9 # if 文を挟んでも動作することをまず確認
```

## if-else を使う
この時点での sample.sh
```bash
#!/bin/bash

if [ -n "$1" ]; then
    echo "今日は $1 月です！"
else
    echo "引数を指定してください。"
fi
```

実行
```bash
./sample.sh 9
./sample.sh
```

## for 文を使う
この時点での sample.sh
```bash
#!/bin/bash

for ((i=1;i<=$1;i++)) do
    echo "こんにちは"
done
```

実行
```bash
./sample.sh 5
```

# Day7-4: これまでのふりかえりを順番に表示するスクリプトを作成する①
## シェルスクリプト　create-journal-html.sh　の作成
```bash
touch create-journal-html.sh
vim create-journal-html.sh
```

この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"
echo $JOURNAL_DIR
```

参考：ディレクトリパスのコピペのために別タブで実施したコマンド
```bash
ssh
realpath 01linux7days-journals
```

実行
```bash
./create-journal-html.sh # エラー

chmod +x create-journal-html.sh
./create-journal-html.sh
```

## create-journal-html.sh をアップデートし、for 文でディレクトリ内の .txt ファイルを全て取得する
※ ここからの `vim create-journal-html.sh` は省略します。

この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"

for txt_file in $JOURNAL_DIR/*.txt; do
    echo $txt_file
done
```

※ 以降、create-journal-html.sh の実行は全て引数なしの `./create-journal-html.sh` となるので、コマンドを省略します。

# Day7-5: これまでのふりかえりを順番に表示するスクリプトを作成する②
## ファイルの名前だけを短く表示する形にする
```bash
basename /var/www/html/index.html
```

この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"

for txt_file in $JOURNAL_DIR/*.txt; do
    echo "$(basename $txt_file)"
done
```

## cat コマンドでファイルの中身を出力する
この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"

for txt_file in $JOURNAL_DIR/*.txt; do
    echo "$(basename $txt_file): $(cat $txt_file)"
done
```

# Day7-6: これまでのふりかえりを index.html に出力するようスクリプトを修正する①
## TEMP ファイルに出力
この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"
TEMP_FILE="/home/ec2-user/temp-index.html"

for txt_file in $JOURNAL_DIR/*.txt; do
    echo "$(basename $txt_file): $(cat $txt_file)" >> $TEMP_FILE
done
```

## body までの TEMP ファイル完成
この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"
TEMP_FILE="/home/ec2-user/temp-index.html"

cat > $TEMP_FILE << 'EOF'
<!DOCTYPE html>
<html>
<body>
    <h1>講座の学び</h1>
    <ul>
EOF

for txt_file in $JOURNAL_DIR/*.txt; do
    echo "      <li>$(basename $txt_file): $(cat $txt_file)</li>" >> $TEMP_FILE
done

cat >> $TEMP_FILE << 'EOF'
    </ul>
</body>
</html>
EOF
```

# Day7-7: これまでのふりかえりを index.html に出力するようスクリプトを修正する②
## head タグを追加
この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"
TEMP_FILE="/home/ec2-user/temp-index.html"

cat > $TEMP_FILE << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>01linux7days 講座</title>
</head>
<body>
    <h1>講座の学び</h1>
    <ul>
EOF

for txt_file in $JOURNAL_DIR/*.txt; do
    echo "        <li>$(basename $txt_file): $(cat $txt_file)</li>" >> $TEMP_FILE
done

cat >> $TEMP_FILE << 'EOF'
    </ul>
</body>
</html>
EOF
```

# 最終版の　create-journal-html.sh　を作成
この時点での create-journal-html.sh
```bash
#!/bin/bash

JOURNAL_DIR="/home/ec2-user/01linux7days-journals"
OUTPUT_FILE="/var/www/html/index.html"
TEMP_FILE="/home/ec2-user/temp-index.html"

cat > $TEMP_FILE << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>01linux7days 講座</title>
</head>
<body>
    <h1>講座の学び</h1>
    <ul>
EOF

for txt_file in $JOURNAL_DIR/*.txt; do
    echo "        <li>$(basename $txt_file): $(cat $txt_file)</li>" >> $TEMP_FILE
done

cat >> $TEMP_FILE << 'EOF'
    </ul>
</body>
</html>
EOF

sudo cp $TEMP_FILE $OUTPUT_FILE
rm $TEMP_FILE
```

参考：index.html ファイルのパスをコピペするために別タブで実施したコマンド
```bash
sudo find / -name "index.html”
```

# Day7-8: cron を使い、index.html 作成スクリプトが定期的に実行されるようにする
```bash
## インストールしないと使えない
crontab -e

## インストールと起動の流れ
sudo dnf install -y cronie
sudo systemctl start crond
sudo systemctl enable crond
sudo systemctl status crond

## 編集
crontab -e
====
*/10 * * * * /home/ec2-user/create-journal-html.sh
====

## 確認
crontab -l

# （Day6まではファイルがあり、かつ index.html に反映済みなので）Day7の学びファイルを追加する
vim 01linux7days-journals/day7.txt

# 実行ログの確認
sudo journalctl | grep CRON
```



