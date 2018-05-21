---
layout: post
title:  "DCI Tokyo 2 - Commonality / Variability Analysis: Practical MPD - が開催されました"
date:   2018-4-20 08:42:00 +0900
categories: DCITokyo
---

2018年1月10日に開催された DCI Tokyo 1 に続き、2018年3月27日に DCI Tokyo 2 が開催されました。今回も James Coplien [@jcoplien](https://twitter.com/jcoplien) さんをお招きしてのトークセッションとなりました。会場は [株式会社ヴァル研究所](https://www.val.co.jp/) 様に提供していただきました。

セッションは、前回同様 [@remore](https://twitter.com/remore) さんと [@ganchiku](https://twitter.com/ganchiku) さんによる同時通訳とともに進められました。

今回のテーマはマルチパラダイムデザイン（Multi-Paradigm Design: MPD）の中核を成し、DCI / リーンアーキテクチャ（Lean Architecture）とも深く関係する 共通性／可変性分析 でした。

レポートは [@smori1983](https://twitter.com/smori1983) が担当させていただきます。

当日の様子は Coplien さんの許可を得て YouTube の DCI Tokyo 公式アカウントにて公開されています。Coplien さんのおもしろTシャツにも注目しながら、ぜひご覧ください。

- [DCI Tokyo 2- Commonality / Variability Analysis: Practical MPD by James Coplien (Part 1 of 4)](https://www.youtube.com/watch?v=4rgPBzR8nVg)
- [DCI Tokyo 2- Commonality / Variability Analysis: Practical MPD by James Coplien (Part 2 of 4)](https://www.youtube.com/watch?v=XtlPEnr77us)
- [DCI Tokyo 2- Commonality / Variability Analysis: Practical MPD by James Coplien (Part 3 of 4)](https://www.youtube.com/watch?v=Gfw2SEpx62E)
- [DCI Tokyo 2- Commonality / Variability Analysis: Practical MPD by James Coplien (Part 4 of 4)](https://www.youtube.com/watch?v=QNVmbzXlN0I)
- [DCI Tokyo 2- Commonality / Variability Analysis: Practical MPD by James Coplien (extra)](https://www.youtube.com/watch?v=CDY-h4QBlVc)

## 全体の流れ

全体の流れとしては、1980年代以降のソフトウェア開発の歴史を、主にオブジェクト指向に対する考え方の変遷を通して振り返りつつ、MPD と DCI の関係性に迫るものとなりました。

## オブジェクト指向（1988）

当時は C++ の他に、テレコム業界向け言語である CHILL や、軍需産業向け言語である Ada などの言語があり、それらの言語がオブジェクト指向なのかという議論がされていました。

最終的に C++ が残ることになりますが、C++ の作者である Stroustrup 氏は、そもそも C++ をオブジェクト指向言語ではなく、マルチパラダイム言語と定義していたことに注意する必要があります。

当時考えられていたオブジェクト指向の構成要素は次の3項目でした。

1. inheritance / 継承
1. polymorphism / ポリモーフィズム
1. instantiation / インスタンス化

1994年に Coplien さんは David Weiss 氏と Robert Chi Tao Lai 氏とともに仕事をしますが、その時、共通性／可変性分析を通じて、ソフトウェアファミリの振る舞いなどを特徴づけ、当時取り組んでいたソフトウェアのための DSL(Domain Specific Language) を開発しました。開発言語は C でした。

この、共通性に注目してグルーピングしたものをファミリと定義する方法は、Weiss 氏らが考案したFAST(Family Abstraction Specification Technique) という手法に基づいています。

DSL は小さなドメインを対象にしていて、ドメインが変化しない限りはうまくいったそうですが、ドメインは変化していくものです。DSL は、デバッガなど周辺技術との密な関係性の中で成立するため、その変化に対しては脆いという問題がありました。

ただし、共通性／可変性分析自体は非常に有用で、ドメイン分析に対しては非常に多くの収穫があったそうです。

分析の結果得られる共通性／可変性テーブルの内容の埋め込み（注: 解決ドメインの抽象による実装）に関して、

- DSL で埋め込むと、固くなりすぎ、脆くなる。
- C で埋め込むと、言語特有の制約が多く、開発者の intentionality（意図）がわからなくなる。

という課題がありましたが、C++ を用いることで、よりうまくいくようになりました。

## オブジェクト指向（1995）

Coplien さんは、C++ を

> 共通性／可変性分析のための、汎用言語キットである

と捉えます。

その視点で見たオブジェクト指向は、次の4項目の構成要素で理解されるようになりました。

1. Common Signature
1. Common Data
1. Variation in Data
1. Variation in Algorithm

また、polymorphism / instantiation は、共通性／可変性のバインディングタイム（注: 共通性／可変性が変数に束縛されるタイミング）の選択の問題として捉え直されることになりました。

## オブジェクト指向（DCI 時代）

Coplien さんによれば、更にこの後、3段階の気付きを経て、現在のオブジェクト指向理解へとつながっているそうです。

### 1段階目: There  is no OO in C++

- 2010年くらいまでは、オブジェクト指向という言葉を、抽象データ型（ Abstract Data Type: ADT ）という意味で使っていた。
- C++ は実際には抽象データ型ではなく、具象データ型（ Concrete Data Type: CDT ）であるということ。

### 2段階目: There are no objects in source code

- C++ プログラムはソースコードである。
- そこにはオブジェクトは存在しない。ソースコードに記述しているのはクラスである。
- 多くの人々が Java などの言語でやっているのはクラス指向プログラミングである。

### 3段階目: OO is mental model

- Trygve Reenskaug 氏とオブジェクトとは何か、オブジェクトパラダイムとは何かについて話し合い、DCI が生まれた。
- DCI は、抽象データ型の実装を意味したこれまでのオブジェクト指向とは別の視点を与える。
- 実行時におけるオブジェクトのネットワークがどのように問題を解決するかに注目する（ Alan Kay の定義）。
- 我々はクラスとロールをごちゃまぜにして考えていた。
    - 抽象データ型について考えるのがクラス指向。
    - ロールについて考えるのがオブジェクト指向。
    - 抽象データ型＋インスタンス化がロールを実行（play）。

## MPD と DCI

MPD は、クラス指向のレイヤーにおいてドメイン分析を行う時、ソフトウェアファミリの共通性／可変性分析を通じてドメインモデルを構築する手法を提供します。

DCI は、我々のメンタルモデルに基づいて、オブジェクトどうしの相互作用を定義します。そこでは、抽象データ型としてのクラスに具体的なロールを注入されたオブジェクトが、特定のコンテキストのもとに配置され、相互作用するという、動的な側面を捉える方法を提供します。

以上を簡単にまとめると、次の表のようになるかと思います。

|パラダイム|設計対象|領域|手段|視点|
|---|---|---|---|---|
|**DCI**|オブジェクトとそれらの相互作用|ロール / コンテキスト|ユースケース|Time|
|**MPD**|抽象データ型、モデルの構造|クラス / Abstract Base Class (ABC)|共通性／可変性分析|Space|

歴史的には MPD が先行しました。それは後に、モデルの静的な構造の分析を行うための領域を担う設計方法論という捉え直しがされました。そして、オブジェクト指向に対する新しい視点を提供する DCI が、クラス指向では扱うことが難しかった、モデルの動的な関係性を捉えるための方法論として提起されています。

## Time and Space と DCI における共通性／可変性

DCI は、オブジェクトどうしの相互作用に注目します。つまり基本的には時間の経過（シーケンス）が意識されます。しかし、あるロールを付与された複数のオブジェクトが配置されてこそ、相互作用が成立するものです。

コンテキストとは、このように、ある状況に参加する主体の空間的配置と時間的な相互作用の総体として捉えられるものと言えます。

ここで、日本人にとってはありふれた「間」という概念が取り上げられました。具体例としては次のようなものが話題にあがりました。

- 歌舞伎（ Coplien さん）
- 龍安寺の石庭（ [@koriym](https://twitter.com/koriym) さん）

また、アレグザンダーの考案したパターンランゲージに話が及びました。パターンランゲージは、建築の構造（空間的なパターン）だけではなく、その構造が生み出す人々の相互作用（時間的なパターン）も内包したものとして理解されるべきです。

> pattern of behavior = pattern in space + pattern in time

セッションの終盤では、このように、DCI が担う領域における共通性／可変性概念の適用可能性が示唆されました。

## さいごに

会場を提供していただきましたヴァル研究所様、DCI Tokyo スタッフの皆様、今回もすばらしいイベントとなりました。ありがとうございました。

次回は、ワークショップ形式での開催も視野に入れながら、DCI Tokyo 3 が開催されることと思います。MPD / DCI / Lean Architecture に興味のある方は、ぜひご参加ください。
