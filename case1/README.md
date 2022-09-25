## Case1. 大量の誤字をいっきに書き換える

このディレクトリの中身

```
case1
├── my-quiz       // 修正対象のコンテンツ
└── sample-quiz   // 正しいコンテンツ
```


### 差分を比較してみよう

diff コマンドを使うとファイルを比較することができます。

書式:  
```
diff FILE1 FILE2
```

diff コマンドはディレクトリも比較できます。

例
```sh
diff my-quiz sample-quiz
```

### ファイルの内容を書き換えてみよう

sed(stream-editor)コマンドを使うと大量のファイルも一気に書き換え可能です。

書式 macOS(BSD)の場合:  
```
sed -i '' -e 's/対象文字の正規表現/対象文字の置換後の文字/g' 対象ファイルパス
```

書式 Ubuntu(GNU)の場合:  
```
sed -i -e 's/対象文字の正規表現/対象文字の置換後の文字/g' 対象ファイルパス
```

例 Ubuntu(GNU)の場合: 
```
sed -i -e 's/ansers/answers/g' ./my-quiz/style.css
```

#### 補足
macOSでGNUのsedを使いたい場合は、 `brew install gsed` でインストールをしましょう。  
gsed コマンドをでGNUと同じ仕様のsedが利用できます。
`

### 複数ファイルの内容を書き換えてみよう

