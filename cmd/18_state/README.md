# State
State（ステート、 状態）は、オブジェクトの内部状態が変化した時にその挙動を変化させる
それはあたかもそのオブジェクトのクラスが変わったかのように見える

![state](https://refactoring.guru/images/patterns/content/state/state-ja.png)

## 構造
![UML](https://refactoring.guru/images/patterns/diagrams/state/structure-ja-indexed.png)

1. コンテキスト （Context） は、 複数個ある具象状態クラスのオブジェクトのいずれか一つへ参照を保存し、 状態に固有の作業は、 すべてそのオブジェクトに委任されます。 コンテキストは、 状態インターフェースを介して状態オブジェクトと通信します。 コンテキストには、 新しい状態オブジェクトを渡すための公開の setter があります。

2. ステート （State） インターフェースは状態固有のメソッドを宣言します。 これらのメソッドはすべての具象ステートに意味のあるものでなければなりません。 いくつかの状態では絶対に呼ばれない無駄なメソッドがないようにしたいからです。

3. 具象ステート （Concrete State、 具象状態） は、 状態固有のメソッドに対して独自の実装を行います。 似たようなコードが複数の状態で重複することを避けるために、 共通の振る舞いを内包した中間層の抽象クラスを作ることもできます。ステート・オブジェクトは、 コンテキスト・オブジェクトへの逆参照を格納するようにもできます。 この参照を通して、 ステートはコンテキスト・オブジェクトから必要な情報を取得したり、 状態遷移を開始したりできます。

4. コンテキストと具象ステートの両方とも、 コンテキストの次の状態の設定が可能で、 コンテキストにリンクされたステート・オブジェクトを置き換えることにより実際の状態遷移を行います。

## 実装方法
1. コンテキストとして適切なクラスを決定します。 状態依存のコードがある既存のクラスがそれかもしれません。 もし状態固有のコードが複数のクラスに分散している場合は、 新規クラスが必要となります。

2. 状態インターフェースを宣言します。 コンテキスト内で宣言されたメソッド全部を反映させることもできますが、 ステート固有の振る舞いを持つものだけに絞ることを目指してください。

3. 実際の状態ごとに、 状態インターフェースを実装するクラスを作成します。 次に、 コンテキストのメソッドを見渡し、 その状態に関連するすべてのコードを新しく作成したクラスに抽出します。コードを状態クラスに移動する際に、 それがコンテキストの非公開メンバーに依存していることを発見するかもしれません。 これにはいくつかの回避策があります：

  - これらのフィールドまたはメソッドを公開にする。
  - 抽出中の振る舞いをコンテキスト中の公開メソッドとし、 状態クラスから呼び出す。 醜いが、 即効性があり、 後で修正可能。
  - ステート・クラスをコンテキスト・クラスにネスト。 ただし、 プログラミング言語がクラスのネストをサポートしている場合に限定。

4. コンテキスト・クラスに、 状態インターフェースの型の参照フィールドと、 そのフィールドの値を変更できる公開 setter を追加します。

5. コンテキストのメソッドを再度一つずつ検証し、 空 （から） の状態条件文を状態オブジェクトに対する対応したメソッドの呼び出しと置き換えます。

6. コンテキストの状態を切り替えるには、 状態クラスのどれか一つのインスタンスを作成し、 コンテキストに渡します。 これは、 コンテキスト自身の内部でも、 種々の状態内部、 あるいはクライアント内でもできます。 どこで行うにせよ、 クラスは、 それがインスタンスを作成した具象状態クラスに依存するようになります。


## 他のパターンとの関係
- Bridge、 State、 Strategy （と限られた意味合いでは、 Adapter も） は、 非常に似た構造をしています。 実際のところ、 これらの全てのパターンは、 合成に基づいており、 仕事を他のオブジェクトに委任します。 しかしながら、 違う問題を解決します。 パターンは、 単にコードを特定の方法で構造化するためのレシピではありません。 パターンが解決する問題に関して、 開発者同士がするコミュニケーションの道具でもあります。

- State は、 Strategy の拡張と考えることができます。 どちらのパターンも合成 （コンポジション） に基づいており、 コンテキストの振る舞いの変更を、 ヘルパー・オブジェクトに仕事の一部を委任することにより行います。 Strategy では、 これらのオブジェクトは完全に独立しており、 互いを意識しません。 しかし、 State では、 具象状態間の依存関係を制限せず、 コンテキストの状態を自由に変更できます。

## 引用元

> https://refactoring.guru/ja/design-patterns/state
> https://refactoring.guru/images/patterns/content/state/state-ja.png
> https://refactoring.guru/images/patterns/diagrams/state/structure-ja-indexed.png