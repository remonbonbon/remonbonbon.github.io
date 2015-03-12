---
layout: post
title: browserify + bower + vueifyでWebクライアントサイドを作る
category: howto
tags : [JavaScript, vue.js, gulp, browserify, bower, vueify]
description: vueifyでコンポーネントごとにコードを分割する。
---
# 構成
- ビルドツール: gulp
  - browserify, bower, vueify
- AltJS: CoffeeScript
- CSS Preprocessor: Stylus
- Template engine: Jade
- テストツール: mocha

# インストール
以下をインストール。手順は省略。

- node.js
- git (bowerに必要)

# プロキシ設定

## npm
registryは、httpsをhttpに変える。

~~~
npm config set proxy http://username:password@hostname:port
npm config set https-proxy http://username:password@hostname:port
npm config set registry http://registry.npmjs.org/
~~~

## bower
`~/.bowerrc`

~~~
{
  "proxy": "http://username:password@hostname:port",
  "https-proxy": "http://username:password@hostname:port"
}
~~~

## git
~~~
git config --global http.proxy http://username:password@hostname:port
git config --global https.proxy http://username:password@hostname:port
~~~

# プロジェクトの作成
~~~
mkdir test-project; cd test-project
npm init
~~~

## npm install
~~~
npm install --save-dev glob gulp gulp-watch vinyl-source-stream browserify bower debowerify uglifyify vueify stylus jade coffeeify coffee-script mocha
~~~

## bower install
`node_modules/.bin/bower`にシンボリックリンクを張ろう。(もしくは`npm install --global bower`)
~~~
bower install --save vue jquery lodash
~~~

## gulpfile.coffee
`.on 'error' ～`が無いとwatchタスクでビルドエラー時に、watch自体が終了してしまう。

~~~
gulp = require 'gulp'
watch = require 'gulp-watch'
browserify = require 'browserify'
source = require 'vinyl-source-stream'
glob = require 'glob'

SRC_DIR = './src'
COMPONENTS_DIR = "#{SRC_DIR}/components"
SPEC_DIR = './spec'
SRC_FILES  = glob.sync "#{SRC_DIR}/*.coffee"
SPEC_FILES = glob.sync "#{SPEC_DIR}/*_spec.coffee"

gulp.task 'watch', ->
  gulp.watch ["#{SRC_DIR}/*.coffee", "#{COMPONENTS_DIR}/**/*.vue"], ['build']
  gulp.watch ["#{SPEC_DIR}/**/*_spec.coffee", "#{COMPONENTS_DIR}/**/*.vue"], ['build-test']

gulp.task 'build', ->
  browserify
    entries: SRC_FILES
  .transform 'coffeeify'
  .transform 'debowerify'
  .transform 'vueify'
  .transform 'uglifyify'
  .bundle()
  .on 'error', (err)->
    console.log err.toString()
    this.emit 'end'
  .pipe source 'build.js'
  .pipe gulp.dest './public/'

gulp.task 'build-test', ->
  browserify
    entries: SPEC_FILES
  .transform 'coffeeify'
  .transform 'debowerify'
  .transform 'vueify'
  .bundle()
  .on 'error', (err)->
    console.log err.toString()
    this.emit 'end'
  .pipe source 'build-test.js'
  .pipe gulp.dest './public-test/'
~~~

## ビルド
`node_modules/.bin/gulp`にシンボリックリンクを張ろう。(もしくは`npm install --global gulp`)
~~~
gulp build
~~~

# サンプルコード

## src/components/sample.vue
scssでもいいけどstylusもscssに互換性があるし公式推しっぽいので。
template engineとAltJSはjade, CoffeeScriptしか選択肢がない。

~~~
<style lang="stylus">
.test {background-color: #ffe}
</style>

<template lang="jade">
div.test {{message}}
</template>

<script lang="coffee">
module.exports = {
  data: -> {message: 'hello, world!'}
}
</script>
~~~

## src/main.coffee
追加ライブラリはbowerでインストールしrequireで読み込む。

~~~
Vue = require "vue"
vueSample = require "./components/sample.vue"

app = module.exports = new Vue {
  el: '#app',
  components: {
    "vue-sample": vueSample
  },
  data: {
    items: [
      {message: "hello"},
      {message: "world"}
    ]
  }
}
~~~

## public/index.html
`main.coffee`で`components`に書いた`vue-sample`というタグが使えるようになる。

~~~
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
</head>
<body id="app">
  <!-- ↓コンポーネント -->
  <vue-sample v-repeat="items"></vue-sample>
  <script src="build.js"></script>
</body>
</html>
~~~

## spec/sample_spec.coffee
specファイルを`build-test`タスクで1つのjsファイルにする。

~~~
Vue = require "vue"
vueSample = require "../src/components/sample.vue"

assert = require "assert"
describe 'Sample', ->
  describe '#data()', ->
    it 'message is hello, world!', ->
      assert.equal(vueSample.data().message, 'hello, world!')
~~~

## public-test/index.html
これをブラウザで直接開くとテストが実行される。

~~~
<html>
<head>
  <meta charset="utf-8">
  <title>Mocha Tests</title>
  <link rel="stylesheet" href="mocha.css" />
</head>
<body>
  <div id="mocha"></div>
  <script src="mocha.js"></script>
  <script>mocha.setup('bdd')</script>
  <script src="build-test.js"></script>
  <script>
    mocha.checkLeaks();
    mocha.run();
  </script>
</body>
</html>
~~~

# 参考サイト
- [gulpでbrowserify使ってcoffee-scriptの監視とコンパイル - Qiita](http://qiita.com/mizchi/items/10a8e2b3e6c2c3235e61)
- [Browserify: それはrequire()を使うための魔法の杖 - Qiita](http://qiita.com/cognitom/items/4c63969b5085c90639d4)

