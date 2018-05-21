---
layout: post
title:  "「Lean Architecture / DCI Evening」参加レポート"
date:   2017-11-10 11:51:00 +0900
categories: DCITokyo
---
2017年10月18日、James Coplienさんとその奥様であるGertrud Bjørnvigさんをお招きして、「[Lean Architecture / DCI Evening](https://mpdosaka.connpass.com/event/69002/) 」というイベントを開催しました。日本ではソフトウェアパターンやアジャイルのリーダーとして知られるJames Coplienさんは、『 [マルチパラダイムデザイン](https://www.amazon.co.jp/%E6%96%B0%E8%A3%85%E7%89%88-%E3%83%9E%E3%83%AB%E3%83%81%E3%83%91%E3%83%A9%E3%83%80%E3%82%A4%E3%83%A0%E3%83%87%E3%82%B6%E3%82%A4%E3%83%B3-%E3%82%B8%E3%82%A7%E3%83%BC%E3%83%A0%E3%82%B9%E3%83%BB-%E3%83%BB%E3%82%B3%E3%83%97%E3%83%AA%E3%83%B3/dp/4894715287) 』（1998年）でドメインとドメイン間の関係を中心に据えた設計パラダイムを提唱していました。Coplienさんは2009年、MVCアーキテクチャの考案者である [Trygve Reenskaug](https://en.wikipedia.org/wiki/Trygve_Reenskaug) さんと共に「DCIアーキテクチャ」を発表しました。2010年、CoplienさんはGertrudさんとともに書籍『 [Lean Architecture](https://www.amazon.co.jp/Lean-Architecture-Agile-Software-Development/dp/0470684208) 』を上梓、トヨタ生産方式をソフトウェアアーキテクチャに適用するリーンアーキテクチャについて、DCIアーキテクチャをそのビルディングブロックと位置づけた具体的な方法を記述しました。今回、マルチパラダイムデザインの読書会を大阪で開催していた縁から、表題のイベントを開催することができました。この記事ではセッションの内容についてトークを再現する形でお伝えしたいと思います。

> **NOTE:** （※）内は筆者によるコメントです。また、基本的に英語から修正せずに翻訳しているので、事実関係に関しては確認がとれていないものもあります。

## CoplienさんによるDCIアーキテクチャの概説

**Coplienさん（以下、C）**: 最初にクイズをします。みんな立って。DCIは、DCIと呼ぶ前はなんと呼んでいたか知っていますか？・・・DCA (Data-Collabolation-Algorithm)(※関連リンクを参照)です。知ってた人はそのまま立って。これを差し上げます。（※参加者の一人がサイン付きのLean Architectureをプレゼントされる）

C: (※DCIで開発するための言語基盤とIDEを提供する)Trygveのプログラムの拡張子は「.k」になっているのはなぜでしょう？これは哲学者のカントからとっている。

C: オブジェクト指向プログラミングができない、モダンな言語はなんでしょう？Rust？Java。OK、それだ。Javaはオブジェクト指向ではなく、クラス指向の言語だ。

C: DCIのWebサイトfulloo.infoにいろんなリソースがあります。サンプルコードとかビデオとかも入っている。

C: 今日はインフォーマルな会です。小さいDCIのチュートリアルをすることもできるし、私が最近どういう研究をやっているかを話すこともできる。DCIが実際にどうやってプログラマーの集中力を高めてエラーを取り除くことができるかということを話してもいいし、Trygve言語の実際の例もお見せすることができる。見た目はJavaみたいだけどね。そこには、コンテキストとロールも登場する。設計の話もしてもいいし、ドメイン分析の話をしてもいい。

参加者A: ドメイン分析とTrygve IDEも見たい。私たちは大阪で「マルチパラダイムデザイン」の読書会をしていました。MPDとDCIをどのように繋ぐかの話も聞きたい。

C: 今日は何人、大阪から来たの？Oh! DCIを殆ど知らないという人は？最初に小さいインタラクションをしましょうか。15分くらい。

C: マルチパラダイムデザインは、クラスベースの言語に基礎を置いて議論を展開している。DCIはリアルなビルディングブロック(構成要素)とオブジェクト群のネットワークです。そこが大きな違いです。ドメイン分析とかユースケース、ロールのアクターに関しては後ほど妻が説明してくれる。

C: まずはDCIの触り。簡単なデモを。銀行口座間の送金のケースを考えてみる。ソフトウェア上ではそれはどういう状態でしょうか。クラスを書いて、オブジェクト同士をインタラクションさせますね。クラスを書くときというのは、クラスの全てのメソッドを考慮して書く必要がある。

C: ATMの例を考えてみましょう。ATMが稼働していて、それを確認するために、コンソールにオブジェクトIDをプリントするプログラムを考えてみましょう。

（※メソッドが呼ばれるたびにオブジェクトIDがプリントされていくデモ）

C: これを見て何かパターンは見つかるでしょうか？ 単に同じオブジェクトが延々と呼ばれていることがわかるだけですね！（※会場笑）呼ばれるオブジェクトの順番とかは全然重要ではなかったわけです。

C: 次にオブジェクトIDではなく、クラス名が表示されるようにしてみましょう。クラス名は重要ですよね。コンソールを見てパターンは見つかりますか？ないですね。これもまだ完璧ではないようです。クラスのうち、2つは口座に関するもの、最後の3つ目は通貨に関するものだということはわかります。しかし、アーキテクチャの振る舞いはクラスだけでは表現されていないようです。

C: さて、今度はメソッドが呼ばれる毎に、役割（※ロール）がプリントされるようにしてみますよ。・・・Ah ha!(※会場笑) 分かりましたね。お金を送金するときの構造はこういうふうになりますよね。構造は役割（※ロール）の中に入っています。

C: 1つの銀行に2つ以上の口座を持っている人はいますか？（※会場から手が）それでは、私に1つの自分の口座から他の口座に送金するユーザーストーリーについて説明して下さい。

参加者B: ある口座からある口座に送金します。妻にダイアモンドを買うために。貯蓄口座から普通口座へ。

C: もっと一般的に言うと？

参加者B: 振込元から振込先へ送金する。

C: 振込元口座を持っている人はいますか？それは、オブジェクトでしょうか？クラスでしょうか？なんでしょうかね、振込元口座みたいな役割とは？・・・人はこれを役割（ロール）として考えているわけですよね。クラスやオブジェクトとして役割（ロール）を考えてはいないですよね。

C: アラン・ケイの最初に考えていたオブジェクト指向というのはヒューマンメンタルモデルをコンピュータに適用させたものでした。そのメンタルモデルを今発見したのです！

参加者B: どうやって、口座の振込元とか振込先とかを判別するんでしょうか？

C: ピアジェという発達心理学者がいます。ピアジェに「オペレーショナルモデル理論」というのがあります。ビルディングブロック（※構成要素）を使って論理的に考える必要があります。オブジェクトやクラスではないものがあるのです。おそらく私たちは最初にオブジェクトを学び、その後にクラス・クラス化を学ぶようです。子どもが一番最初にオブジェクトを学ぶ時は、全てを自分自身として認識するようです、母親さえも自分だと思う。4-7ヶ月の間に区別をつけることができるようになっていく。小さな子どもは馬を見た時に、馬を指差して「犬！」と叫ぶ。（※あ〜！と会場共感の声）こういった認識は物事に共通性を見出していることによって成り立っています。私たちはプログラムを問題解決のために書いています。子どもは8ヶ月くらいの時に原因と結果という因果関係について学ぶ。その頃から机を押したり、上にあるものを叩いたりして何かが落ちたりするのを楽しむ。そうして最終的には、問題解決する能力を学習していく。それがオペレーショナルモデルと呼ばれるものです。

C: プログラムを通して問題解決するためにこの考えが必要ですね。プログラムを書くときにはどうやっているのでしょう？ここにこのようにオブジェクトが散らばっている。そしてオブジェクトをつなげるユースケースがある。これはまた別のユースケースですね。これはまた別のユースケース、また別のユースケース・・・（※会場笑）ユースケースはオブジェクトのシーケンスではないんですよね、こう見ると。順番があるわけではない。

C: では、ユースケースのコードをオブジェクトよりも上のレイヤーに取り出す、ということをやってみましょう。今、ユースケースのコードは、オブジェクトのクラスの中にあります。このユースケースの部分を、クラスの外側に出すのです。そうすると、オブジェクトの方は基本的なクラスのインスタンスのままなので、とても単純です。しかし、これらは実際のところユースケースの一部にはなりません。では、ユースケースの部品は何でしょうか？ ロールですね。ユースケースの部品は、ロールの中にあります。開発者は、このようなロールの中からいくつかを選んでまとめます。このまとまりを、コンテキストと呼びます。
つまり、コンテキストがユースケースに相当します。そして、ユースケースを実行するには、サブジェクト（※ユースケースの入り口となるエンティティ、後述のサンプルコードの`TransferMoneyContext.SOURCE_ACCOUNT`に代入されるオブジェクトにあたる）にこれらのメソッド（※ロールのメソッド）をインジェクトします。このオブジェクト（※他のエンティティ、後述のサンプルコードの`TransferMoneyContext.DESTINATION_ACCOUNT`に代入されるオブジェクトにあたる）にもこれらのメソッドをインジェクトします。

C: オブジェクトは既にあるからユースケースを書きたい。例えばヘリコプターを飛ばすことについて考えてみましょう。オペレーショナルモデルで思考して、ロールを考えます。映画の「マトリックス」を見たことがあるますか？トリニティはヘリコプターを飛ばさなければいけなかったですよね。ヘリコプターを操縦するプログラムをダウンロードしていました。ロールのプログラムをダウンロードすることによってヘリコプターを飛ばすことができたわけです。ランタイムでオブジェクトに対してどのようにユースケース上で振る舞うのかを教えてあげる。ユースケースが終わると、オブジェクトは何をやるか、何をやったかを（※どのように振る舞うかを）忘れてしまうわけです。

参加者B: トリニティはオブジェクト（※エンティティ）、パイロットはロールということですね。

C: トリニティは、ヘリコプターを操縦するためのメソッドを持っている。手を動かしたり、足を動かしたりといった基本的なトリニティが持っているメソッドを使うことができる。コンテキストがヘリコプターを運転させる。トリニティは基本的な動作はできる。コンテキストとトリニティの間のメソッドの契約というものが必要で、トリニティは人なので人が持っているメソッドだけが使えるという契約がある。手を動かすとか。ヘリコプターを操縦するというのははコンテキストです。ロールはデータを持たない。純粋にメソッドを持っている。以上がDCIの基本的な話でした。

参加者C: ロールとクラスの違いがまだわからない。

C: クラスはデータを持ちます。あなた達はいわば人間クラスのインスタンスですよね？そこから初期化されている。（※会場笑）ヘリコプターパイロットはロールなので、ヘリコプターパイロットのインスタンスを作ることはできない。人間が必要なんです。あるいはロボット？もしくはゴリラ。

参加者C: Javaのインタフェースはロールに近い？

C: 似てるとは言えるけど、実装がないからロールを完全に表すかというと違うと思います。（※Java 8以降はインタフェースは実装を持てるようになりましたが）あと、Javaはインタフェースは静的に決まっていて、動的に決めることができないというところが異なります。クラスがインタフェースをextendするというのを全てコンパイル時に決めなければなりません。

参加者D: Swiftのプロトコルエクステンションを使うと近いことができますか？

C: Objective-Cでお腹いっぱいなので、Swiftの学習は避けているものでね（※会場笑）プロトコルエクステンションを使うと近いことができるようです。

参加者C: クラスとロールの違いについて。Java クラスとインタフェース、Scala クラスとトレイト、C++ 動的クラス、Virtualクラス これらはDCIの実装として使うことができるか。

C: Scalaのトレイトはかなり近いです。Scalaではトレイトのミックスインを使う。トレイトを使ってクラスに合成できる。その結果クラスは両方の機能のメソッドを持つことができる。C++ではどうするか？C++の場合はクラスの合成に継承を使ってしまうので難しい。どうするか？ロールをテンプレートを使って書くことができる。テンプレートを使えばDCIを実現することは可能です。ただそれはコンパイルのタイミングになってしまう。ダイナミックではない。fulloo.infoも見てくださいね。なんとなく、DCIのイメージが掴めましたか？

C: DCIのための言語として気に入ってるものの一つはRubyです。オブジェクトの拡張が簡単にできる。モジュールを使ってミックスインすることもできます。ただ、メソッドは自由に追加することはできるが、それを取り外すのが難しい。名前の衝突の問題もある。Matzはこの問題について理解してくれたが、一度インジェクトしたものを引き剥がす機能を入れることまでは、説得するには至らなかった。Rubyの仮想マシンを変更すればできるのですがね。

（※実は筆者はRubyistなのですが、refinementを使うとある程度は目的に適うのではないのかなーと思ってしまいました）

C: Trygveを使ってる人。（※一人だけ手が挙がる）オープンソースですよ！（※会場笑） お金の送金のサンプルを見せていきます。ここにクラスがあります。これはドメイン分析から抽出されたものです。 `amount()`(金額) などのメソッドを持っている。クラス指向のプログラミングならこんなものでしょうか？Trygveの紹介をしていきますね。

（※以下のコードはTrygve で書かれた送金のサンプル https://github.com/jcoplien/trygve/blob/master/examples/july_money_transfer.k より引用）

<pre class="prettyprint lang-java">/*
* july_money_transfer.k
*/

class Currency
{
    public Currency(double amount) {
        amount_ = amount.clone
    }
    public Currency +(Currency amount) {
        assert(false)
        return this;
    }
    public Currency -(Currency amount) {
        assert(false)
        return this;
    }
    public Currency() {
    }
    public String name() const
    {
        assert(false);
        return ""
    }
    public String sign() const
    {
        assert(false);
        return ""
    }
    public double amountInEuro() const {
        assert(false);
        return 0.0
    }
    public double amount() const {
        return amount_
    }
    public String toString() const {
       return amountInEuro().toString()
    }
    public int compareTo(Currency other) {
       if (amount() > other.amount()) return 1
       else if (amount() < other.amount()) return -1;
       return 0
    }

    private double amount_
}

class Euro extends Currency {
    public Euro(double amount) {
        Currency(amount)
    }
    public Euro -(Currency amount) {
        return new Euro(amount() - amount.amountInEuro())
    }
    public Euro +(Currency amount) {
        return new Euro(amount() + amount.amountInEuro())
    }
    public String name() const
    {
        return "Euro";
    }
    public String sign() const
    {
        return "€";
    }
    public double amountInEuro() const
    {
        return amount()
    }
    public String toString() const {
        return amount().toString()
    }
}

class Account
{
    public Account(int acctno) {
       acct_ = acctno
    }
    public String accountID() const {
       return acct_.toString()
    }
    public Currency availableBalance() const { assert(false); return null }
    public void increaseBalance(Currency amount) { assert(false) }
    public void decreaseBalance(Currency amount) { assert(false) }
    public void updateLog(String message, Date dt, Currency amount) {
        assert(false)
    }

    private int acct_
}

class CheckingAccount extends Account {
    public CheckingAccount() {
			Account(1234);
			availableBalance_ = new Euro(100.00)
    }
    public Currency availableBalance() const
    {
        return availableBalance_
    }
    public void decreaseBalance(Currency c) {
        availableBalance_ = availableBalance_ -  c
    }
    public void updateLog(String message, Date t, Currency c) const {
        System.out.print("account: ").print(accountID())
                  .print(" CheckingAccount::updateLog(\"").print(message)
                  .print("\", ").print(t.toString()).print(", ")
                  .print(c.toString()).print(")")
                  .println()
    }
    public void increaseBalance(Currency c) {
        availableBalance_ = availableBalance_ + c
    }

    private Currency availableBalance_
}

class SavingsAccount extends Account {
    public SavingsAccount() {
			Account(1234);
			availableBalance_ = new Euro(0.00)
    }
    public Currency availableBalance() const {
        return availableBalance_
    }
    public void decreaseBalance(Currency c) {
        assert(c > availableBalance_);
        availableBalance_ = availableBalance_ - c
    }

    public void updateLog(String logMessage, Date timeOfTransaction,
                               Currency amountForTransaction) const {
        assert(logMessage.length() > 0);
        assert(logMessage.length() < MAX_BUFFER_SIZE);
        // assert(new Date() < timeOfTransaction);
        System.out.print("account: ").print(accountID())
                  .print(" SavingsAccount::updateLog(\"").print(logMessage)
                  .print("\", ").print(timeOfTransaction.toString())
                  .print(", ").print(amountForTransaction.toString())
                  .print(")").println()
    }
    public void increaseBalance(Currency c) {
        availableBalance_ = availableBalance_ + c
    }

    private Currency availableBalance_;
    private int  MAX_BUFFER_SIZE = 256
}

class InvestmentAccount extends Account
{
    public InvestmentAccount() {
			Account(1234);
			availableBalance_ = new Euro(0.00)
    }
    public Currency availableBalance() const {
        return availableBalance_
    }
    public void increaseBalance(Currency c) {
        availableBalance_ =  availableBalance_ + c
    }
    public void decreaseBalance(Currency c) {
        availableBalance_ = availableBalance_ - c;
    }
    public void updateLog(String s, Date t, Currency c) const {
       System.out.print("account: ").print(accountID())
                 .print(" InvestmentAccount::updateLog(\"")
                 .print(s).print("\", ").print(t.toString())
                 .print(", ").print(c.toString()).print(")")
                 .println()
    }

    private Currency availableBalance_;
}

class Creditor
{
    public Creditor(Account account) {
			account_ = account
    }
    public Account account() { return account_ }
    public Currency amountOwed() const { return new Currency(0.0) }

    private Account account_
}

class ElectricCompany extends Creditor
{
    public ElectricCompany() {
        Creditor(new CheckingAccount())
    }
    public Currency amountOwed() const {
        return new Euro(15.0)
    }
}

class GasCompany extends Creditor
{
    public GasCompany() {
        Creditor( new SavingsAccount());
        account().increaseBalance(new Euro(500.00))    // start off with a balance of 500
    }
    public Currency amountOwed() const {
        return new Euro(18.76)
    }
}

context TransferMoneyContext
{
    // Roles

    role AMOUNT {
        public Currency(double amount);
        public Currency +(Currency amount);
        public Currency -(Currency amount);
        public String name() const;
        public String sign() const;
        public double amountInEuro() const;
        public double amount() const;
        public String toString() const;
        public int compareTo(Currency other)
    } requires {
        Currency(double amount);
        Currency +(Currency amount);
        Currency -(Currency amount);
        String name() const;
        String sign() const;
        double amountInEuro() const;
        double amount() const;
        String toString() const;
        int compareTo(Currency other)
    }

    role GUI
    {
        public void displayScreen(int displayCode)
    } requires {
        void displayScreen(int displayCode)
    }

    role SOURCE_ACCOUNT {
        public void transferTo() {
            // This code is reviewable and meaningfully testable with stubs!
            int SUCCESS_DEPOSIT_SCREEN = 10;

            // beginTransaction();

            if (this.availableBalance() < AMOUNT) {
                // endTransaction();
                assert(false, "Unavailable balance")
            } else {
                this.decreaseBalance(AMOUNT);
                DESTINATION_ACCOUNT.increaseBalance(AMOUNT);
                this.updateLog("Transfer Out", new Date(), AMOUNT);
                DESTINATION_ACCOUNT.updateLog("Transfer In", new Date(), AMOUNT);
            }
            // GUI.displayScreen(SUCCESS_DEPOSIT_SCREEN);
            // endTransaction()
        }
    } requires {
        void decreaseBalance(Currency amount);
        Currency availableBalance() const;
        void updateLog(String msg, Date time, Currency amount)
    }

    role DESTINATION_ACCOUNT {
        public void transferFrom() {
            this.increaseBalance(AMOUNT);
            this.updateLog("Transfer in", new Date(), AMOUNT);
        }
        public void increaseBalance(Currency amount);
        public void updateLog(String msg, Date time, Currency amount)
    } requires {
        void increaseBalance(Currency amount);
        void updateLog(String msg, Date time, Currency amount)
    }

    public TransferMoneyContext(Currency amount, Account source, Account destination)
    {
        SOURCE_ACCOUNT = source;
        DESTINATION_ACCOUNT = destination;
        AMOUNT = amount
    }
    public TransferMoneyContext() {
        lookupBindings()
    }
    public void doit()
    {
        SOURCE_ACCOUNT.transferTo()
    }
    private void lookupBindings() {
        // These are somewhat arbitrary and for illustrative
        // purposes. The simulate a database lookup
        InvestmentAccount investmentAccount = new InvestmentAccount();
        investmentAccount.increaseBalance(new Euro(100.00)); // prime it with some money

        SOURCE_ACCOUNT = investmentAccount;
        DESTINATION_ACCOUNT = new SavingsAccount();
        DESTINATION_ACCOUNT.increaseBalance(new Euro(500.00)); // start it off with money
        AMOUNT = new Euro(30.00)
    }
}

context PayBillsContext
{
    public PayBillsContext() {
       lookupBindings
    }
    role [] CREDITORS {
    } requires {
       Currency amountOwed()
    }
    stageprop SOURCE_ACCOUNT {
        public String accountID() const;
        public Currency availableBalance() const;
        public void increaseBalance(Currency amount) unused;
        public void decreaseBalance(Currency amount) unused;
        public void updateLog(String message, Date dt, Currency amount) unused
    } requires {
        String accountID() const;
        Currency availableBalance() const;
        void increaseBalance(Currency amount);
        void decreaseBalance(Currency amount);
        void updateLog(String message, Date dt, Currency amount)
    }

    // Use case behaviours

    public void doit()  {
        for (Creditor credit : CREDITORS) {
            // Note that here we invoke another Use Case
            TransferMoneyContext xfer = new TransferMoneyContext(
                                                      credit.amountOwed(),
                                                      SOURCE_ACCOUNT,
                                                      credit.account());
            xfer.doit()
        }
    }

    private void lookupBindings() {
       // These are somewhat arbitrary and for illustrative
       // purposes. The simulate a database lookup
       InvestmentAccount investmentAccount = new InvestmentAccount();
       investmentAccount.increaseBalance(new Euro(100.00)); // prime it with some money
       SOURCE_ACCOUNT = investmentAccount;

       Creditor [] creditors = new Creditor [2];

       creditors[0] = new ElectricCompany();
       creditors[1] = new GasCompany();

       CREDITORS = creditors
    }
}


{
    // Main

    TransferMoneyContext  aNewUseCase = new TransferMoneyContext();
    aNewUseCase.doit();

    PayBillsContext anotherNewUseCase = new PayBillsContext();
    anotherNewUseCase.doit()
}</pre>

C: `Account`(口座) という別のクラスがあります。ドメイン分析から抽出されたものです。口座番号というデータを持っている。メソッドは `increaseBalance()`(残高増) と `decreaseBalance()`(残高減) 。口座番号を与えれば、このメソッドを使っていろんなことができる。いわば、ちょっと高級な `Integer` みたいなものです・・・。全くもって良くないですね！貯金用の口座とか、投資用の口座とかどんどん増えていってしまいます。会社の口座、お金の債権者… ここまでができの良くないクラス指向のプログラミングのやり方でした。

C: さて、それでは送金のユースケースについて見ていきましょう。`Context` がキーワードです。多くの点でクラスのように見えるのだけれど、クラスとは異なっていて、ほとんどの場合データを持たずにロールだけを持っている。 `Amount`(金額) と呼ばれるロールはとてもシンプルなロールです。これで資金移動をすることができる。

C: （※デモを見せて）これがロールのインタフェースのシグネチャです。オブジェクトがこれらのメソッド全てをサポートするように定義する必要があります。これを「必須の契約」(※the requires contract)と言います。オブジェクトがロールを表現する場合、これらのメソッドを全てサポートしなければならない。

参加者C: ここで言う契約とは、「契約による設計」の契約のことですか？

C: その通りです。バートランド・メイヤーという人が提唱した契約による設計。この人の言ってることは、だいたい合っているけど突き詰められていない。

参加者B: メイヤーの考えのどこが機能しないんでしょう？

C: クラスAがあり、そこにメソッド1があります。そこには事前条件、メソッド実行、事後条件があります。メソッド1は事前条件が真であることを要求し、事後条件が保証されていることを約束します。次にメソッド1の中からメソッド2を呼ぶケースを考えてみましょう。メソッド2にとっての事前条件を見た時、メソッド2にとってはメソッド1が状態を維持してくれているという想定をしているわけですが、メソッド1の実行中に自己隠蔽されている状態の中で条件がおかしくなった状態でメソッド2を呼ぶとメソッド2が結果保証している事後条件が壊れてしまうことがある。

C: クラス側で本体となる実装を持たないのは、ロールのプレイヤーの持っている実装を使うだけだからです。ロールを演じるオブジェクトの実装を使います。別のオブジェクトに由来するメソッドを使うわけです。これらのメソッドはパブリックインタフェースで、ロールが何をするかを知ることができます。

C: よりわかり易い例として `SourceAccount`(振込元口座) と呼ばれる別のロールも見ていきましょう。`SourceAccount` は `transferTo()`(〜に送金する) として呼ばれるメソッド、責務を持っています。今からこの箇所のコメントを外して動くようにしましょう。トランザクションを開始しますよ。もし振込元口座が利用可能な残高を持っていれば、コメントを外した箇所は何になるでしょうか？

参加者B: ロールを注入されたオブジェクト

C: そうです！ `SourceAccount` ロールを適切に演じるオブジェクトです。利用可能な残高はどこでしょうか？気をつけてください。ここ（※ロール）にはありませんよ。残高は、ロールを演じる口座オブジェクトの側、ロールプレイヤーにあります。

C: クラスみたいだけれど、ランタイムで考えられるようにしてくれます。`SourceAccount` ロールを演じるオブジェクトについて考えてほしいです。ロールプレイヤーはマトリックス（※コンテキストが持つロールとオブジェクトのマッピング表）によってもたらされるものです。ロールがオブジェクトに注入されると、オブジェクトが機能を持つようになって、送金ユースケースを実現できるようになります。送金メソッドは振込元の `Account` オブジェクトに注入されます。

C: こちらは別のロール、`DestinationAccount`(振込先口座) です。`DestinationAccount`はメソッド `transferFrom()`(〜から送金される) を持ちます。`transferFrom()` メソッドは `DestinationAccount` の残高を更新します。

C: コンテキストのコンストラクタを見てみましょう。`TransferMoneyContext`(送金コンテキスト) です。コンテキストの中で何がオブジェクトかを伝える必要あります。送金の金額、振込元、振込先、口座の種類。

C: マトリックス（※コンテキストが持つロールとオブジェクトのマッピング表）を表すオブジェクトを持っていて、ロールのメソッドが振込元の `Account` オブジェクトに注入されます。要はこれ（※コンテキスト）はオブジェクトだと言うことです。`Account` オブジェクトに対して振込元口座にあるべきメソッド群を与えていきます。送金ユースケースのためにロールプレイヤーが演じるメソッド群です。そうすれば、ランタイムでオブジェクトを組み立てることができます。

C: 他のユースケース、お金を支払うについて見ていきましょう。特別なロールがあります。そして、`Amount` や `SavingsAccount`(貯蓄口座) のようなオブジェクトが新たに一つここにあります。これらは、プラグアンドプレイをするために用意したアクターです。私が台本を書いたロールがあり、アクターは芝居においてロールを演じます。

C: これは舞台のようなもので、大道具（stage props）はアクターの持っている道具です。大道具だけでは、一切何もできません。ただデータを引き出すことはできます。

参加者B補足: 何かをすることはできないのだけど、データを引き出すことはできる。

C: このロールの全てのメソッドは `const` で、アクターの状態を変更することはできません。

参加者B: `const` というのはロールの型かなにかですか？

C: そうです。ロールの束縛、制約の種類です。

C: さて、stage props (大道具に由来する機構名) の使いどころはわかりますか？オブジェクトは2つのロールを一度に演じるかもしれません。同じオブジェクト、同じインスタンスで二役を一度にこなすことがあります。とても複雑な例として再帰実行になるケースもありえます。これが先程上がっていた問題の一つで、複数回呼び出したら事前条件が崩れることがあるという例です。このstage propsはデータを更新しないから、問題が起こらないようにすることができます。

C: ちょっと難し目の例を見てみましょう。Javaのグラフ描画のデータ構造です。ブロック崩しと言われるピンポンゲームです。変数の中をチェックするデバッガも入っています。700行くらいのプログラムでこれができます。

C: MPDからDCIへどう変遷していくかについて。よく知られているように、対象のドメインを理解することから始めます。殆どの場合、インタラクティブなプログラムはクラス指向のプログラムでできています。オブジェクト指向プログラミングとは、アラン・ケイが定義したように、オブジェクト同士の協調によるネットワークです。これをどうやって意識するかについて。

## GertrudさんとCoplienさんによるユーザーストーリー、アジャイルとリーンアーキテクチャ

**Gertrud さん（以下、G）**: 皆さんはユーザーストーリーは知っていますか？ユースケースは？アジャイルになるためにはユーザーストーリーをたくさん作りましょう。リーンになるためには、ユースケースが必要。

G: ユーザーストーリーの形式（フォーム）として、以下のようなフォーマットで考えるといいです。

<pre>
As a ＜USER or ROLE＞ 例: 口座を持っている人として 
I want ＜FEATURE＞    例: ある口座から別の口座に移したいという行為
So I can ＜MOTIVATION＞   そうすればなにができるか。例えば、ダイアモンドを買ってあげることができる。
</pre>

ユーザーストーリーの構成要素はこのようになっています。これをブレインストーミングなどでたくさん作っていく。
上から、Who、What、Why に対応している。

G: ユーザーロールには、口座を持っている人、銀行の従業員、(※例えば電気代を支払おうとする）市民などがいます。

G: お金の送金は、今回の場合、全てのロールで行うケースがある。しかし、それぞれのロールでやりたい事は違う。ロールやモチベーションによってコンテキストが変わってくる。実装する前に、モチベーションがわかるので、実装することができる。これは実装する前に知る必要があるんです。

G: ユーザーストーリーの洗い出しは、ブレインストーミングであげていく。するとかなり数が増えていく。そこから一般的な（※抽象度の高い）状態に近づけていく。そうすれば、ユーザーストーリーからユースケースに落とし込むことができる。

G: ユーザーストーリーは具体的で、抽象的なのがユースケース。どこまで抽象的にするべきか？コードで理解できるレベルまでです。ユースケースをリーダブルコードに落とし込めるところまで持っていくのが目標になる。

G: ユーザーストーリーはインプットなので、なるべくたくさんの実例を出したほうがいい。銀行の業務であれば、昔に使っていた小切手の話をユーザーストーリーとして抽出するくらい。

G: ユースケースの段階で要点を厳密に抽出する。これがそのままプログラムに使われるから。

(ここで休憩に入りました)

G: ユースケースで大事なことはターミノロジー（※用語）。システム分析とシステム設計で同じボキャブラリー、ターミノロジーを使ってお互いの同意を取っていくことが大事。関係者の間で合意をとるのも大事になってくる。

G: 分析と設計で、同じボキャブラリー、同じターミノロジーを使って話をするので、相互に一貫性をもたせることができる。もちろん違うチームに行くと言葉も変わっていく。

G: 銀行だったら銀行員にも話を聞かないといけない。ドメインエキスパートだけではなく、銀行員もチームに入って、彼らの使っている用語をコードに落とし込んでいく。ユースケースレベルになると、コードのステップレベルまで落とし込めるようになる。

G: コアシナリオとサテライトシナリオというのがある。コアシナリオはシナリオの中でも本道で大事な物。サテライトシナリオは、例外的だったり拡張されていたり複雑だったりするもの。それでも、システムはこういうものもカバーしなければならない。

参加者: なぜ、「サテライト」という用語にしたか？

G: オリジナルの用語は、インクルードとエクスクルードだった。その使い分けはわかりにくいので、コアシナリオとサテライトシナリオにした。また、デビエイション（※deviation: 逸脱、脱線、偏向）と一時期は呼んでいた。コアシナリオじゃないなにか、そんな感じ。

G: ユースケースという用語は昔ながらの用語で古臭い感じがするので使わない方向でというのはある

G: 感覚的にはコアシナリオがスコープ全体の80%、残りはサテライトシナリオという割合になる。

G: サテライトシナリオのほうがアジャイルっぽいやり方で進めていける。そもそも複雑だったり、変わりやすかったりするので。

G: 具体的なユーザーストーリーをユースケースに落としていく時に、どこまで一般化するのか。例えば、銀行で資金移動する時に使う一般的な用語は送金するという用語とか、そういうことを考えてちょうどいい落とし所を見つけていく。

C: 銀行口座の残高がマイナスになった時に、自動的に所有者の他の口座から振り替えてマイナスを補填するような、システム起動のユーザーが関与しない形でのシステムオペレーションを考える。MVCはエンドユーザーから見た、それに基づいたプログラミングモデル。DCIはプログラマーサイドのユーザーモデルを表現している。先程の、ユーザーがでてこないシステムオペレーションのようなシステムオペレーションも自然に扱うことができる。

G: ターミノロジーがとても大事。ターミノロジーのデータベースを会社が持っていて、緻密に定義した辞書を持っている。そう、会社に辞書がある。プログラムの中の変数やファイル名なんかもそのデータベースを見て決める。
辞書に用語がなければ、辞書に追加したり変更したりする。追加するときにはちゃんと申請しなければいけない。

G: ソースコードの中にはコメントを書いてはいけない。さきほどの辞書を使って、ソースコードが、例えば英語からドイツ語などの自然言語にそのまま翻訳されるので。ソースコード中にコメントがあるとその内容をいちいち翻訳しなければならなくなる。

G: 最初に生まれた子供と同じくらいの注意深さで名前をつけよう。

C: ポリモーフィズムがないからコードが読みやすいのかもしれない。ポリモーフィズムがあると、メソッドがどこから呼び出されるかわからないからプログラムの見通しが悪くなる。ボキャブラリーも大事だが構造も大事。ある研究では、ユースケースのいろんな箇所にコードを分散させるとエラー率が70%増えるという調査結果がある。

C: リーンアーキテクチャの良いところとして、エラーを早く見つけることができることがある。テストをしているときではなく、コードを読んでいる時にエラーに気づくことができる。

C: フォードは組み立てを実際にして、テストは最後に確認している。トヨタの場合は、組立時にその場でテストをしている。リーンのアプローチの1つとして、問題をより早く見つけるという利点がある。

C: アジャイルとリーンについて。リーンはクラスの構造が重要。リーンにコストをかけて、アジャイルで実際にどう稼いで行くかを考える。アジャイルはオブジェクトのインタラクションが重要。

C:  ドメイン分析とユースケース分析も同時並行的にやる。同時並行的にやると重なる場所がある。みんなそれぞれ同時に実行していく。

C: 野中先生がトヨタにいた時に気づいた。それぞれの分析がかさなりあって進んでいく。これを、刺し身モデルという。（※ラグビーの）スクラムも刺し身。挟まり合っているでしょ。ウォーターフォールとは違って皆が全てのことをやるからスクラム。

C: リーンは長い期間のプランニングが必要で大事になってくる。アジャイルはフィードバックに応じた再調整が肝要。リーンは、深いレベルの専門家が作る。アジャイルはなんでもありみたいなところがある。スクラムとDCIはそのふたつのことをやる。

C: パターンも同じように両方のものを含む。これは日本の禅から来ていると思います。パターン系のガイダンスに従って、門を通ってゴールに近づくことはできるのではないか？パターンもスクラムも、ある程度のレベルではDCIも、ほとんどは日本由来のもの。道教だよね、これ。

C: リーン側のほうは、日本のトヨタの思想。山田 ひろしと言う有名なエンジニアでホンダの人がトヨタに教えた。（※原典見つけられず）

C: アジャイルのほうは、日本でよく知られた比喩で伝えるのは難しいですが… 社会的構造が違うので。デンマークは、社会がとてもフラット。日本は階層的なところがある。よりシステム化されているので、色々なルールが存在する。日本は、もうちょっとアジャイルのほうをエクササイズするといいかもしれません。

C: 実際の開発の流れ。ドメイン分析 → 共通性可変性分析 → クラス → インタフェース、API設計。最初はインタフェースしか書かない。もしかしたらスタブくらいは書くかもしれない。実装はいつ書くか？ユースケースのタイミングで。Just In Time。ユースケースが必要とするインタフェースだけを実装する。（※インターフェイスをクラスで実装するのではなく、インターフェイス自体を実装する）インタフェースは抽象的な概念で、ロールは具体的なインタラクションやアルゴリズムなので、そのタイミングで。

C: この中でプラットフォームを開発している人。プラットフォーム開発はリーンではない。あらかじめ用意しておくもの。だからリーンではない。使わないものを作る時は会社が危なくなる。これは、無駄と言います。

C: DCIアーキテクチャを利用した開発の順番としては、まずアーキテクチャを作ってそれからそれぞれデータベースとかGUIのインタフェースを作る。会社としてなにを売るかというのはいろいろあるけど、売るのはユースケース。それぞれのクラスを売っているわけではない。ユースケースは、それぞれのデータベースとかサーバーとかクライアントのユーザーインタフェースなどを少しずつ重ねたもの。

C: 生産性が一番の無駄を生むと、トヨタの大野耐一さんがおっしゃっている。

C: こういう話を、「object-composition」というメーリングリストで話をしているので、入ってくださいね。

C: 1月にまた東京に来るので、またこういう会を開催しましょう！

最後に、GertrudさんとCoplienさんと参加メンバーで集合写真を撮りました。

<figure class="tmblr-full" data-orig-height="2448" data-orig-width="3264"><img src="https://78.media.tumblr.com/c333cf348635583400cf951ce0ef9df6/tumblr_inline_oz6lu97wp01ted0fb_540.jpg" data-orig-height="2448" data-orig-width="3264" alt="image"></figure>

> **NOTE:** 当日は同時翻訳付きの英語トークでした。通訳の任を快諾してくださった、 [@ganchiku](https://twitter.com/ganchiku) さん、[@remore](https://twitter.com/remore) さん、ありがとうございました。また、記事を書くにあたっては英語の下訳で [@kuma_nana](https://twitter.com/kuma_nana) さんに貢献していただきました。こちらもありがとうございます。もちろん、当ポストの文責は執筆者である私にあります。

## 関連リンク

- [fulloo.info](http://fulloo.info/)
- [The trygve language project](https://github.com/jcoplien/trygve)
- [マルチパラダイムデザイン読書会](https://github.com/phpmentors-jp/mpdosaka)
- [[Reenskaug 2006] Trygve Reenskaug. MVC and DCA Example program comments](http://heim.ifi.uio.no/~trygver/2006/09-JavaZone/mvc-dca.pdf)
- [object-compositionメーリングリスト](https://groups.google.com/forum/#!forum/object-composition)

## 関連記事

- [PHP Mentors -> Beyond MVC](http://phpmentors.jp/post/69076928673/beyond-mvc)
- [PHP Mentors -> Debasish Ghosh氏のブログ記事「ドメイン駆動設計：可変性の管理」を翻訳しました](http://phpmentors.jp/post/129261828893/ddd-managing-variability)
- [PHP Mentors -> PHPカンファレス2015 PHPメンターズセミナー「モデルを設計せよ！―ドメイン駆動設計を超えて」参加レポート](http://phpmentors.jp/post/131193847968/phpcon2015-phpmentors-workshop-design-models-beyond-ddd)
