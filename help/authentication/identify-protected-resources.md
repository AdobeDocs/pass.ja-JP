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
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

各認証要求（または認証を確認する要求）には、ユーザーがアクセスを要求する保護されたリソースの一意の識別子を含める必要があります。 保護されたリソースは、MVPD と参加するプログラマーの間で合意された、許可された任意のレベルのコンテンツにすることができます。 保護されているリソースの可能性は、より具体的な精度のこのツリー構造に適合する必要があります。

- ネットワーク
   - チャネル
      - 表示
         - エピソード
            - アセット

</br>

## メディア RSS 形式 {#media_rss}

リソースは、単純な文字列（チャネルの一意の識別子）で識別できます。また、Adobe( またはAdobe Pass認証の承認パートナー ) と参加する MVPD およびプログラマーとの間で合意されたとおりに、メディア RSS 形式 (MRSS) で表すこともできます。 リソース指定子として使用される RSS 文字列には、評価や親の制御メタデータなどの追加情報を含めることができます。


「TNT」などの単純なリソース識別子を使用する場合、チャネルを表すと見なされ、次の RSS リソース指定子に変換されます。

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


より複雑な指定子は、例えば追加の評価情報を含めることができます。 RSS 文字列全体を、リソース ID を必要とする Access Enabler 関数に渡すことができます。例えば、 [`getAuthorization()`](/help/authentication/rest-api-reference.md):

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

リソース指定子はAdobe Pass認証に対して不透明で、単に MVPD に渡されます。 MVPD がリソース指定子を認識しないか解析できない場合は、Adobe Pass認証にエラーが返され、エラーが `tokenRequestFailed()` コールバック。

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
