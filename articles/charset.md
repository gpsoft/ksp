# 文字コードの話し(1) 〜 文字コードとは?

「文字コード」という言葉は、文脈に応じて3通りの意味で使われます。

- 文字セット(charset)
- ある文字セットにおいて、特定の1文字に割り当てられた数値
- 符号化方式(encoding)

## 文字セット(charset)

いろんな文字を集め、各文字に一意な数値を対応付けた体系のことを文字コードと呼ぶことがあります。本来なら「文字セット(charset)」と呼ぶべきでしょう。ASCIIコード、Unicode、シフトJIS、EUC-JPなどが文字セットの例です。文字セットが違えば、サポートする文字のバリエーションや使われる数値の範囲も異なります。例えばASCIIコードは7bit(0〜127)ですが、Unicodeは21bit分の数値を使います。

## ある文字セットにおいて、特定の1文字に割り当てられた数値

例えばシフトJISでは、「文」という文字に0x95B6(10進数なら38326)という数値を割り当てています。この数値のことを文字コードと表現することがあります。『シフトJISでは「文」の文字コードは0x95B6です』といった具合です。

ありがたいことにUnicodeでは、この数値のために専用の用語が用意されています。「コードポイント」です。Unicodeでは「文」のコードポイントは0x6587(10進数なら25991)です。

## 符号化方式(encoding)

テキスト(文字や文字列)を符号化する方式のことを文字コードと呼ぶ場合があります。一般に符号化(encode)というのは、ある情報をバイト列(つまり8bitデータの羅列)に変換することです。符号化が必要になるのは、情報をファイルに保存したり、別のコンピュータへ送信するときです。

例えば「こんにちは」をUnicodeで表現すると、コードポイントはそれぞれ、0x3053, 0x3093, 0x306B, 0x3061, 0x306Fです。この文字列をファイルに保存するとき、「どのようにバイト列に変換するか」を決めるのが符号化方式の役割です。

符号化方式は文字セットによって異なります。また文字セットごとに、サポートする符号化方式の数も異なります。例えばシフトJISの符号化方式は1つだけですが、Unicodeは、UTF-8やUTF-16LEなど複数の符号化方式をサポートしています。逆に、符号化方式が決まれば文字セットも1つに決まる、と考えて良さそうです。

## 余談 〜 エンディアンとは?

符号化の話が出たついでに、エンディアンについても触れておきます。既にエンディアンを理解している方は飛ばして構いません。

ちょっと想像してみてください。C言語には`unsigned short`というデータ型があります。これは16bit値です。例えば0x95B6は、`unsigned short`型の変数に格納できます。この変数は、メモリ上のどこかの連続する2バイト分を占めているはずです。メモリは、8bitのデータを格納できる箱がズラッと1列に並んだものと考えてください。箱の位置は「アドレス」で識別します。仮にアドレス100番地と101番地が、この変数用の領域だとしましょう。さて、あなたなら、0x95,0xB6の順に格納しますか? それとも0xB6,0x95の順でしょうか?

どっちかが正解で、どっちかが間違いということはありません。例えば、CPUがIntel系なら、0xB6,0x95の順になります。しかしARM系なら、0x95,0xB6の順になるかもしれません(設定によって決まる)。大事なのは、このメモリの例に限らず、8bitを超える値を、8bit単位で区切ってバイト列として表現するときは、必ずバイトの順序を意識する必要がある、という点です。

このバイト順のことを「エンディアン(endian)」と呼びます。選択肢は2種類しかありません。

- ビッグエンディアン(big endian)は、上位のバイトから順に並べる
- リトルエンディアン(little endian)は、下位のバイトから順に並べる

例えば、0xDEADBEAFという32bit値をビッグエンディアンで並べると0xDE,0xAD,0xBE,0xAFとなり、リトルリトルエンディアンで並べると0xAF,0xBE,0xAD,0xDEになります。

このあとで文字列の符号化方式を考えるとき、エンディアン意識する必要が出てくるのでお忘れなく。

# 文字コードの話し(2) 〜 文字セットとしてのUnicode

文字セット(charset)は、いろんな文字を集め、各文字に一意な数値を対応付けた体系のことです。Unicodeは、そんな文字セットの一つです。

## コードポイント

Unicodeがサポートするすべての文字には、「コードポイント」と呼ばれる21bitの一意な数値が割り当てれられています(21bitというのが中途半端ですが気にしないでください)。例えば半角アルファベットの`'a'`という文字のコードポイントは、0x000061です。「漢」なら0x006F22、「字」は0x005B57、「☺」は0x00263A、「叱」は0x0053F1、「𠮟」は0x020B9Fです。「叱」と「𠮟」は似てるけど別々の文字です。

上の例では、0xを前に付けて表記しましたが、コードポイントを表す16進数を表記するときは特別に、U+を前に付けます。またそのとき、始まりの0は省略して、4桁か5桁で表記します。U+0061, U+6F22, U+5B57, U+263A, U+53F1, U+20B9Fといった感じです。

## 面とは?

21bitで表現できる数値の範囲は0x000000～0x1FFFFFですが、このうちUnicodeでコードポイントに使われるのは、U+0000～U+10FFFFのみです。上位の5bitと下位の16bitに分けて考えると、上位の5bitは0x00～0x10(10進数なら0～16)になります。この上位5bitの部分を「面」と呼びます。よってUnicodeの文字は、第0面から第16面の計17面に分けることができます。

第0面はBMP(Basic Multilingual Plane、基本多言語面)と呼ばれ、ほとんどの常用文字がこの面のコードポイントに割り振られています。BMPのコードポイントはU+0000〜U+FFFFなので、(BMPに絞って考えるなら)16bit値の範囲内に収まり、コンピュータで扱いやすいという利点があります。

またBMPのU+0000～U+007FはASCIIコードと互換性があります。例えば、ASCIIコードでは`'a'`の文字コードは0x61ですが、Unicodeでの`'a'`のコードポイントはU+0061です。

日本語であれば、BMP以外にも、第2面のSIP(Supplementary Ideographic Plane、追加漢字面)が使われます。例えば前述の「𠮟(U+20B9F)」はSIPの文字です。

また第15面と第16面はPUA(Private Use Area、私用領域、私用面)と呼ばれ、文字とコードポイントとのマッピングをローカルに決めることができる領域です。私用が可能な領域はBMPの一部(U+E000～U+F8FF)にもあります。私用領域は、携帯電話の絵文字などに使われているようです。

# 文字コードの話し(3) 〜 Unicodeの符号化方式

符号化方式(encoding)とは、テキスト情報(文字や文字列)をバイト列へ変換する方法のことでした。Unicodeの場合は、コードポイント(21bitデータ)の羅列を8bitデータの羅列へ変換することになります。

## UTF-32、BOM

最もシンプルなのは、1文字を4バイトで符号化する方式でしょう。21bitのコードポイントを収納するには3バイト(=24bit)あれば足りるのですが、3バイトの塊というのはコンピュータでは扱いにくいので、4バイトとします(11bit分が無駄になる)。この方式をUTF-32と呼びます。UTFはUnicode Transformation Formatの略です。

たとえば「こんにちは(U+3053, U+3093, U+306B, U+3061, U+306F)」の「こ」をビッグエンディアンで4バイトに変換すると、0x00, 0x00, 0x30, 0x53になります。リトルエンディアンなら、0x53, 0x30, 0x00, 0x00です。他の文字も同様に変換すれば、全体で20バイトのデータになりますね。

エンディアンの選択は自由ですが、書き手と読み手の間で一致させる必要があります。その方法が3つあります。

- 暗黙の了解で、どちらかを採用する
- ビッグならUTF-32BE、リトルならUTF-32LEのように呼び分ける
- BOMを付ける

BOM(Byte Order Mark)は、文字通り、エンディアンを判定するためのマークです。BOM方式の場合、コードポイントU+FEFFを符号化して先頭に付ける、という決まりになっています。よって「こんにちは」をBOM付きUTF-32で符号化すると、全体で24バイトになります。読み手は、先頭の4バイトを見て、0x00, 0x00, 0xFE, 0xFFならビッグエンディアン、0xFF, 0xFE, 0x00, 0x00ならリトルエンディアンと判断できます。

UTF-32で符号化したあとのデータサイズは、(BOMを別にすれば)元の文字列の文字数を4倍すれば得られます。文字と文字の境界を見つけるのも簡単です。この辺がUTF-32のメリットでしょう。逆に言えば、他の符号化方式の場合、文字と文字の境界を見つけにくいってことです(このまま読み進めると、その理由が分かります)。

## UTF-16、符号単位、サロゲートペア

UTF-32はシンプルな反面、無駄も多いです。タダでさえ11bitが無駄になっていました。また、よく使われる文字のコードポイントは、U+0000〜U+FFFFの範囲、つまりBMP(基本多言語面)に割り当てられているので、16bitに収まります。UTF-32では半分が無駄ということになります。前述の「こんにちは」が良い例ですね(やたらと0x00が多い)。

そこで、BMPの文字を重視し、原則として1文字を2バイトで符号化しようと考えたのがUTF-16です。UTF-32と同じように、エンディアンに応じてUTF-16BEやUTF-16LEと呼び分けたり、BOMを付けることも可能です。例えば「こんにちは」をBOM付きのビッグエンディアンで符号化すると、0xFE, 0xFF, 0x30, 0x53, 0x30, 0x93, 0x30, 0x6B, 0x30, 0x61, 0x30, 0x6Fの12バイトになります。

上に「原則として1文字を2バイトで符号化」と書きましたが、この2バイトのことを「符号単位(code unit)」と呼びます。BMPの文字なら1つの符号単位で表現できるのですが、他の面の文字(U+10000〜U+10FFFF)は1つでは足りません。その場合は、符号単位を2つ使って符号化します。詳しいことは後で説明しますが、符号単位を2つ使うので、「サロゲートペア(surrogate pair、代用対)」と呼びます。また1つ目を「上位サロゲート」、2つ目を「下位サロゲート」と呼びます。上位サロゲートと下位サロゲートの順序は、(エンディアンに関係なく)一定です(上位が先)。

UTF-16で符号化されたバイト列を解読するとき、符号単位ごとに見ていくわけですが、その符号単位がBMPの文字なのか、サロゲートペアの一部なのか、どうやって見分ければ良いでしょうか?

実はBMPのU+0000〜U+FFFFの中には、コードポイントに使われてない数値があります。0xD800～0xDBFFの1024個と、0xDC00～0xDFFFの1024個です。サロゲートペアはこれらの値を使うので、BMPの文字と混同する心配がありません。これらの値を2つ使えば、1048576(=1024×1024)通りの組み合わせを作ることができます。そして都合の良いことに、この数字は、BMP以外の面の全文字数(=16面×256×256)と一致します。

そこで、0xD800～0xDBFFの中から上位サロゲート値を取り、0xDC00～0xDFFFから下位サロゲート値を取って、その組み合わせをBMP以外のコードポイントと対応付けます。例えば、

- 0xD800と0xDC00を、U+10000へ
- 0xD800と0xDC01を、U+10001へ
- 0xD800と0xDC02を、U+10002へ

といった具合です。これがサロゲートペアのカラクリです。

UTF-16で符号化したあとのデータサイズは、(特殊なテキストでない限り)おおむねUTF-32の半分になります。一方で、文字によって符号化後のデータサイズが違う、という複雑さが生じてしまうのが難点と言えるでしょう。

## UTF-8

UTF-16は、UTF-32と比べれば無駄が減りましたが、アルファベット文化の人にとって見れば、まだまだ無駄があります。例えば英文であれば、ASCIIコードの7bitでほとんどのテキストが符号化できてしまうのですから。

そこで、ASCIIコードとの互換性を重視して生まれたのがUTF-8です。

UTF-8では、コードポイントを1〜4バイトに符号化します。ルールは以下の通りです。

- U+0000～U+007Fは、有効bit数が7なので、それをそのまま使って1バイトとする
- U+0080～U+07FFは、有効bit数の11を上位5bitと下位6bitに分け、それぞれに0xC0と0x80を足して2バイトとする
- U+0800～U+FFFFは、有効bit数16を上位から4bit、6bit、6bitに分け、それぞれに0xE0、0x80、0x80を足して3バイトとする
- U+10000～U+10FFFFは、有効bit数21を上位から3bit、6bit、6bit、6bitに分け、それぞれに0xF0、0x80、0x80、0x80を足して4バイトとする

少々複雑ですが、重要なのは、BMPのU+0000～U+007Fについては、下位7bitをそのまま使って1バイトに符号化するので、ASCIIコードの符号化と互換性があるという点です。また、日本語の文字の多くは3バイトになり、BMP以外の面の文字は4バイトになります。

いくつか例を挙げましょう。「叱(=U+53F1)」は有効bit数16なので、4bit、6bit、6bitに分けて0x05, 0x0F, 0x31、これに0xE0, 0x80, 0x80を足して0xE5, 0x8F, 0xB1となります(3バイト)。

似たような字の「𠮟(=U+20B9F)」は有効bit数21で、3bit、6bit、6bit、6bitに分けて0xF0, 0x20, 0x2E, 0x1F、それぞれに0xF0, 0x80, 0x80, 0x80を足して0xF0, 0xA0, 0xAE, 0x9Fです(4バイト)。

ちなみに、「叱」は「口」にカタカナの「ヒ」と書きますが、「𠮟」は口に漢数字の「七」です。後者は、BMPから漏れた漢字の例として良く挙がります。同様に「齟齬」はBMPですが、「𪗱𪘚」はBMPから漏れています(U+2A5F1とU+2A61A)。文字化けしてますか? だとすると、あなたのコンピュータ上でこの記事を表示しているフォントには、これらの文字のグリフが含まれてないってことです。

UTF-8に関しては、エンディアンの考慮は不要です(ルールから自ずとバイトオーダーが決まるので)。よってBOMを付けることもありません。しかし、Windowsの「メモ帳」のように、UTF-8のファイルにBOMを付けてしまうエディタも存在するので注意しましょう(コードポイントU+FEFFをUTF-8で符号化した、0xEF, 0xBB, 0xBFをファイルの先頭に付けてしまう)。

# 文字コードの話し(4) 〜 文字列の符号化方式とプログラミング

プログラムを書くとき、いろんな場面で文字列の符号化方式(encoding)を意識する必要が生じます。例えば:

- プログラムのソースファイルを保存するとき
- そのプログラムを処理系に通すとき
- プログラムの中でテキストファイルを読み書きするとき
- ネットーワーク経由でテキスト情報を通信するとき

などです。

大事なのは、書き手と読み手の間で符号化方式を一致させることです。

## ソースファイルの符号化方式

プログラムのソースファイルを保存するときは、エディタの設定を通して符号化方式を指定するはずです。符号化方式の選択肢は、ファイルの読み手である、言語の処理系(コンパイラやインタープリタ)に依存します。

処理系が符号化方式を決める方法は:

- 設定ファイルで指定する
- コンパイルオプションで指定する
- 自動判定する
- ソースファイルの特殊なコメント文により指定する

など、様々です。

例えばJavaなら、システムプロパティ`file.encoding`値で決まります。phpなら`php.ini`の`default_charset`値、rubyはソースファイル先頭のマジックコメントといった具合です。

「デフォルトはUTF-8」という方針を採用している処理系が多いので、あまり意識せずにUTF-8でソースファイルを保存すれば問題無いかもしれません。ただしWindowsの.NET系は、マイクロソフトのシフトJIS(cp932, ms932, Windows-31J)を使うのが無難です。

## ファイルIO、通信

プログラム内部に持っている文字列を外部へ出力(ファイルに書き出したり、別のコンピュータへ送信したり、stdoutへ表示したり)するときや、逆に外部から受け取る(ファイルを読んだり、データを受信したり、stdinから入力する)ときには、やはり符号化方式を意識する必要があります。

符号化方式の決め方はケースバイケースなので詳細には立ち入れませんが、もしプログラムが文字化けを起こしたら、読み手と書き手の間で符号化方式がズレてないか確認しましょう。

## プログラム内部での文字列の扱い

プログラム内部で文字列を扱うときはどうでしょうか? 符号化方式を意識する必要があるでしょうか? 答えはYESですが、プログラミング言語によって意識の度合いが違います。パターンとしては、大きく3つに分かれると思います。

- A) 文字列型か`String`クラスが標準でサポートされており、符号化方式は1つに固定される
- B) 文字列型か`String`クラスが標準でサポートされており、符号化方式は自由
- C) 標準的な文字列型や`String`クラスをサポートしてない

Aパターンの例は、JavaやJavaScript、C#などです(ちなみに、この3つの言語の場合は、UTF-16を採用しています)。符号化方式を変更する余地が無いので、符号化方式への意識はやや薄いです。たとえば文字列をバイト列へ変換するときは、変換先の符号化方式を明示する必要がありますが、変換元の符号化方式を意識する必要はありません。

```java
// Windows用のシフトJISなバイト配列へ変換。
String hello = "こんにちは";
byte[] ba = hello.getBytes("cp932");
```

一方、Bパターン(rubyやphpなど)の言語では、文字列オブジェクトごとに符号化方式を変えることができます。例えばphpの場合、`php.ini`の`mbstring.internal_encoding`値、あるいは関数`mb_internal_encoding()`により、デフォルトの符号化方式が決まります。また、関数`mb_convert_encoding()`を使えば符号化方式を変更できますが、変換元と変換先の符号化方式を正しく指定するのはプログラマの責任です。

```php
hello_utf8 = 'こんにちは';
hello_cp932 = mb_convert_encoding(hello_utf8, 'cp932');
hello_utf8_again = mb_convert_encoding(hello_cp932, 'utf-8', 'cp932');
```

最後のCパターンの例は、C言語などです。「文字列」を抽象化したデータ型が(標準的には)無いので、例えばC言語では、`char`値(8bit値)の配列として文字列を管理します。なので、徹頭徹尾、符号化方式を意識しておく必要があります。

3つのパターンに共通して言えるのは、文字列を文字単位に分解するときは、かならず符号化方式を意識する必要がある、ということです。例えば文字列の文字数をカウントしたり、先頭から3文字目までを切り取りたい、といったケースです。言語の標準関数が、符号化方式を踏まえたうえで計算してくれるなら、それを使えば良いでしょう。そんな関数が提供されてない場合は、プログラマが意識して計算する必要があります。

## Javaの文字列

前述の通り、Java内部での文字情報の符号化方式はUTF-16です。つまり`Character`オブジェクトや`String`オブジェクトの内部では、文字データがUTF-16で保持されています。プリミティブ型の`char`値(16bit)がUTF-16の符号単位に相当し、サロゲートペアも適用されます。コードポイントの表現にはプリミティブ型の`int`値(32bit)が使われます。`Character`クラスや`String`クラスには、Unicode文字の属性を調べるためのメソッドが多数用意されています。以下に、例を挙げます。

- `Character#charCount()`
任意のコードポイントが何個の符号単位になるか(1か2か)を返す。

- `Character#isHighSurrogate()`, `Character#isLowSurrogate()`
   任意の`char`値が上位サロゲート、あるいは下位サロゲートかどうかを返す。

- `String#codePointCount()`
   文字列長を、`char`数ではなくコードポイント数で返す。

- `String#offsetByCodePoint()'
   文字列のインデックスを、`char`数ではなくコードポイント数で変化させる。

- `String#getBytes()`
   文字列を任意の符号化方式(Shift_JISとかUTF-8とか)でエンコードして`byte[]`に変換する。

- `String#String()`
   任意の方式でエンコードされた`byte[]`から`String`オブジェクトを生成する。
