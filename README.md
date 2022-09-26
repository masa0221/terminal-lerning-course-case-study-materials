# ケーススタディ

## 進め方
各ケーススタディを行う時は、該当するディレクトリ以下にカレントディレクトリを移動して操作しましょう。  
各ケーススタディ用のディレクトリには、説明が書いているREADME.mdファイルがあります。  
README.mdを読みながら進めてください。

### ディレクトリ構成
以下のような構成になっています。
```
.
├── README.md
├── case1
│   ├── README.md
│   ├── my-quiz
│   └── sample-quiz
├── case2
│   ├── README.md
│   └── access_log
└── case3
    ├── README.md
    ├── c01.csv
    ├── c02.csv
    ├── c03.csv
    └── futariijou3.csv
```
各ケースのディレクトリには、利用するコマンドが書いているREADME.mdと、利用するサンプルファイルが入っています。

## git の操作
ファイルのバージョン管理を行うgitですが、ここではローカルでこのリポジトリを操作するための方法だけ紹介します。  
（gitの話をすると主旨が変わるので割愛します ※詳しく知りたい場合は、 git を学びましょう。）  

### git のインストール
各種パッケージマネージャを使ってgitをインストールをしてください。

macOS
```
brew install git
```
Ubuntu
```
apt install git
```

### `git clone`: リポジトリをローカルに複製 （ファイルをローカルにダウンロード)
```
git clone https://github.com/masa0221/terminal-lerning-course-case-study-materials.git
```

### `git diff`: 変更した内容を比較（バージョン管理されていない変更とバージョン管理されている状態を比較)
```
git diff
```

### `git restore`: 指定したファイルやディレクトリ以下のファイルを元に戻す(バージョン管理されているファイルに限る)
```
git restore PATH
```
PATHで指定したディレクトリ以下のファイルを変更前の状態に戻す

例: カレントディレクトリ以下の変更を全て元に戻す
```
git restore .
```

### `git clean`: 管理していないファイル、ディレクトリを削除
```
git clean -df
```

