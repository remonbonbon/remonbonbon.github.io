---
layout: post
title: boost/geometryで凹多角形の当たり判定を行う
category: howto
tags : [C++, boost]
description: boost/geometryの凹多角形の当たり判定を使ってみる
---
# 対象バージョン
boost 1.57

# 主な型
~~~
#include <boost/geometry/geometry.hpp>
#include <boost/geometry/geometries/point_xy.hpp>
#include <boost/geometry/geometries/polygon.hpp>
#include <boost/geometry/geometries/box.hpp>

using point = boost::geometry::model::d2::point_xy<double>;
using polygon = boost::geometry::model::polygon<point>;
using box = boost::geometry::model::box<point>;
~~~

# ポリゴンの作成
boost/geometryは穴あき凹多角形に対応している。
凹んでたり、穴が(複数)開いていたりしても大丈夫。

外側の線を`outer()`、内側の穴の線を`inners()`で取得・設定できる。
初期状態では穴が空いていないため、`polygon::ring_type`を`push_back()`して穴を追加する。

~~~
polygon poly;

// add vertex of outer-ring
poly.outer().push_back(point(x, y));

// add new inner-ring
poly.inners().push_back(polygon::ring_type());
// add vertex of inner-ring
poly.inners().back().push_back(point(x, y));
~~~

# bounding boxの計算
ポリゴンのbounding boxは以下のように簡単に得られる。

~~~
box bb;
boost::geometry::envelope(poly, bb);
~~~

# ポリゴンと点の当たり判定
`boost::geometry::within()`を使うと簡単に当たり判定ができるのだが、
とても遅いため、最低でもbounding boxで最初に判定した方が良い。

~~~
point pt(x, y)
if (boost::geometry::within(pt, bb) &amp;&amp; boost::geometry::within(pt, poly)) {
  // hit!
}
~~~

# 参考
[http://boostjp.github.io/tips/geometry.html](http://boostjp.github.io/tips/geometry.html)
