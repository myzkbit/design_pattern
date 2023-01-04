# Prototype

既存オブジェクトのコピーをそのクラスに依存することなく可能とする

![prototype](https://refactoring.guru/images/patterns/content/prototype/prototype.png)

## 構造
![UML](https://refactoring.guru/images/patterns/diagrams/prototype/structure-indexed.png)

1. プロトタイプ （Prototype） インターフェースはクローン作成メソッドを宣言します。 ほとんどの場合、 ただ一つの clone メソッドからできています。

2. 具象プロトタイプ （Concrete Prototype） のクラスは、 クローン作成メソッドを実装します。 元のオブジェクトのデータをコピーするだけではなく、 このメソッドはクローン作成時にクローン同士をリンクするとか、 再起的依存性を取り払うなどの作業も行うかもしれません。

3. クライアント （Client） は、 プロトタイプのインターフェースに従うどんなオブジェクトでも、 そのコピーを生成できます。

## 実装方法
1. プロトタイプのインターフェースを作成し、 そこで clone メソッドを宣言します。 または、 既存のクラス階層がある場合は、 階層下のすべてのクラスにメソッドを追加するだけでもかまいません。

2. プロトタイプ・クラスは、 そのクラスのオブジェクトを引数として受け取る代替コンストラクターを定義しなければなりません。 コンストラクターは、 渡されたオブジェクトから新しく作成されたインスタンスにクラスで定義されたすべてのフィールドの値をコピーする必要があります。 サブクラスを変更する場合は、 スーパークラスが非公開フィールドのクローン作成処理できるように親コンストラクターを呼び出す必要があります。お使いのプログラミング言語がメソッドの多重定義をサポートしていない場合は、 オブジェクトのデータを複製するための特別なメソッドを定義することになります。 コンストラクターは、 new 演算子を呼び出した直後に生成されたオブジェクトを使えるため、 これを行うためのより便利な場所です。

3. クローン作成メソッドは通常、 プロトタイプのクラスの new 演算子を呼び出すだけの一行だけです。 どのクラスもクローン作成メソッドを、 new に続けてそのクラス名が来るように上書きしなければなりません。 そうしないと、 親クラスのオブジェクトがクローンされてしまいます。

4. 必要ならば、 頻繁に使用されるプロトタイプのカタログを備蓄しておく、 集中型プロトタイプ・レジストリーを作成します。レジストリーは、 新しいファクトリー・クラスとして実装することもできますし、 またはプロトタイプを取得する静的メソッドをプロトタイプの基底クラスに追加することもできます。 このメソッドは、 クライアント・コードからメソッドに渡される検索条件に基づいてプロトタイプを検索します。 検索条件は単純な文字列によるタグかもしれませんし、 または複雑な検索パラメータの組み合わせかもしれません。 適切なプロトタイプが見つかった後、 レジストリーはそれをクローンし、 クライアントに返します。最後に、 サブクラスのコンストラクターへの直接呼び出しを、 プロトタイプ・レジストリーのファクトリー・メソッドへの呼び出しに置き換えます。

## 他のパターンとの関係
- 多くの設計は、 まず比較的単純でサブクラスによりカスタマイズ可能な、 Factory Method から始まり、 次第に、 もっと柔軟だが複雑な Abstract Factory や Prototype や Builder へと発展していきます。

- Abstract Factory クラスは、 多くの場合 Factory Methods の集まりですが、 Prototype を使ってメソッドを書くこともできます。

- Prototype は、 Commands のコピーを履歴に保存する必要がある場合に役立ちます。

- Composite と Decorator を多用する設計に対しては、 Prototype の使用が有益かもしれません。 このパターンを適用すると、 複雑な構造を初めから再構築するのではなく、 それをクローンします。

- Prototype は継承に基づいていないので、 継承の欠点はありません。 一方、 Prototype は、 クローンされたオブジェクトの複雑な初期化が必要となります。 Factory Method は継承に基づいていますが、 初期化のステップは必要ありません。

- 場合によっては、 Prototype を Memento の代わりに使用した方が簡単な場合があります。 状態の履歴を保存したいオブジェクトが比較的単純で、 他の外部リソースへのリンクを持たないか簡単に再現できる場合に、 この方法が使えます。

- Abstract Factories、 Builders、 Prototypes はどれも Singletons で実装可能です。

## 引用元

> https://refactoring.guru/ja/design-patterns/prototype
> https://refactoring.guru/images/patterns/content/prototype/prototype.png
> https://refactoring.guru/images/patterns/diagrams/prototype/structure-indexed.png