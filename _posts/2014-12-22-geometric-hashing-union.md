---
layout: post
title: unionで幾何学的ハッシュを実装する
category: technics
tags : [C++, ハッシュ, 幾何学的ハッシュ]
description: 当たり判定等の高速判定に使う幾何学的ハッシュをunionにより簡単に実装する。
---
# unionを使う
unionを使うと幾何学的ハッシュ(Geometric hashing)を簡単に実装できる。
ビットシフトしてもいいけど、この方が分かりやすい。

3次元にする場合は、64bitに収まるようにbitフィールドで調整する。
`int16_t`を4つ並べて、3つだけ使っても良い。

なお、ハッシュ化する際に浮動小数に乗ずる数(`GEOHASH_FACTOR`)がパフォーマンスに影響する。
この値は、ハッシュ化される浮動小数の値域によって最適値が異なる。  
小さすぎると、1つのハッシュ値にたくさん集まりすぎて走査に時間がかかる。
大きすぎると、ハッシュキーの衝突が増えて、ハッシュテーブルからの取得に時間がかかる。

~~~
static const double GEOHASH_FACTOR = 1000; 
union GeoHash {
  struct {
    int32_t x;
    int32_t y;
  } grid;
  uint64_t hash;

  GeoHash(const uint64_t hash_) : hash(hash_) {}
  GeoHash(const int32_t x, const int32_t y) : grid({x, y}) {}
  GeoHash(const double x, const double y)
    : GeoHash(static_cast<int32_t>(x * GEOHASH_FACTOR), static_cast<int32_t>(y * GEOHASH_FACTOR)) {}
}
~~~

## ある範囲のハッシュ値列挙
unionを使うことで、x,yの取り出しが容易となり、ハッシュ値の列挙もやりやすい。

~~~
#include <unordered_map>

std::unordered_map<GeoHash, Hoge> geo_hash_table;
const GeoHash min(10.0, 20.5);
const GeoHash max(23.4, 35.7);
for (auto y=min.grid.y; y<=max.grid.y; y++) {
  for (auto x=min.grid.x; x<=max.grid.x; x++) {
    geo_hash_table[GeoHash(x,y).hash] = Hoge(piyo);
  }
}
~~~
