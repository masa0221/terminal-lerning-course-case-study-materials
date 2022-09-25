# Case3. CSV / TSV ファイルを取り扱う 

データの提供元  
出典：政府統計の総合窓口(e-Stat)（ https://www.e-stat.go.jp/ ）

## STEP1. ファイルの文字コードを確認する
以下を実行すると文字化けした内容が出力されます
```
head c01.csv
```

以下でファイルの文字コードを確認します
```
% file --mime c01.csv
```

## STEP2. 文字コードを変換して文字化けしないようにする
### 方法1: iconv を使う
元ファイルの文字コードがわかる場合はiconvコマンドを使って変換することができる
```
head c01.csv | iconv -f shift-jis -t utf-8
```

### 方法2: nkf を使う
日本語ファイルで文字コードが不明な場合は、nkfを使うと良い

macOSの場合
```
% brew install nkf
```

Ubuntuの場合
```
% apt install nkf
```

#### nkfのかんたんな使い方
文字コードを確認
```
% nkf -g ./c01.csv
```

文字コードを自動判定して出力を変換( 引数で指定する場合 )
```
% nkf ./c01.csv
```

文字コードを自動判定して出力を変換( 標準入力に渡す場合 )
```
% cat ./c01.csv | nkf
```


## STEP3. CSVやTSVを好きに加工する

CSVをTSV（タブ区切り）にする
```
% head ./c03.csv | nkf | head | tr ',' '\t'
```

沖縄県の情報だけ出力
```
% cat ./c03.csv | nkf | grep -v '総数には年齢「不詳」を含む' | awk -F, '$2 == "沖縄県" { print $0 }' | head
```

ヘッダなしで出力する
```
% tail -n +2 ./c03.csv | nkf | head
```
awkでcsvに任意の列を追加する - Qiita.  
https://qiita.com/tochiji/items/284daafaedec941afdca



