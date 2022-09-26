# Case1. 大量の誤字をいっきに書き換える

このディレクトリの中身

```
case1
├── my-quiz       // 修正対象のコンテンツ
└── sample-quiz   // 正しいコンテンツ
```


## STEP1. 差分を比較してみよう

diff コマンドを使うとファイルを比較することができます。

書式:  
```
% diff FILE1 FILE2
```

diff コマンドはディレクトリも比較できます。

例
```sh
diff my-quiz sample-quiz
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
1. grep で対象のファイルを探しファイル名を一覧にする
2. xargs を使って sed にファイル名を渡して置換する

例 Ubuntu(GNUのsed)の場合: 
```sh
grep -r '<br />' . -l --include "*html" | xargs sed -i -e 's/<br \/>/<br>/g'
```
※ macOSの場合は、 `gsed` を使うか、 `sed` の `-i` オプションを `-i ''` に変えましょう


### 補足1
sed の 引数が分かりにくい時は以下のように書くこともできます。
```
grep -r '<br />' . -l --include "*html" | xargs sed -i -e 's#<br />#<br>#g'
```
区切り文字を / から # に変えることができます。

- 区切り文字(/): `s/regex/string/g`
- 区切り文字(#): `s#regex#string#g`
