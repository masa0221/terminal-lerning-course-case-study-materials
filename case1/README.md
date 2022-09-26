# Case1. 大量の誤字をいっきに書き換える

このディレクトリの中身

```
case1
├── my-quiz       // 修正対象のコンテンツ
└── sample-quiz   // 正しいコンテンツ
```


## STEP1. 状況を確認してみよう
### 1.1. 正しい状態のファイルとの差分を確認する
diff コマンドを使うとファイルを比較することができます。

書式:  
```
% diff FILE1 FILE2
```

diff コマンドはディレクトリも比較できます。

例
```sh
diff -uw my-quiz sample-quiz
```
- `-u` 差分があった箇所の前後も表示
- `-w` whitespace(空白)を無視

### 1.2. 誤字を検索する
grep で検索する
```
grep -r ansers .
```

ag （the-silver-searcher） で検索する
```
ag ansers
```

### 1.3. ファイル名のみを表示する
grep で検索でマッチしたファイルのパスを表示
```
grep -r ansers . -l
```
さらに `--include "*html"` をつけると html だけに絞ることができる

ag （the-silver-searcher） で検索でマッチしたファイルのパスを表示
```
ag ansers -l
```
さらに `-G html` をつけると html だけに絞ることができる


grep -v を使って特定の対象を省く手もある
```
ag ansers -l | grep -v README.md
```


## STEP2. ファイルの内容を書き換えてみよう

sed(stream-editor)コマンドを使うと大量のファイルも一気に書き換え可能です。

書式 macOS(BSDのsed)の場合:  
```
% sed -i '' -e 's/対象文字の正規表現/対象文字の置換後の文字/g' 対象ファイルパス
```

書式 Ubuntu(GNUのsed)の場合:  
```
% sed -i -e 's/対象文字の正規表現/対象文字の置換後の文字/g' 対象ファイルパス
```

例 Ubuntu(GNUのsed)の場合: 
```sh
sed -i -e 's/ansers/answers/g' ./my-quiz/style.css
```
- `-i` そのファイル自体を編集 (macOSの場合　`-i ''` で同じ意味になる)
- `-e` sed のコマンドを指定

### 補足1
macOSでGNUのsedを使いたい場合は、 `brew install gsed` でインストールをしましょう。  
gsed コマンドをでGNUと同じ仕様のsedが利用できます。

### 補足2
```
diff <(cat ./my-quiz/style.css) <(sed -e 's/ansers/answers/g' ./my-quiz/style.css)
```
こうすると、丸かっこの中のコマンドの結果をファイルとして扱うことができます。  
つまりコマンドの結果を比較できます。

Process substitution - Wikipedia  
https://en.wikipedia.org/wiki/Process_substitution
`

## STEP3. 複数ファイルの内容を書き換えてみよう
- 3.1. grep で対象のファイルを探しファイル名を一覧にする
- 3.2. xargs を使って sed にファイル名を渡して置換する

例 Ubuntu(GNUのsed)の場合: 
```sh
grep -r '<br />' . -l --include "*html" | xargs -p sed -i -e 's/<br \/>/<br>/g'
```
- grepのオプション
  - `-l` ファイル名のみを表示
  - `--include` 対象ファイルのパターンを指定
- xargsのオプション
  - `-p` 実行前に確認(yかYで実行する)

※ macOSの場合は、 `gsed` を使うか、 `sed` の `-i` オプションを `-i ''` に変えましょう


### 補足1
sed の 引数が分かりにくい時は以下のように書くこともできます。
```
grep -r '<br />' . -l --include "*html" | xargs sed -i -e 's#<br />#<br>#g'
```
区切り文字を / から # に変えることができます。(区切り文字が `/` ではないので `/`を`\`でエスケープする必要がない )

- 区切り文字(`/`): `s/regex/string/g`
- 区切り文字(`#`): `s#regex#string#g`
