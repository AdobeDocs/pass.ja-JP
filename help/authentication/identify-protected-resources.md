---
title: 保護されたリソースの識別
description: 保護されたリソースの識別
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# 保護されたリソースの識別 {#identifying-protected-resources}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

各認証リクエスト（または認証を確認するリクエスト）には、ユーザーがアクセスをリクエストしている保護されたリソースの一意の識別子が含まれている必要があります。 保護されるリソースは、MVPD と参加するプログラマーの間で合意された、承認された任意のレベルのコンテンツにすることができます。 保護される可能性のあるリソースは、細分性が向上する次のようなツリー構造に適合する必要があります。

- ネットワーク
   - チャネル
      - 表示
         - エピソード
            - アセット

</br>

## メディア RSS 形式 {#media_rss}

リソースは、単純な文字列（チャネルの一意の ID）で識別することも、Adobe（またはAdobe Pass認証承認済みパートナー）と参加する MVPD およびプログラマーの間で合意されているように、メディア RSS フォーマット（MRSS）で表すこともできます。 リソース指定子として使用される RSS 文字列には、規制や保護者による制限のメタデータなどの追加情報を含めることができます。


「TNT」などの単純なリソース識別子を使用する場合、その識別子はチャネルを表すものと見なされ、この RSS リソース指定子に変換されます。

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


より複雑な指定子には、例えば、追加の評価情報が含まれることがあります。 [`getAuthorization()`](/help/authentication/rest-api-reference.md) のようなリソース ID を必要とするアクセス イネーブラ機能に RSS 文字列全体を渡すことができます。

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

リソース指定子は、Adobe Pass認証に対して不透明です。これらは単に MVPD に渡されます。 MVPD がリソース指定子を認識しない、または解析できない場合は、Adobe Pass Authentication にエラーを返し、`tokenRequestFailed()` コールバックにエラーを返します。

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
