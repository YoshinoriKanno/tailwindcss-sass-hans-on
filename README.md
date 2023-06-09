# tailwindcss + Sass の hans-on

tailwindcss + Sass の hans-on 用のrepository です。

Quick Start のみで hans-on 用の環境を構築できます。

環境構築方法も記述しました。興味がある方は参考ください。

# Quick Start

こちらの repository から Download ZIP して解凍してください。

finder で解凍したフォルダを右クリックして「フォルダに新規ターミナル」を選択、

ターミナル.app で node package をインストールします。



```sh
npm install
```
tailwindcss と Sass コンパイラーがインストールされます。

## watch の実行

下記コマンドでファイルの変更を監視してください。

```sh
npm run watch
```

index.html の
```html
<h1>Hello tailwindcss + Sass</h1>
```

を

```html
<h1 class="text-center">Hello tailwindcss + Sass</h1>
```

とクラスを追加してみてください。

```dist/tailwind.css``` に

```
.text-center {
  text-align: center;
}
```
が追記されます。

```src/styles/style.scss``` を編集すると、```dist/styles/``` に ```style.css``` が作成されます。

### watch の終了

ターミナル.app にて ``` controle + c ``` で tailwindcss watch を終了します。

以下に環境構築方法があります。興味がある方は参考ください。

# 環境構築

環境構築の方法は以下を確認してください。


## 作業ディレクトリの作成

今回はデスクトップに作成します。

ターミナル.app にて下記コマンドを実行してください。

```sh
mkdir ~/Desktop/tailwindcss-sass-hans-on && cd ~/Desktop/tailwindcss-sass-hans-on
```

### 訳

> デスクトップに tailwindcss-sass-hans-on というフォルダを作って、tailwindcss-sass-hans-on に移動する


## HTML ファイル作成

dist フォルダを作成して、その中に index.html を作成しjます。

ターミナル.app にて下記コマンドを実行してください。


```sh
mkdir dist && touch dist/index.html
```

index.html の内容を下記のように書き換えてください。
```html
<!DOCTYPE html>
<html lang="ja">

  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="styles/tailwind.css">
    <link rel="stylesheet" href="styles/style.css">
  </head>

  <body>
    <h1>Hello World</h1>

  </body>

</html>

```

### ここまでのディレクトリ構造

```sh
.
└── dist
    └── index.html

```

## npm のインストール

npm をイニシャライズします。

ターミナル.app にて下記コマンドを実行してください。

```sh
npm init -y
```

ルートディレクトリに package.json ファイルが作成されます。


```sh
.
├── dist
│   └── index.html
└── package.json
```



## node のバージョンを固定

volta で node のバージョンを固定します。

ターミナル.app にて下記コマンドを実行してください。


```sh
volta pin node@18.16.0
```
package.json に下記が追記されます。

```json
"volta": {
  "node": "18.16.0"
}
```


## tailwindcss と sass のインストール

tailwindcss をインストールします。

ターミナル.app にて下記コマンドを実行してください。


```sh
npm install -D tailwindcss sass
```
package.json に下記が追記されます。

```json
"devDependencies": {
  "sass": "^1.62.1",
  "tailwindcss": "^3.3.2"
}
```

```sh
.
├── dist
│   └── index.html
├── node_modules
├── package-lock.json
└── package.json
```



## tailwindcss のイニシャライズ

tailwindcss をイニシャライズしします。

ターミナル.app にて下記コマンドを実行してください。


```sh
npx tailwindcss init
```
ルートに tailwind.config.js が生成されます。

```sh
.
├── dist
│   └── index.html
├── node_modules
├── package-lock.json
├── package.json
└── tailwind.config.js
```


## tailwindcss の設定

tailwind.config.js の内容を下記のように書き換えます。

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./dist/**/*.{html,js}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

これで ```./dist``` 配下の全ての HTML ファイル、 JavaScript ファイルを監視します。



## tailwindcss のディレクティブ指定

ディレクティブ指定ファイルを作成します。

ターミナル.app にて下記コマンドを実行してください。


```sh
mkdir src && touch src/tailwindcss-input.css
```

```sh
.
├── dist
│   └── index.html
├── node_modules
├── package-lock.json
├── package.json
├── src
│   └── tailwindcss-input.css
└── tailwind.config.js
```



src/input.css の内容を書き換えます。

```css
/* @tailwind base; */
/* @tailwind components; */
@tailwind utilities;
```

## tailwindcss 実行の npm script 作成

package.json に npm script を追記します。

```json
"scripts": {
    "watch:tailwindcss": "npx tailwindcss -i ./src/tailwindcss-input.css -o ./dist/styles/tailwind.css --watch"
},
```


## SASS の環境設定

### SCSS ファイルの作成

```sh
mkdir src/styles && touch src/styles/style.scss
```

```sh
.
├── dist
│   └── index.html
├── node_modules
├── package-lock.json
├── package.json
├── src
│   ├── styles
│   │   └── style.scss
│   └── tailwindcss-input.css
└── tailwind.config.js
```



npm scripts に sass の実行コマンドを追記します。

```json
"scripts": {
  "watch:tailwindcss": "npx tailwindcss -i ./src/tailwindcss-input.css -o ./dist/styles/tailwind.css --watch",
  "watch:sass": "sass --no-source-map --watch src/styles/:dist/styles/"
},

```




## npm script watch を設定

npm scripts で tailwindcss と sass の実行コマンドをまとめます。

```json
"scripts": {
  "watch": "npm run watch:sass & npm run watch:tailwindcss",
  "watch:tailwindcss": "npx tailwindcss -i ./src/tailwindcss-input.css -o ./dist/styles/tailwind.css --watch",
  "watch:sass": "sass --no-source-map --watch src/styles/:dist/styles/"
},
```
