---
title: 事前に実行する環境とテストの設定
description: 事前に実行する環境とテストの設定
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---

# 事前認定での環境のセットアップとテスト{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>このページの内容は、情報提供のみを目的として提供されています。 この API を使用するには、Adobe Systems からの最新のライセンス版が必要です。 無断使用は認められません。

このテクニカルノートの目的は、パートナーが環境を設定し、Adobe Systems事前認定環境に展開された新しいビルドをテストする開始を支援することです。

プロダクション&#x200B;***と***&#x200B;ステージング&#x200B;***の2つのビルドフレーバー***&#x200B;があるため、このドキュメントでは、ステージングのすべての手順が同じであり、URLのみが異なることに言及して、本番環境のセットアップフォーカスするます。

ステップ1と2はテストマシンの1つにテスト 環境を設定し、ステップ3は基本的なフローの検証であり、ステップ4と5はいくつかのテストガイドラインを示しています。

>[!IMPORTANT]
>
> テスト 環境を変更するたびに手順 1 と 2 を実行することが非常に重要です (ステージングから運用プロファイルに切り替えるか、またはその逆)


## 手順 1. IP に渡すドメインの解決 {#resolving-pass-domain-to-an-ip}

* スプーフィングに使用できるロードバランサー IP を見つけるには、次のコマンドを実行します。

* **Windows の場合**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

* **Linux/Mac の場合**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>ドメインは関連性がなく、ユーザーごとに異なる可能性があるため、～に回答から除外ユーザー。

>[!IMPORTANT]
>
> これらの IP アドレスは将来変更される可能性があり、地理的に異なる地域のユーザーでは同じではない可能性があります。


## ステップ2.  本番環境となる事前認定環境のスプーフィング {#spoofing-the-prequalification-environment}

* *c:\\windows\\System32\\drivers\\etc\\hosts* ファイル(Windows)または */etc/hosts* ファイル(Macintosh/Linux/Android の場合)編集、次の内容を追加します。

* 実稼働プロファイルのなりすまし
   * 52.13.71.11 http://entitlement.auth.adobe.com、http://sp.auth.adobe.com、http://api.auth.adobe.com

**Android でのスプーフィング：** Android でスプーフィングを行うには、Android エミュレーターを使用する必要があります。

* スプーフィングを設定したら、次のように、実稼動およびステージングプロファイルの通常の URL を使用できます ( つまり、 `http://sp.auth-staging.adobe.com` および `http://entitlement.auth-staging.adobe.com` そして実際に *事前認定環境/実稼動環境* *の新しいビルドの。


## 手順 3.  適切な環境を指していることを確認します。 {#Verify-you-are-pointing-to-the-right-environment}

**これは簡単なステップです。**

* エンタイトルメント PREQUAL 環境](https://entitlement-prequal.auth.adobe.com/environment.html) および[エンタイトルメント](https://entitlement.auth.adobe.com/environment.html)を読み込み[ます。同じ応答が返されます。


## ステップ4.  プログラマーのWebサイトを使用した簡単な認証/認証フローを実行します {#peform-a-simple-auth-flow}

* この手順では、プログラマーの Web サイト アドレスといくつかの有効な MVPD 資格情報 (認証および承認されているユーザー) が必要です。

## ステップ5.  プログラマーの Web サイトを使用したシナリオ・テストの実行 {#perform-scenario-testing-using-programmer-website}

* 環境のセットアップを完了し、基本的な認証認証フローが機能していることを確認したら、より複雑なシナリオのテストに進むことができます。


## 手順 6.  API テストサイトを使用したテストの実行 {#perform-testing-using-api-testing-site}

* Adobe Pass認証のテストを詳しく調べるには、 [API テストサイト](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

API テストサイトの詳細は、 [Adobeの API テストサイトを使用した認証フローと承認フローのテスト方法](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
