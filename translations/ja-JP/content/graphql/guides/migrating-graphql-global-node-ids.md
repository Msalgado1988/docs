---
title: GraphQLグローバルノードIDの移行
intro: 2つのグローバルノードIDフォーマットについて、そして旧来のフォーマットから新フォーマットへの移行方法について学びます。
versions:
  fpt: '*'
  ghec: '*'
topics:
  - API
shortTitle: グローバルノードIDの移行
---

## 背景

{% data variables.product.product_name %} GraphQL APIは、現在2種類のグローバルノードIDフォーマットをサポートしています。 旧来のフォーマットは非推奨と成り、新しいフォーマットで置き換えられます。  このガイドは、必要な場合の新フォーマットへの移行方法を紹介します。

新しいフォーマットに移行することで、リクエストに対するレスポンスタイムが一貫して小さくなることが保証できます。 また、旧来のIDが完全に非推奨になっても、アプリケーションが動作し続けられることも保証できます。

旧来のグローバルノードIDフォーマットが非推奨になる理由について学ぶには、「[GraphQLに新しいグローバルIDフォーマットが到来](https://github.blog/2021-02-10-new-global-id-format-coming-to-graphql)」を参照してください。

## 対応の必要性の判断

GraphQLグローバルノードIDへの参照を保存している場合にのみ、移行のステップを踏んでいかなければなりません。  これらのIDは、スキーマ中の任意のオブジェクトの`id`フィールドに対応します。  グローバルノードIDをまったく保存していないなら、変更なしにAPIを扱い続けられます。

加えて、現在は旧来のIDを型情報を抽出するためにデコードしている（たとえば`PR_kwDOAHz1OX4uYAah`の最初の2文字を利用してそのオブジェクトがPull Requestかを判断している場合）なら、IDのフォーマットが変更されたために、そのサービスは動かなくなります。  これらのIDを不透明な文字列として扱うよう、サービスを移行しなければなりません。  これらのIDは一意になるので、直接参照として依存できます。


## 新しいグローバルIDへの移行

新しいIDフォーマットへの移行を容易にするために、GraphQL APIリクエスト中で`X-Github-Next-Global-ID`ヘッダを利用できます。 `X-Github-Next-Global-ID`の値は`1`もしくは`0`にできます。  値を`1`にすれば、`id`フィールドをリクエストされたすべてのオブジェクトに対し、レスポンスのペイロードでは常に新しいIDフォーマットが使われるようになります。  値を`0`に設定するとデフォルトの動作に戻ります。この場合、オブジェクトの作成日に応じて旧来のIDもしくは新IDが示されます。

以下は、cURLを使ったリクエストの例です:

```
$ curl \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "X-Github-Next-Global-ID: 1" \
  https://api.github.com/graphql \
  -d '{ "query": "{ node(id: \"MDQ6VXNlcjM0MDczMDM=\") { id } }" }'
```

クエリでは旧来のIDの`MDQ6VXNlcjM0MDczMDM=`が使われていますが、レスポンスには新しいIDのフォーマットが含まれています。
```
{"data":{"node":{"id":"U_kgDOADP9xw"}}}
```
`X-Github-Next-Global-ID`ヘッダを使うと、アプリケーションで参照する旧来のIDに対する新IDフォーマットが見つかります。 レスポンスで受信されたIDで、それらの参照を更新できます。 旧来のidへの参照をすべて更新し、移行のAPIへのリクエストでは新しいIDフォーマットを使わなければなりません。 バルク操作を行う際には、1つのAPIコールで複数ノードのクエリをサブミットするために、エイリアスを利用できます。 詳しい情報については「[GraphQLのドキュメント](https://graphql.org/learn/queries/#aliases)」を参照してください。

アイテムのコレクションに対して新しいIDを取得することもできます。 たとえば、Organization中の最後の10個のリポジトリの新しいIDを取得したい場合は、以下のようなクエリを使うことができます。
```
{
  organization(login: "github") {
    repositories(last: 10) {
      edges {
        cursor
        node {
          name
          id
        }
      }
    }
  }
}
```

`X-Github-Next-Global-ID`を`1`に設定していることが、クエリの返値の中のすべての`id`フィールドに影響していることに注意してください。  これは、非`node`クエリをサブミットしていたとしても、`id`フィールドをリクエストすれば新しいフォーマットのIDが返されるということを意味します。

## フィードバックを送る

自分のアプリケーションに影響するこの変更のロールアウトに関して懸念があるなら、[{% data variables.product.product_name %}にお問い合わせ](https://support.github.com/contact)いただき、私どもがうまく支援させていただけるよう、アプリケーション名などの情報をお知らせください。
