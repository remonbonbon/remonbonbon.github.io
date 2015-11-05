---
layout: post
title: eslintの設定
category: memo
tags : [設定, ESlint]
description: vim, iptablesなどの設定値
---

### Default rules

~~~
// Default rules for all
{
  // Initialize rules
  "extends": "eslint:recommended",

  // Change from default rules
  "rules": {
    // ------------------- Possible Errors -------------------
    // Allow trailing comma
    "comma-dangle": 0,
    // Warn for missing semicolon
    "no-unexpected-multiline": 1,

    // ------------------- Best Practices -------------------
    // Warn when scoped variables are used outside of the block.
    "block-scoped-var": 1,
    // Disallow: == and !=
    "eqeqeq": 2,
    // Allow: hoge == null
    // ex) options.num = options.num == null ? defaultNum : options.num
    "no-eq-null": 0,
    // Warn: parseInt("071");  // => 57
    "radix": 1,

    // ------------------- Strict Mode -------------------
    // use strict
    "strict": 2,

    // ------------------- Variables -------------------
    // Warn variable shadowing
    "no-shadow": 1,
    // Warn unused without arguments
    "no-unused-vars": [1, {"vars": "all", "args": "none"}],

    // ------------------- Node.js -------------------

    // ------------------- Stylistic Issues -------------------
    // ex) [hoge, piyo, fuga]
    "array-bracket-spacing": [1, "never"],
    // use trailing {
    "brace-style": [2, "1tbs", { "allowSingleLine": true }],
    // use camelCase
    "camelcase": [2, {"properties": "always"}],
    // ex) var hoge = 1, piyo = 2;
    "comma-spacing": [1, {"before": false, "after": true}],
    // use comma at last,
    "comma-style": [2, "last"],
    // ex) obj['property']
    "computed-property-spacing": [1, "never"],
    // use TAB for indentation
    "indent": [2, "tab", {"SwitchCase": 1}],
    // use LF
    "linebreak-style": [2, "unix"],
    // ex) func()
    "no-spaced-func": 1,
    // No trailing spaces
    "no-trailing-spaces": 1,
    // ex) {hoge: 'piyo'}
    "object-curly-spacing": [1, "never"],
    // ex) {'as-needed': hoge}
    "quote-props": [2, "as-needed"],
    // use 'single quote'
    "quotes": [2, "single", "avoid-escape"],
    // use semicolon always;
    "semi" : 2,
    // ex) if (hoge)
    "space-after-keywords": 1,
    // ex) if (hoge) {
    "space-before-blocks": 1,
    // ex) function hoge() {
    "space-before-function-paren": [1, "never"],
    // ex) hoge(piyo, fuga)
    "space-in-parens": [1, "never"],
    // ex) hoge + piyo
    "space-infix-ops": 1,
    // ex) return -hoge;
    "space-return-throw-case": 1,

    // ------------------- ECMAScript 6 -------------------

    // ------------------- Legacy -------------------

    "__END_OF_CONTENT__": 0
  }
}
~~~

### Extend default rules

~~~
{
  "extends": "./eslint-default.json",
  "env": {
    "node": true
  }
}
~~~
