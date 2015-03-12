---
layout: post
title: CSSで親要素のサイズを基準に要素のサイズを決める。
category: technics
tags : [CSS, レイアウト]
description: position:absoluteとtop,bottom,left,rightを使って自由にレイアウトする。
---
# topとbottom、leftとrightを両方指定する。
`position: absolute`にした上で、
`top`と`bottom`、`left`と`right`に値を与えると、
それぞれ高さ、幅が親要素の端からオフセットした長さになる。

例えば、`left`は`10px`、`right`は`50%`のようにすると、左端から10pxオフセットした左半分を占める要素にできるなど、
`width`, `height`よりも柔軟性が高い。  
`top`、`bottom`、`left`、`right`を全て0にすると、親要素とピッタリ同じ大きさになる。

親要素は、`position:static`以外であれば良いので、`position:relative`などを設定する。
もちろん、`position: absolute`でも良いので、
何も考えなくても同じCSSをそのまま入れ子にできる。

absoluteなので兄弟要素の幅・高さを考慮して、オフセットする必要がある。
ヘッダー・フッターのような固定高さ要素とか、
コンテンツの分割レイアウトとか
の入れ物のレイアウトに適する。

## 例1 ヘッダー、コンテンツ、フッター型のレイアウト
<style>
  #test1-container {border:1px solid black; padding:0;margin:0; width:30em; height:20em; position:relative; }
  #test1-header {background-color:rgba(255,192,255,0.5); position:absolute; top:0;left:0;right:0; height:30px;}
  #test1-content{background-color:rgba(255,255,192,0.5); position:absolute; top:30px;right:0;bottom:30px;left:0;}
  #test1-footer {background-color:rgba(192,255,255,0.5); position:absolute; bottom:0;left:0;right:0; height:30px;}
  #test1-inner-header {background-color:rgba(255,192,255,0.5); position:absolute; top:0;left:50%;right:0; height:30px;}
  #test1-inner-content{background-color:rgba(255,255,128,0.5); position:absolute; top:30px;right:10%;bottom:30px;left:10%;}
  #test1-inner-footer {background-color:rgba(192,255,255,0.5); position:absolute; bottom:0;left:50%;right:0; height:30px;}
</style>
<div id="test1-container">
  <div id="test1-header">header</div>
  <div id="test1-content">content
    <div id="test1-inner-header">inner header</div>
    <div id="test1-inner-content">inner content
      <div id="test1-inner-header">inner inner header</div>
      <div id="test1-inner-content">inner inner content</div>
      <div id="test1-inner-footer">inner inner footer</div>
    </div>
    <div id="test1-inner-footer">inner footer</div>
  </div>
  <div id="test1-footer">footer</div>
</div>

~~~
<style>
  #test1-container {border:1px solid black; padding:0;margin:0; width:30em; height:20em; position:relative; }
  #test1-header {background-color:rgba(255,192,255,0.5); position:absolute; top:0;left:0;right:0; height:30px;}
  #test1-content{background-color:rgba(255,255,192,0.5); position:absolute; top:30px;right:0;bottom:30px;left:0;}
  #test1-footer {background-color:rgba(192,255,255,0.5); position:absolute; bottom:0;left:0;right:0; height:30px;}
  #test1-inner-header {background-color:rgba(255,192,255,0.5); position:absolute; top:0;left:50%;right:0; height:30px;}
  #test1-inner-content{background-color:rgba(255,255,128,0.5); position:absolute; top:30px;right:10%;bottom:30px;left:10%;}
  #test1-inner-footer {background-color:rgba(192,255,255,0.5); position:absolute; bottom:0;left:50%;right:0; height:30px;}
</style>
<div id="test1-container">
  <div id="test1-header">header</div>
  <div id="test1-content">content
    <div id="test1-inner-header">inner header</div>
    <div id="test1-inner-content">inner content
      <div id="test1-inner-header">inner inner header</div>
      <div id="test1-inner-content">inner inner content</div>
      <div id="test1-inner-footer">inner inner footer</div>
    </div>
    <div id="test1-inner-footer">inner footer</div>
  </div>
  <div id="test1-footer">footer</div>
</div>
~~~

## 例2 分割レイアウト
<style>
  #test2-container {border:1px solid black; padding:0;margin:0; width:30em; height:20em; position:relative; }
  .test2-v-col2 {position:absolute; display:inline-block; left:0;right:0;}
  .test2-v-col2:nth-of-type(1) {background-color:rgba(192,255,255,0.5); top:0;bottom:50%;}
  .test2-v-col2:nth-of-type(2) {background-color:rgba(255,192,255,0.5); top:50%;bottom:0;}
  .test2-h-col3 {position:absolute; display:inline-block; top:0;bottom:0;}
  .test2-h-col3:nth-of-type(1) {background-color:rgba(255,255,192,0.5); left:0;right:66.7%;}
  .test2-h-col3:nth-of-type(2) {background-color:rgba(255,192,255,0.5); left:33.3%;right:33.3%;}
  .test2-h-col3:nth-of-type(3) {background-color:rgba(192,255,255,0.5); left:66.7%;right:0;}
</style>
<div id="test2-container">
  <div class="test2-v-col2">row1</div>
  <div class="test2-v-col2">row2
    <div class="test2-h-col3">col1</div>
    <div class="test2-h-col3">col2</div>
    <div class="test2-h-col3">col3</div>
  </div>
</div>

~~~
<style>
  #test2-container {border:1px solid black; padding:0;margin:0; width:30em; height:20em; position:relative; }
  .test2-v-col2 {position:absolute; display:inline-block; left:0;right:0;}
  .test2-v-col2:nth-of-type(1) {background-color:rgba(192,255,255,0.5); top:0;bottom:50%;}
  .test2-v-col2:nth-of-type(2) {background-color:rgba(255,192,255,0.5); top:50%;bottom:0;}
  .test2-h-col3 {position:absolute; display:inline-block; top:0;bottom:0;}
  .test2-h-col3:nth-of-type(1) {background-color:rgba(255,255,192,0.5); left:0;right:66.7%;}
  .test2-h-col3:nth-of-type(2) {background-color:rgba(255,192,255,0.5); left:33.3%;right:33.3%;}
  .test2-h-col3:nth-of-type(3) {background-color:rgba(192,255,255,0.5); left:66.7%;right:0;}
</style>
<div id="test2-container">
  <div class="test2-v-col2">row1</div>
  <div class="test2-v-col2">row2
    <div class="test2-h-col3">col1</div>
    <div class="test2-h-col3">col2</div>
    <div class="test2-h-col3">col3</div>
  </div>
</div>
~~~

