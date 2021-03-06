# ビジネスロジック、問題領域

ビジネスロジックという言葉の意味は、特別、難しいものではありません。でも、せっかくなので、言葉の背景も含めて理解しておいた方が良いと思います。

## 問題領域 vs 解決領域

例えば、ある企業では、タイムカードで社員の勤務時間を管理しているとします。毎月、総務部では、社員から提出されたタイムカードを(なんと)手入力でExcelにまとめ、残業手当などを計算しています。時間もかかるし間違いも生じやすいですね。これが「問題(Problem)」です。

そこで、あなたの会社(Webシステム開発ベンダー)が、勤務管理用のWebアプリ開発を請け負うことになりました。あなたの会社が問題の「解決策(Solution)」を提供するわけです。

ここには2つの領域(あるいは立場)が存在します。

- 問題が発生している領域(問題領域、問題ドメイン、Problem domain)
- それを解決する領域(解決領域、解決ドメイン、Solution domain)

問題領域と解決領域では、使う言葉がまるっきり違うことが多いです。特に、あなたがお客様と話すときは、解決領域の用語を使ってはなりません。「このクラスはシングルトンです」とか「テーブル分けた方がいいですか?」と言われても、お客様には通じませんからね。

一方、あなたは、問題領域の(お客様の)用語には不慣れかもしれませんが、頑張って理解する必要があります。また、あなたが知ってるつもりの単語でも、お客様は違う意味で使っている可能性があるので注意しましょう。

## ビジネス、ドメイン

当然ながら、より重要なのは問題領域の方です。「ビジネスドメイン」と呼ぶこともあります。ビジネスという単語には、関心事とか本題、重要事項といったニュアンスが含まれます。会議が脇道に逸れたあとでback to businessと言ったり、「あなたに関係ない」という意味でnone of your businessと言ったりします。

我々にとって解決ドメインはホームグラウンドです。会話の中で、わざわざ「解決ドメインの話しなんだけど…」などと断る必要はありません。一方、ビジネスドメインの話を始める場合は、気持ちをアウェイに切り替える必要があります。そんなわけで、我々が単に「ドメイン」と言った場合、それはデフォルトでビジネスドメインのことを指すと考えて良いです。

## ロジック(Logic)、論理、ルール

お客様には、お客様なりのロジックがあります。残業時間は時給が1.25倍になるとか、18時から30分間は強制的な休憩時間なので勤務時間に入れないといった感じです。ビジネスドメイン独特のロジック、つまりビジネスロジックですね。ビジネスルールと言う場合もあります。

ビジネスロジックは、あなたが開発するWebシステムの肝です。これをシステムに反映しないと意味がありません。システムに反映するべきビジネスロジックが何かを、お客様から漏れなく聞き出すことが、機能設計(あるいは要件定義)フェーズのゴールの一つです。

## 爆弾

ビジネスロジックは開発中に変化する場合があります。来年度から残業手当は1.2倍になるとか、休憩時間は18時からの15分間の間違いだったとか……。また、開発が進むにつれて、開発者のビジネスドメインへの理解も深まり、矛盾に気付いたりすることも多いです。そこで、よくよくお客様に確認してみると、新たなビジネスロジックが発覚したりします。そして、これに「対応できません」と回答する選択肢は我々には無い、というのが常です。

そんなわけで、ビジネスロジックを実装するコードは一箇所にまとめておくのが得策でしょう。プログラムのあちこちに、1.25倍するコードがあったりすると最悪です。最低でも1.25は定数として定義すべきでしょう。さらに残業手当を計算するコードを関数化しておけば、もし「原則1.25倍だけど、30時間を超えた分は1.2倍」みたいな仕様変更があっても、対応しやすいですね。

