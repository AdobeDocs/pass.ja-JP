---
title: 環境の設定と事前テスト
description: 環境の設定と事前テスト
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# 事前認定での環境のセットアップとテスト{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>このページの内容は、情報提供のみを目的として提供されています。 この API を使用するには、Adobe Systems からの最新のライセンス版が必要です。 無断使用は認められません。

このテクニカルノートの目的は、パートナーが環境を設定し、Adobe Systems事前認定環境に展開された新しいビルドをテストする開始を支援することです。

***プロダクション***&#x200B;と&#x200B;***ステージング***&#x200B;の2つのビルドフレーバーがあるため、このドキュメントでは、ステージングのすべてのステップが同じであり、URLのみが異なることに言及して、プロダクションのセットアップフォーカスするします。

ステップ1と2はテストマシンの1つにテスト環境を設定し、ステップ3は基本的なフローの検証であり、ステップ4と5はいくつかのテストガイドラインを示しています。

>[!IMPORTANT]
>
> テスト 環境を変更するたびに手順 1 と 2 を実行することが非常に重要です (ステージングから運用プロファイルに切り替えるか、またはその逆)


## 手順 1. IP へのパスドメインの解決 {#resolving-pass-domain-to-an-ip}

* スプーフィングに使用できるロードバランサーの IP を見つけるには、次のコマンドを実行します。

* **Windows の場合**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **Linux/Mac の場合**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>ドメインは関連性がなく、ユーザーごとに異なる可能性があるため、～に回答から除外ユーザー。

>[!IMPORTANT]
>
> これらの IP アドレスは将来変更される可能性があり、地理的に異なる地域のユーザーでは同じではない可能性があります。


## ステップ2.  本番環境となる事前認定環境のスプーフィング {#spoofing-the-prequalification-environment}

* *c:\\windows\\System32\\drivers\\etc\\hosts* ファイル(Windows)または */etc/hosts* ファイル(Macintosh、Linux、Android の場合)編集、次の内容を追加します。

* 実稼働プロファイルのなりすまし
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Androidでのスプーフィング：** Androidでスプーフィングするには、Android エミュレーターを使用する必要があります。

* スプーフィングが行われたら、実稼働プロファイルとステージングプロファイルに通常の URL （つまり、`http://sp.auth-staging.adobe.com` と `http://entitlement.auth-staging.adobe.com`）を使用するだけで、新しいビルドの *事前選定環境/実稼働* に実際にヒットします。


## 手順 3.  正しい環境を指していることを確認します {#Verify-you-are-pointing-to-the-right-environment}

**これは簡単なステップです。**

* [エンタイトルメント PREQUAL 環境](https://entitlement-prequal.auth.adobe.com/environment.html) および [エンタイトルメント](https://entitlement.auth.adobe.com/environment.html)を読み込みます。同じ応答が返されます。


## ステップ4.  プログラマーのWebサイトを使用した簡単な認証/認証フローを実行します {#peform-a-simple-auth-flow}

* この手順では、プログラマーの Web サイト アドレスといくつかの有効な MVPD 資格情報 (認証および承認されているユーザー) が必要です。

## ステップ5.  プログラマーの web サイトを使用してシナリオテストを実行 {#perform-scenario-testing-using-programmer-website}

* 環境の設定を完了し、基本的な認証と承認のフローが機能していることを確認したら、より複雑なシナリオのテストに進むことができます。


## 手順 6.  API テストサイトを使用したテストの実行 {#perform-testing-using-api-testing-site}

* Adobe Pass認証のテストを詳しく調べる場合は、[API テストサイト &#x200B;](http://entitlement-prequal.auth.adobe.com/apitest/api.html) を使用することをお勧めします。

APIテストサイトの詳細については、 [Adobe SystemsのAPIテストサイトを使用してAuthenticationフローと認可フローテスト方法](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)を参照してください。
