# バイナリデータ

コンピュータが扱う情報は、すべて「バイナリデータ」です。

## デジタルの世界

アナログとデジタル。プログラミングはデジタルの世界です。デジタルには、0と1しかありません。回路がつながってるか、切れてるか。電気が点くか、点かないか、の二択です。「切れかかっている」とか、「ちょっと点く」みたいな中途半端な選択肢はありません。

0と1だけ。しかし心配いりません。我々が日常生活の中で使う数字は0から9までの10個だけですが、別に困ってないですよね。それと同じ作戦を使えば、0と1だけでも困らないのです。その「作戦」とは、「桁を増やすこと」です。

## 十進数

日常生活では、`0`から`9`までの10個の数字を使って、あらゆる数値を表記します。説明を簡潔にするために、自然数に絞って考えましょう。つまり、「`0`から`9`までの10個の数字を使って、あらゆる自然数を表記する」です。

数字と数値(自然数)を区別したいので、表記方法を分けましょう。数字は`0`や`8`と表記し、数値は0や8と書くことにします。

0から9までの自然数を表記するには、そのまま数字を対応させて、`0`とか`3`とか`8`とか`9`、と書けば良いですね。では、「9の次の自然数」を表記するなら、どうでしょう。もう使える数字は余っていません。そこで、桁を増やします。数字を2つ使うわけですね。「9の次の自然数」は`10`と表記します。

`00`じゃないので注意して下さい。桁を増やすときは`1`から始める、という決まりです。当たり前に見えると思いますが、あとで、この作戦をデジタル(`0`と`1`だけ)の世界にも応用するので、お忘れなく。

さらに続けると、 `11`, `12`, ..., `19`と進みます。`9`まで来たので、次は、上の桁を1つ上げて、`20`です。`21`, `22`, ...と進んで、`99`まで来たら、次はまた桁を増やして`100`と続きます。

このように、桁を増やすことにより、10種類の数字だけを使ってあらゆる自然数を表記するのが「十進数」です(英語で言えばdecimal、デシマル)。

## 二進数、ビット、バイト

この作戦をデジタルの世界に適用したのが「二進数」です(英語で言えばbinary、バイナリ)。`0`と`1`、2種類の数字を使って数値を表記します。0と1なら、そのまま、`0`と`1`。1の次の数値(つまり2)は、桁を増やして`10`となります(`00`じゃないですよ)。で、次(つまり3)が`11`。桁を増やして`100`, `101`, `110`, `111`(数値の4,5,6,7)と進み、また桁を増やして`1000`と続きます。4桁の二進数の最大は`1111`で、これは数値の15です。

規則性を感じるため、縦に並べてみましょう。

```
   0
   1
  10
  11
 100
 101
 110
 111
1000
1001
1010
1011
1100
1101
1110
1111
```

桁を増やしていけば、`0`と`1`だけを使って、無限に大きな数値を表記できる、ということが分かりました。「桁」を英語に訳すとdigitですが、二進数の場合は特別な呼び名があります。「ビット(bit)」です。例えば、`10110101001010100111`は20ビットの二進数です。

また、コンピュータの世界では、8ビットをひと塊として扱うのが一般的です。このひと塊を「バイト」と呼びます。1バイトで表現できる自然数は0から255です。256個の自然数。256は「にごろ」と読んだりします。

## 十六進数

2個の数字を使うのが二進数、10個の数字を使うのが十進数、と来れば、十六進数(hexadecimal、ヘキサデシマル)は16個の数字を使う、と予想できますね。でも数字は10個しかないので、足りない6個にはアルファベットの`a`から`f`を使います。

ひと桁目は、順に、`0`, `1`, `2`, `3`, ..., `8`, `9`, `a`, `b`, ..., `e`, `f`となり、自然数の15までを表しています。桁を増やして、`10`(自然数の16)。`11`(=17), `12`(=18), ..., `19`(=25)と進みます。さらに続けると、`1a`, `1b`, ..., `1f`, `20`, ..., `9f`, `a0`, ..., `ff`, `100`といった感じです。`100`は自然数の256です。

8ビット(つまり1バイト)の最大値255は、二進数なら`11111111`と8桁になってしまいますが、十六進数なら2桁で`ff`と表記できます。同様に、16ビットの最大値は65535で、二進数なら`1`が16個ですが、十六進数なら4桁で`ffff`と書けます。

このように、十六進数表記は、ちょうど二進数の4分の1の桁数になります。コンパクトに書けるので、プログラミングの中でも良く使われます。

## デジタルとバイナリ

デジタル(digital)は、「桁(digit、ディジット)」に由来する言葉です。ということは、冒頭に書いた「デジタルには、0と1しかありません」は、ちょっと飛躍してますね。

もう少し分解して書くと、以下のような感じになります。

- デジタルの世界では、情報を数値に置き換えて扱う(デジタル => 数値)
- 数値は「桁」の集まり(数値 => 桁)
- 「桁」を表記する最小構成は、二進数(桁 => 二進数)
- 二進数には、`0`と`1`しかない(二進数 => `0`と`1`)
- 全部つなげて途中を省けば、「デジタル => `0`と`1`」

見方を変えれば、いまのコンピュータは、デジタル情報を扱うために、最も原始的で楽な構成である二進数(バイナリ)を採用した、ということです。

こういった背景から、コンピュータが扱う情報全般のことを「バイナリ」とか「バイナリデータ」とか呼んだりすることがあります。
