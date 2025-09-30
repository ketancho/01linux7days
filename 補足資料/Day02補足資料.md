Day2 のハンズオンで利用するコマンドや情報を記載します。

# Day2-1: touch コマンドでファイルを作成し、ls コマンドで確認する
## touch コマンド
```bash
man touch

touch sample.txt
ls

touch sample2.txt
ls
```

## ls コマンド
```bash
man ls
ls
ls -l
ls -a
ls -la
```

# Day2-2: echo コマンドとリダイレクトを使って、ファイルに書き込みする
## echo コマンド
```bash
man echo
echo "Welcome to 01linux7day!"
echo "01linux7day へようこそ！"
```

## リダイレクト
```bash
cat sample.txt

echo "Welcome to 01linux7day!" > sample.txt
cat sample.txt

echo "01linux7day へようこそ！" > sample.txt
cat sample.txt

echo "追記テスト" >> sample.txt
cat sample.txt
```

# Day2-3: Vim 操作の基本① - 挿入モード
* Vim 日本語ドキュメント https://vim-jp.org/vimdoc-ja/intro.html

## Vim の練習
```bash
vim sample.txt
# i を押して挿入モード
# :wq + Enter で保存& vim を終了

cat sample.txt
```

# Day2-4: Vim 操作の基本② - コマンドラインモード
```bash
vim sample.txt

vim .bash_history
```

# Day2-5: Vim 操作の基本③ - ノーマルモード
```bash
vim sample.txt 
```

# Day2-6: cat / less / head / tail - ファイルの中身を閲覧する様々なコマンド
```bash
vim long-sample.txt

cat long-sample.txt

less long-sample.txt

head -5 long-sample.txt

tail -5 long-sample.txt

tail -f long-sample.txt
# 下記のコマンドは CloudShell の別タブを開いて実施します
echo "test" >> long-sample.txt
```

# Day2-7: mv / cp / rm - ファイル操作のためのコマンド
```bash
ls -l

mv long-sample.txt super-long-sample.txt
ls -l

cp super-long-sample.txt super-long-sample2.txt
ls -l

rm super-long-sample2.txt
ls -l

rm *.txt
ls -l
```
