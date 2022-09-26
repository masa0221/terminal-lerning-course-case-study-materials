# Case2. 大量のアクセスログから状況を把握する

## アクセスログについて
このディレクトリにある `access_log`の中には、Webサーバー(Apache)のアクセスログが入っています。  


## STEP1. アクセスログのファイルの行数を確認する
いきなり検索するより、ファイルのサイズはざっくりと把握しておきましょう。

```sh
wc -l ./access_log
```


## STEP2. ログのフォーマットを理解する
ログ内容がどのようなものか確認をするために、先頭だけ表示してみましょう。
```sh
head access_log
```

次に必要なフィールドだけ出力してみましょう
```
head access_log | awk '{ print $1 }'
```

### 補足1: Apacheのログフォーマットについて
今回使用したサーバーのログフォーマット(Apache)は以下です
```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\""
```
%h: リモートホスト(サーバーに接続してくるクライアントのホスト名)

詳しい意味はこちら  
https://httpd.apache.org/docs/2.4/ja/mod/mod_log_config.html


### 補足2: awk(オーク)
awk は特定のパターンを処理するための言語です。  
特定のルールがある文字の操作を行いやすいので以下の例だけでも覚えておくと損はないでしょう。

例： スペース区切りの文字の3番目を出力
```
echo "foo bar baz" | awk '{ print $3 }'
```

例： カンマ区切りの文字の2番目を出力
```
echo "foo,bar,baz" | awk -F, '{ print $2 }'
```
※ -F オプションで区切り文字を指定している


## STEP3. データの件数を数える
ログは大体のケースで膨大な量になるので、まずはどんなログが多いか件数を確認することはよくあります。  
ここでは、サーバーに接続してきたクライアントのIPアドレスの件数を集計してみましょう。

head でテストをする
```
head access_log | awk '{ print $1 }' | sort | uniq -c | sort -nr
```

ログ全体の件数を確認
```
cat access_log | awk '{ print $1 }' | sort | uniq -c | sort -nr
```

上位のものだけを出力
```
cat access_log | awk '{ print $1 }' | sort | uniq -c | sort -nr | head
```

### 補足
いろんなフィルターを使って自由に操作してみましょう。

例: 上位20件を表示
```
cat access_log | awk -F\" '{ print $2 }' | sort | uniq -c | sort -nr | head -n 20
```

例: GETのリクエストだけに絞る
```
cat access_log | awk -F\" '{ print $2 }' | sort | uniq -c | sort -nr | grep GET | head
```

例: GETやPOST以外のリクエストだけに絞る
```
cat access_log | awk -F\" '{ print $2 }' | sort | uniq -c | sort -nr | grep -vE 'GET|POST' | head
```


