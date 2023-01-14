# Chain of Responsibility
Chain of Responsibility （責任の連鎖）は、リクエストをそれに関するすべての情報を含む独立したオブジェクトに転換する
この転換によりリクエストをメソッドの引数として渡したり、リクエストの実行を遅らせたり、待ち行列に入れたり、取り消し操作を行なうことが可能になる

![chain_of_responsibility](https://refactoring.guru/images/patterns/content/chain-of-responsibility/chain-of-responsibility.png)

## 構造
![UML](https://refactoring.guru/images/patterns/diagrams/chain-of-responsibility/structure-indexed.png)

1. ハンドラー （Handler） は、 すべての具象ハンドラーに共通したインタフェースを宣言します。 通常は、 リクエストを処理するためのメソッド一つだけを含んでいますが、 時には連鎖上の次のハンドラーを設定するための別のメソッドを含んでいることもあります。

2. 基底ハンドラー （Base Handler） は、 ある場合とない場合があります。 すべてのハンドラー・クラスに共通する定型のコードを入れることができます。通常、 このクラスには次のハンドラーへの参照を格納するためのフィールドが定義されています。 クライアントは、 ハンドラーを、 その手前のハンドラーのコンストラクターか setter に渡すことで、 連鎖を構築できます。 クラスは、 次のハンドラーがあることを確認した後、 実行を次に移すといった、 デフォルトの処理動作を実装することもできます。

3. 具象ハンドラー （Concrete Handler） にはリクエストを処理するための実際のコードが含まれています。 リクエストを受け取ると、 各ハンドラーはそれを処理するかどうか、 さらには連鎖に沿ってそれを渡すかどうかを決める必要があります。ハンドラーは通常、 自己完結型かつ不変であり、 コンストラクターを通して必要なすべてのデータを一度だけ受け入れます。

4. クライアント （Client） は、 一度だけ連鎖を構成するかもしれませんし、 動的に構成するかもしれません。 これは、 アプリケーションのロジックによります。 リクエストは連鎖内のいずれのハンドラーにも送ることができます。 最初のハンドラーとは限りません。

## 実装方法
1. ハンドラー・インターフェースを宣言し、 そこでリクエストを処理するメソッドのシグネチャーを記述します。クライアントがリクエスト・データをどうにメソッドに渡すかを決めます。 最も柔軟な方法は、 リクエストをオブジェクトに変換し、 それを処理メソッドに引数として渡すことです。

2. 具象ハンドラー内同士の定形コードの重複を排除するために、 ハンドラー・インターフェースから派生した抽象基底クラスを作成する価値があるかもしれません。このクラスには、 連鎖内の次のハンドラーへの参照を格納するためのフィールドが必要です。 クラスを変更不可にすることを検討してください。 ただし、 実行時に連鎖を変更する計画がある場合は、 参照フィールドの値を変更する setter を定義する必要があります。次のオブジェクトがある限りそこへリクエストを転送することを、 処理メソッドの便利なデフォルトとして実装することもできます。

3. ハンドラーの具象サブクラスの作成と、 その処理メソッドを一つずつ実装していきます。 各ハンドラーは、 リクエストを受け取る時に以下の二つの事項を決定をする必要があります：

  - リクエストを処理すべきか。
  - 連鎖に沿ってリクエストを渡すべきか。

4. クライアントは、 それ自身で連鎖を組み立てるかもしれませんし、 または他のオブジェクトから事前構築済みの連鎖を受け取ることもできます。 後者の場合、 構成または環境設定に従って連鎖を構築するためのファクトリー・クラスをいくつか実装する必要があります。

5. クライアントは、 最初のハンドラーに限らず、 連鎖内のどのハンドラーからでも、 処理を開始できます。 リクエストは、 あるハンドラーが次に渡すことを拒否するか、 あるいは連鎖の最後に到達するまで、 連鎖沿いに渡されます。

6. 連鎖は動的に変化するので、 クライアントは次のような状況に対処できる必要があります：

  - 連鎖には、 リンクが一つしかない。
  - 一部のリクエストは連鎖の最後に到達しないかもしれない。
  - 他のリクエストは処理されないまま、 連鎖の最後に到達するかもしれない。


## 他のパターンとの関係
- Chain of Responsibility と Command と Mediator と Observer は、 リクエストの送り手と受け手を接続する様々な方法を示します：

  - Chain of Responsibility は、 潜在的受け手の動的な連鎖に沿って、 どれか一つが処理するまで、 リクエストを順番に渡します。
  - Command は、 送り手と受け手との間で単方向の接続を確立します。
  - Mediator は、 送り手と受け手の間の直接の接続を削除し、 メディエーター・オブジェクトを介しての間接的通信を強制します。
  - Observer では、 受け手が動的にリクエストの受信申し込みをしたり、 申し込み取り消しをしたりできます。

- Chain of Responsibility は、 よく Composite と一緒に使われます。 この場合、 リーフ （末端） のコンポーネントがリクエストを受ける時、 リクエストは、 全部の親コンポーネントからオブジェクト・ツリーのルート （根） までを通るかもしれません。

- Chain of Responsibility のハンドラーは、 Commands で実装可能です。 この場合、 リクエストに代表される同一のコンテキスト・オブジェクトに対して多くの異なる処理を実行できます。しかしもう一つのやり方は、 リクエスト自身をコマンド・オブジェクトとすることです。 この場合、 連鎖にリンクされた異なる一連のコンテキスト中で同じ処理を実行できます。

- Chain of Responsibility と Decorator とは非常によく似た階層構造をしています。 両パターンとも、 一連のオブジェクトを通して実行を渡すために、 再起的合成に依存します。 しかしながら、 いくつかの重要な違いがあります。CoR ハンドラーは、 互いに独立して任意の処理を実行可能です。 また、 任意の時点でそれ以上リクエストを渡すのを止めることもできます。 一方、 様々な Decorator では、 オブジェクトの振る舞いの拡張をする時、 基底インターフェースとの一貫性を保つ必要があります。 さらに、 デコレーターはリクエストの流れを断ち切ることは許されていません。

## 引用元

> https://refactoring.guru/ja/design-patterns/chain-of-responsibility
> https://refactoring.guru/images/patterns/content/chain-of-responsibility/chain-of-responsibility.png
> https://refactoring.guru/images/patterns/diagrams/chain-of-responsibility/structure-indexed.png