---
layout: post
title: jQuery.Deferredを使って処理を並列化・直列化
category: howto
tags : [JavaScript,jQuery,ajax]
description: jQuery.Deferredを使って処理を並列化したり直列化したりする方法。
---
基本
================
ajaxでない処理の場合。
`deferred.resolve()`をコールすることで、続く`then()`を実行することができる。


## 直列つなぎ
`deferred.then()`で接続する。

~~~
$.Deferred().resolve()
.then(function() {
  console.log("serial 1");
})
.then(function() {
  console.log("serial 2");
});
~~~

### つなぐ個数が可変の場合
~~~
var n = 10;
var defer = $.Deferred().resolve();
for (var i=0; i<n; i++) {
  defer = defer.then(function() {
    console.log("serial", i);
  });
}
defer.done(function() {
  console.log("done");
});
~~~

## 並列つなぎ
`jQuery.when()`の引数に与える。

~~~
$.when(
  $.Deferred().resolve().then(function() {
    console.log("parallel 1");
  }),
  $.Deferred().resolve().then(function() {
    console.log("parallel 2");
  })
);
~~~

$.ajax
================
`$.ajax`の戻り値が`then()`やら`done()`などのメソッドを持っているため、より簡潔に書くことができる。
`then()`に渡すfunctionにはajaxで得たレスポンスが入る。

## 直列つなぎ
~~~
$.Deferred().resolve()
.then(function() {
  return $.ajax("https://api.github.com/repositories");
})
.then(function(res1) {
  console.log(res1)
  return $.ajax("https://api.github.com/users");
})
.then(function(res2) {
  console.log(res2)
});
~~~

## 並列つなぎ
~~~
$.when(
  $.ajax("https://api.github.com/repositories"),
  $.ajax("https://api.github.com/users")
).then(function(res1, res2) {
  console.log(res1, res2);
});
~~~

CoffeeScript
================
functionとreturnが無くなることでとてもシンプルに。

## 直列つなぎ
~~~
$.Deferred().resolve()
.then ->
  $.ajax "https://api.github.com/repositories"
.then (res1)->
  console.log res1
  $.ajax "https://api.github.com/users"
.then (res2)->
  console.log res2
~~~

## 並列つなぎ
~~~
$.when(
  $.ajax("https://api.github.com/repositories"),
  $.ajax("https://api.github.com/users")
).then (res1, res2)->
  console.log res1, res2
~~~

複合
================

## 直列 - 並列 - 直列
~~~
$.ajax("https://api.github.com/search/users?q=uchi")
.then((res)-> console.log "#{res.total_count} hits")
.then(-> $.when(
  $.ajax("https://api.github.com/search/users?q=yamauchi"),
  $.ajax("https://api.github.com/search/users?q=kawauchi")
))
.then((a,b)-> console.log "#{a[0].total_count} hits", "#{b[0].total_count} hits")
~~~

## 直列処理の並列化
~~~
$.when(
  $.Deferred().resolve()
  .then( -> $.ajax("https://api.github.com/search/repositories?q=jquery"))
  .then( -> console.log "parallel 1"),
  $.Deferred().resolve()
  .then( -> $.ajax("https://api.github.com/search/repositories?q=ajax"))
  .then( -> console.log "parallel 2"),
  $.Deferred().resolve()
  .then( -> $.ajax("https://api.github.com/search/repositories?q=github"))
  .then( -> console.log "parallel 3")
).then ->
  console.log "done"
~~~
