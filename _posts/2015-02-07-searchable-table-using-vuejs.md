---
layout: post
title: vue.jsを使った検索可能なテーブル
category: examples
tags : [JavaScript, vue.js]
description: vue.jsを使ってインクリメンタルサーチとソートができるテーブルを作る。
---
vue.jsを使うと、余分なコードは一切なく驚くほど簡単にできる。

この例ではJavaScriptコードはほとんどデータを定義しているだけである。

なお、`filterBy ～ in searchKey`の`searchKey`はモデル名である。  
例えば、固定的に`name`プロパティに対して検索したい場合は、
`filterBy ～ in 'name'`のように`'`で囲う必要がある。  
`name`モデルが無い状態で、`filterBy ～ in name`だと全プロパティが検索対象になり、エラーも警告も出ないので注意。

## 例
{% raw %}
<div id="test-app">
  Search <input type="text" v-model="searchText" placeholder="type {{searchKey}}">
  in <select v-model="searchKey">
    <option>id</option>
    <option selected>name</option>
  </select>
  order by <select v-model="searchOrder">
    <option>id</option>
    <option selected>name</option>
  </select>
  <div>Search `{{searchText}}` in {{searchKey}} order by {{searchOrder}}</div>
  <table border="1">
    <tr>
      <th>ID</th>
      <th>Name</th>
    </tr>
    <tr v-repeat="items | filterBy searchText in searchKey | orderBy searchOrder">
      <td>{{id}}</td>
      <td>{{name}}</td>
    </tr>
  </table>
</div>
<script src="http://cdnjs.cloudflare.com/ajax/libs/vue/0.11.4/vue.min.js"></script>
<script>
  var vm = new Vue({
    el: '#test-app',
    data: {
      items: [
        {id: 1, name: 'lime'},
        {id: 2, name: 'orange'},
        {id: 3, name: 'cherry'},
        {id: 4, name: 'apple'},
        {id: 5, name: 'raspberry'},
        {id: 6, name: 'banana'}
      ]
    }
  });
</script>
{% endraw %}

~~~
<div id="test-app">
  Search <input type="text" v-model="searchText" placeholder="type {{searchKey}}">
  in <select v-model="searchKey">
    <option>id</option>
    <option selected>name</option>
  </select>
  order by <select v-model="searchOrder">
    <option>id</option>
    <option selected>name</option>
  </select>
  <div>Search `{{searchText}}` in {{searchKey}} order by {{searchOrder}}</div>
  <table border="1">
    <tr>
      <th>ID</th>
      <th>Name</th>
    </tr>
    <tr v-repeat="items | filterBy searchText in searchKey | orderBy searchOrder">
      <td>{{id}}</td>
      <td>{{name}}</td>
    </tr>
  </table>
</div>
<script src="http://cdnjs.cloudflare.com/ajax/libs/vue/0.11.4/vue.min.js"></script>
<script>
  var vm = new Vue({
    el: '#test-app',
    data: {
      items: [
        {id: 1, name: 'lime'},
        {id: 2, name: 'orange'},
        {id: 3, name: 'cherry'},
        {id: 4, name: 'apple'},
        {id: 5, name: 'raspberry'},
        {id: 6, name: 'banana'}
      ]
    }
  });
</script>
~~~

## vue.js参考サイト
- [Vue.jsとjQueryを使った時の違いについて考えてみる。 - 日頃の行い](http://arata.hatenadiary.com/entry/2014/12/23/040507)
- [Backbone.jsを使っている人間がVue.jsを触ってみました。 - Catcher in the tech](http://catcher-in-the-tech.net/801/)
- [お手軽データバインディングライブラリ「Vue.js」を使いこなそう（基礎編） - CodeZine](http://codezine.jp/article/detail/8363)

