# Mediator
オブジェクト間の混沌とした依存性を削減する。
パターンはオブジェクト間の直接の通信を制限し、メディエーター・オブジェクトを介してのみの共同作業を強制する

![mediator](https://refactoring.guru/images/patterns/content/mediator/mediator.png)

## 構造
![UML](https://refactoring.guru/images/patterns/diagrams/mediator/structure-indexed.png)

1. コンポーネント （Component） は、 何らかのビジネス・ロジックを含んだ種々のクラスです。 各コンポーネントは、 メディエーター・インターフェースの型で宣言されたメディエーターへの参照を持っています。 コンポーネントは、 メディエーターの実際のクラスが何かは知りません。 そのため、 別のメディエーターにリンクすることで、 他のプログラムのコンポーネントを再利用することができます。

2. メディエーター （Mediator） インターフェースは、 コンポーネントとの通信するメソッドを宣言します。 通常は、 通知メソッド一つだけからなります。 コンポーネントは、 このメソッドの引数として任意のコンテキストを渡すことができます。 それはオブジェクト自身でもかまいませんが、 受け手のコンポーネントと送り手のクラスが結合しないような方法に限ります。

3. 具象メディエーター （Concrete Mediator） は、 様々なコンポーネント間の関係を内に隠します。 具象メディエーターはしばしば、 管理するすべてのコンポーネントへの参照を保持し、 時にはそのライフサイクルを管理することもあります。

4. コンポーネント （Component） は他のコンポーネントの存在を知る必要はありません。 コンポーネント内で、 コンポーネントに対して、 何か重要なことが発生した場合は、 メディエーターにのみ通知する必要があります。 メディエーターは通知を受け取ると、 送り手を簡単に識別でき、 どのコンポーネントの引き金を引けばいいかを決めるには、 その情報だけで十分かもしれません。コンポーネントの観点からすると、 すべては、 完全なブラックボックスのように見えます。 送り手は誰がそのリクエストを処理することになるのかはわからず、 受け手は誰が最初にリクエストを送信したのかを知りません。

## 実装方法
1. 独立性の強化の恩恵を受けそうな密に結合されたクラスのグループを特定します （例： 保守を容易にする。 クラス再利用の簡易化）。

2. メディエーター・インターフェースを宣言し、 メディエーターと様々なコンポーネント間の望ましい通信プロトコルを記述します。 多くの場合、 コンポーネントから通知を受け取るメソッド一つで十分です。異なる状況でコンポーネント・クラスを再利用したい場合、 このインターフェースは、 極めて重要な役を果たします。 コンポーネントが一般的インターフェースを介してメディエーターと動作する限り、 コンポーネントを異なる実装のメディエーターとリンクさせることができます。

3. 具象メディエーター・クラスを実装します。 このクラスの管理するすべてのコンポーネントへの参照を格納するようにしておくと、 メディエーターのメソッドからどのコンポーネントでも呼ぶことができ、 便利です。

4. さらに進んで、 コンポーネント・オブジェクトの作成と破壊をメディエーターにやらせるようにもできます。 こうすると、 メディエーターは、 ファクトリーやファサードに類似してくるかもしれません。

5. コンポーネントはメディエーター・オブジェクトへの参照を格納すべきです。 この接続は通常、 コンポーネントのコンストラクター内で確立され、 コンストラクターにはメディエーター・オブジェクトが引数として渡されます。

6. コンポーネントのコードを変更して、 他のコンポーネントのメソッドの代わりに、 メディエーターの通知メソッドを呼び出すように します。 他のコンポーネントを呼び出すようなコードを抽出して、 メディエーター・クラスに入れます。 メディエーターがそのコンポーネントから通知を受けるたびに、 このコードを実行します。

## 他のパターンとの関係
- Chain of Responsibility と Command と Mediator と Observer は、 リクエストの送り手と受け手を接続する様々な方法を示します：

  - Chain of Responsibility は、 潜在的受け手の動的な連鎖に沿って、 どれか一つが処理するまで、 リクエストを順番に渡します。
  - Command は、 送り手と受け手との間で単方向の接続を確立します。
  - Mediator は、 送り手と受け手の間の直接の接続を削除し、 メディエーター・オブジェクトを介しての間接的通信を強制します。
  - Observer では、 受け手が動的にリクエストの受信申し込みをしたり、 申し込み取り消しをしたりできます。
  
- Facade と Mediator は、 多くの密に結合されたクラス間の協力関係を組織するという似た目的があります。

  - Facade は、 オブジェクトのサブシステムへの簡略化されたインターフェースを定義しますが、 新しい機能が導入されるわけではありません。 サブシステム自体はファサードを認識しません。 サブシステム内のオブジェクト同士は直接やりとりします。
  - Mediator は、 システムのコンポーネント間の通信を一元化します。 コンポーネントはメディエーター・オブジェクトについてのみ知っており、 直接やりとりしません。

- Mediator と Observer との違いは、 理解に苦しむことがあります。 ほとんどの場合、 これらのパターンのいずれかを実装すればいですが、 両方を同時に適用することもできます。 どうすればそれができるか見てみましょう。

  - Mediator の主目的は、 システム構成コンポーネント間の相互依存をなくすことです。 その代わりに、 これらのコンポーネントは単一のメディエーター・オブジェクトに依存するようになります。 Observer の主目的は、 オブジェクト間の動的な単方向の接続を確立することにあり、 そこではあるオブジェクトが他のオブジェクトの部下として動作します。

  - Observer に依存した、 Mediator パターンの有名な実装方法があります。 メディエーター・オブジェクトがパブリッシャーとしての役割を果たし、 他のコンポーネントがサブスクライバーとしてメディエーターのイベントに通知依頼をしたり依頼解除をします。 このように Mediator を実装すると、 Observer と非常に似たよう見えます。

  - 混乱した時は、 Mediator パターンは、 他の方法でも実装できるということを忘れないでください。 たとえば、 同一のメディエーター・オブジェクトにすべてのコンポーネントを恒久的にリンクできます。 この実装方法は、 Observer には似ていませんが、 Mediator パターンの適用例の一つと言えます。

  - ここで、 プログラム中のすべてのコンポーネントがパブリッシャーとなり、 お互いに動的な接続が許された状況を想像してみてください。 ここでは、 中心となるメディエーター・オブジェクトはなく、 分散されたオブザーバーの集団があるだけです。

## 引用元

> https://refactoring.guru/ja/design-patterns/mediator
> https://refactoring.guru/images/patterns/content/mediator/mediator.png
> https://refactoring.guru/images/patterns/diagrams/mediator/structure-ja-indexed.png