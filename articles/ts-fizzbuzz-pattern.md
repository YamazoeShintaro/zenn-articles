---
title: "【TypeScript】FizzBuzzのいろんな書き方"
emoji: "🗼"
type: "tech"
topics: ["typescript", "fizzbuzz", "初心者", "入門", "コーディング"]
published: true
date: '2024.06.25'
---

## FizzBuzzとは

FizzBuzzは、プログラミングの練習問題として有名で、次のようなルールで１から順番に整数を出力するプログラムです。

・３の倍数ではなく、５の倍数でもないときは整数をそのまま出力する
・３の倍数であり、５の倍数でない時は整数の代わりにFizzと出力する
・３の倍数でなく、５の倍数であるときは整数の代わりにBuzzと出力する
・３の倍数であり、５の倍数でもある時は整数の代わりにFizzBuzzと出力する

例えば、1から15までFizzBuzzを行うと次のように出力されます。

```jsx
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
```

今回は、このプログラムのいろいろな書き方を考えてみたいと思います。

## パターン１　：　普通

```jsx
for(let num = 1; num <= 15; num++) {
    if (num % 3 === 0 && num % 5 === 0) console.log('FizzBuzz');
    if (num % 5 === 0) console.log('Buzz');
    if (num % 3 === 0) console.log('Fizz');

    console.log(num.toString());
}
```

ちなみに、`num % 3 === 0 && num % 5 === 0`の部分は`num % 15 === 0`でもOKですが、私は`num % 3 === 0 && num % 5 === 0`の方が良いかと思います。

理由としては、単純に分かりやすいからです。

今回は`3`と`5`なのでどちらでも良かったですが、例えば問題（要件）に変更が入り`6`と`8`になったとします。

すると、`num % 6 === 0 && num % 8 === 0`か`num % 24 === 0`になりますね。

この`24`という数字は、`6`と`8`の最小公倍数をとっているだけなのですが、パッとみただけではわからないかもしれません。

よって、分かりやすさとメンテナンス性から`num % 3 === 0 && num % 5 === 0`が良いと思います。

## パターン２　：　ループの中の処理を関数に抜き出す

```jsx
const getFizzBuzz = (num: number): string => {
    if (num % 3 === 0 && num % 5 === 0) return 'FizzBuzz';
    if (num % 5 === 0) return 'Buzz';
    if (num % 3 === 0) return 'Fizz';

    return num.toString();
};

for(let num = 1; num <= 15; num++) {
    console.log(getFizzBuzz(num));
}
```

ループの中の処理を`getFizzBuzz`という関数として抜き出しました。

このように大きな処理の一部を関数に抜き出すことは、以下の２つ理由からおすすめします。

**①処理に名前がつく**

関数として抜き出すことにより、ループの中の処理に`getFizzBuzz`という名前がつきました。
関数や変数の名前には、中身を説明するという役割があります。
ですので、この関数は`num`という数字を受け取って`FizzBuzz`の結果を`get`する関数なんだなあと、中身を詳しく読まなくても把握できます。

**②型情報が付けられる**

関数として抜き出すことにより、ループの中の処理に型情報をつけることができました。
このように関数の引数・返り値の方を明記することで、プログラムの安全性が向上するのはもちろんのこと、関数の処理の流れ・データの流れがわかりやすくなります。

## パターン３　：　文字列として連結させる

```jsx
const getFizzBuzz = (num: number): string => {
    let result = '';
    if (num % 3 === 0) result += 'Fizz';
    if (num % 5 === 0) result += 'Buzz';
    return result || num.toString();
};

for (let num = 1; num <= 15; num++) {
    console.log(getFizzBuzz(num));
};
```

空の文字列を作り、そこに条件分岐で当てはまる場合にFizzやBuzzを連結させていくという書き方です。

この書き方の良いところは、以下の２つです。

**①条件分岐が１つ減る**

パターン２までは必要だった`if(num % 3 === 0 && num % 5 === 0) { return 'FizzBuzz'; }`の部分が省略できるところです。
最初は`result=''`で、３の倍数だったら`result='Fizz'`になり、さらに５の倍数でもあれば`result='FizzBuzz'`になるというように、どんどん連結されていくのですね。

**②メンテナンス性に優れている**

`if(num % 3 === 0 && num % 5 === 0) { return 'FizzBuzz'; }`という文がなくなったことにより、問題（要件）が変わった場合に修正の必要な箇所が格段に減りました。
（例えば、`３`と`５`という数字が`６`と`８`に変わり、`Fizz`と`Buzz`が`Jizz`と`Duzz`に変わった場合を考えてみてください。）

また、`return result || num.toString();`という部分で、**短絡演算**を使用しているところもポイントです。
この部分を短絡演算を使用せずに書くと次のように冗長になります。

```jsx
const getFizzBuzz = (num: number): string => {
    let result = '';
    if (num % 3 === 0) result += 'Fizz';
    if (num % 5 === 0) result += 'Buzz';

    if (result) {
        return result;
    } else {
        return num.toString();
    };
};

for (let num = 1; num <= 15; num++) {
    console.log(getFizzBuzz(num));
};
```

## パターン４　：　テンプレートリテラルと三項演算子を使用する

```jsx
const getFizzBuzz = (num: number): string => {
    return `${num % 3 === 0 ? 'Fizz' : ''}${num % 5 === 0 ? 'Buzz' : ''}` || num.toString();
};

for (let num = 1; num <= 15; num++) {
    console.log(getFizzBuzz(num));
};
```

この書き方の良いところは、関数内の処理がたったの1行で書けているところです。

また、テンプレートリテラルの中で`三項演算子`を用いているところがポイントです。
三項演算子については、[こちらの記事](https://zenn.dev/ys37799665/articles/js-conditional-operator){:target="_blank"}をご覧ください。

## まとめ

他にも、以下のように高階関数を使って書くこともできますが、コードが難しくなるだけで他の書き方と比べて特段のメリットがあるわけでもないのでオススメしません。

```jsx
const fizzBuzz = (num: number, rules: Array<(n: number) => string>): string => {
    for (const rule of rules) {
        const result = rule(num);
        if (result) return result;
    }
    return num.toString();
};

const rules = [
    (n: number) => (n % 15 === 0 ? 'FizzBuzz' : ''),
    (n: number) => (n % 3 === 0 ? 'Fizz' : ''),
    (n: number) => (n % 5 === 0 ? 'Buzz' : '')
];

for (let num = 1; num <= 15; num++) {
    console.log(fizzBuzz(num, rules));
};
```

実用性があるかどうかはさておき、TypeScriptの勉強になるのでいろいろな書き方を試してみると面白いかもしれません。

他にもっといい書き方があるよという方はぜひ教えてください！