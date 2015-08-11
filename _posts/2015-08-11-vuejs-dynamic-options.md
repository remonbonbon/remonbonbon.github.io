---
layout: post
title: Vue.js 0.12.10で追加されたselectタグ用options
category: howto
tags : [JavaScript, vue.js]
description: Vue.js 0.12.10で追加されたselectタグ用optionsを使って動的option生成を行う。
---
Vue.js 0.12.10 options
============================

### Before  (vue.js 0.12.9)
最初は`options`は存在せず、Vueの初期化後に`options`プロパティを初期化すると、
`<select>`の`v-model`は反映されず、常に先頭の`<option>`が選択されてしまう。

そのため、`selected`を`options`より後に初期化する必要があった。

~~~
<select v-model="selected">
	<option v-repeat="options" value="{{value}}">{{text}}</option>
</select>
~~~

### After (vue.js 0.12.10)
0.12.10で追加された`options`属性を使うと、後から`options`を初期化しても`v-model`が反映される。

セレクトボックスに表示される要素へは、`value`, `text`, `disabled`が決めうちで反映される。

~~~
<select v-model="selected" options="options">
</select>
~~~

JavaScript code:

~~~
var vm = new Vue({
  el: "#app",
  data: {
    options: [],
    selected: "user" + Math.round(Math.random() * 2 + 1),
    status: "loading options",
  },
  ready: function() {
    setTimeout(function() {
      this.options = [
        {value: "user0", text: "ユーザ0"},
        {value: "user1", text: "ユーザ1"},
        {value: "user2", text: "ユーザ2"},
        {value: "user3", text: "ユーザ3"},
      ];
      this.status = "done";
    }.bind(this), 500);
  }
});
~~~

### Example

{% raw %}
<div id="app">
  <p>Status: {{status}}</p>
  <p>Selected: {{selected}}</p>
  <p>Before (vue.js 0.12.9):
    <select v-model="selected" options="options">
    </select>
  </p>
  <p>After (vue.js 0.12.10):
    <select v-model="selected">
      <option v-repeat="options" value="{{value}}">{{text}}</option>
    </select>
  </p>
</div>
<script src="http://cdnjs.cloudflare.com/ajax/libs/vue/0.12.10/vue.min.js"></script>
<script>
var vm = new Vue({
  el: "#app",
  data: {
    options: [],
    selected: "user" + Math.round(Math.random() * 2 + 1),
    status: "loading options",
  },
  ready: function() {
    setTimeout(function() {
      this.options = [
        {value: "user0", text: "ユーザ0"},
        {value: "user1", text: "ユーザ1"},
        {value: "user2", text: "ユーザ2"},
        {value: "user3", text: "ユーザ3"},
      ];
      this.status = "done";
    }.bind(this), 500);
  }
});
</script>
{% endraw %}
