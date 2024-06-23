---
title: "条件演算子(三項演算子)で書いてみよう"
emoji: "😆"
type: "tech"
topics: "javascript"
published: true
date: '2024.06.23'
---

## 条件演算子とは

条件演算子とは*条件分岐を記述するための演算子*で、次のような構文で書けます。

```jsx
条件式 ? 条件式が真の時の式 : 条件式が偽の時の式
```

このように3つのオペランド(被演算子)を持つことから、三項演算子とも呼ばれます。

## 条件演算子とif文の違い

条件分岐といえば、まず*if文*が思い浮かびますよね。

では、*条件演算子*と*if文*は何が違うのでしょうか？

１つ目の違いは、書き方です。

例として、テストの点数が80点未満の場合"不合格"、80点以上の場合"不合格"と出力する関数を2つの書き方で書いてみます。

```jsx
// if文 ------------------------------------
const checkPassOrFail = (score) => {
    if(score >= 80) {
        return "合格"
    } else {
        return "不合格"
    }
}

console.log(checkPassOrFail(85)) // 合格
console.log(checkPassOrFail(75)) // 不合格

// 条件演算子 ------------------------------------
const checkPassOrFail = (score) => {
    return score >= 80 ? "合格" : "不合格"
}

console.log(checkPassOrFail(85)) // 合格
console.log(checkPassOrFail(75)) // 不合格
```

このように、*条件演算子で書くとかなり簡潔に書けます*。

2つ目の違いは、if文は名前からも分かるとおり*文*なのに対し、条件演算子は*式*であるという違いもあります。

よって条件演算子は、*文が書けない場所にも書くことができます*。

例えばReactのJSXコンポーネント内で条件演算子を使うと、親コンポーネントから受け取ったpropsを評価して表示する内容を変えることができます。

```jsx
// if文 ------------------------------------
import React from 'react'

const CheckPassOrFail = ({ score }) => {
    return (
        <div>
            {score >= 80
                ? <p>あなたは合格です</p>
                : <p>あなたは不合格です</p>
            }
        </div>
    )
}

export default CheckPassOrFail
```

このように、条件付きのレンダリングを行うことができます。

## どのように使い分けるか

まず、当然ですが文が書けない場所では条件演算子を使います。

あとは、*読みやすさやコードの簡潔さを意識して使い分ける*必要があります。

例えば、次のようなコードの場合は条件演算子の方が簡潔で読みやすいと思います。

```jsx
// if文 ------------------------------------
const checkPassOrFail = (score) => {
    if(score >= 80) {
        return "合格"
    } else {
        return "不合格"
    }
}

console.log(checkPassOrFail(85)) // 合格
console.log(checkPassOrFail(75)) // 不合格

// 条件演算子 ------------------------------------
const checkPassOrFail = (score) => {
    return score >= 80 ? "合格" : "不合格"
}

console.log(checkPassOrFail(85)) // 合格
console.log(checkPassOrFail(75)) // 不合格
```

しかし、次の場合はどうでしょう？

```jsx
// if文 ------------------------------------
const checkPassOrFail = (score) => {
    if(score >= 80) {
        return `合格（あと${100 - score}点で満点です！）`
    } else {
        return `不合格（あと${80 - score}点で合格です！）`
    }
}

console.log(checkPassOrFail(85)) // 合格(あと15点で満点です！)
console.log(checkPassOrFail(75)) // 不合格(あと5点で合格です！)

// 条件演算子 ------------------------------------
const checkPassOrFail = (score) => {
    return score >= 80 ? `合格（あと${100 - score}点で満点です！）` : `不合格（あと${80 - score}点で合格です！）`
}

console.log(checkPassOrFail(85)) // 合格(あと15点で満点です！)
console.log(checkPassOrFail(75)) // 不合格(あと5点で合格です！)
```

難しい処理をしているわけでもないのに、なんだか冗長で読みづらいですよね...

こういう時はif文を使うべきだと思います。

## まとめ

リーダブルコードの言葉を引用すると、

>基本的にはif/elseを使おう。条件演算子（三項演算子）はそれによってコードが簡潔になるときだけ使おう。

です！