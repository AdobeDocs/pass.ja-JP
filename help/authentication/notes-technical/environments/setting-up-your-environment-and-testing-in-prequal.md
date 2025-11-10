---
title: 環境の設定と事前テスト
description: 環境の設定と事前テスト
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# 環境の設定と事前テスト{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このテクニカルノートの目的は、パートナーが環境を設定し、Adobeの事前認定環境にデプロイされた新しいビルドのテストを開始するのを支援することです。

***実稼動*** と ***ステージング*** の 2 つのビルドフレーバーがあるので、このドキュメントでは、実稼動のセットアップに焦点を当て、すべての手順がステージングで同じで、URL のみが異なることを説明します。

手順 1 と 2 は、いずれかのテストマシンでテスト環境を設定します。手順 3 は、基本的なフローの検証で、手順 4 と 5 は、いくつかのテストガイドラインを示します。

>[!IMPORTANT]
>
> テスト環境を変更する（ステージングから実稼動プロファイルに切り替える、またはその逆の場合）たびに、手順 1 と 2 を実行することが非常に重要です


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


* **Linux/Macの場合**

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
>関連性がなく、ユーザーによって異なる可能性があるので、回答から除外されたドメイン。

>[!IMPORTANT]
>
> これらの IP アドレスは将来的に変更される可能性があり、異なる地域のユーザーで同じにならない場合があります。


## 手順 2.  実稼動環境への事前認定環境のスプーフィング {#spoofing-the-prequalification-environment}

* *c:\\windows\\System32\\drivers\\etc\\hosts* ファイル（Windows の場合）または */etc/hosts* ファイル（Macintosh/Linux/Androidの場合）を編集して、次の内容を追加します。

* スプーフィング生産プロファイル
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Androidでのスプーフィング：** Androidでスプーフィングするには、Android エミュレーターを使用する必要があります。

* スプーフィングが行われたら、実稼働プロファイルとステージングプロファイルに通常の URL （つまり、`http://sp.auth-staging.adobe.com` と `http://entitlement.auth-staging.adobe.com`）を使用するだけで、新しいビルドの *事前選定環境/実稼働* に実際にヒットします。


## 手順 3.  正しい環境を指していることを確認します。 {#Verify-you-are-pointing-to-the-right-environment}

**これは簡単な手順です：**

* [&#x200B; 使用権限前環境 &#x200B;](https://entitlement-prequal.auth.adobe.com/environment.html) および [&#x200B; 使用権限 &#x200B;](https://entitlement.auth.adobe.com/environment.html) を読み込みます。 同じ応答を返す必要があります。


## 手順 4.  プログラマーの web サイトを使用して、単純な認証/承認フローを実行します。 {#peform-a-simple-auth-flow}

* この手順では、プログラマーの web サイトアドレスと、一部の有効なMVPD資格情報（認証および承認されたユーザー）が必要です。

## 手順 5.  プログラマーの web サイトを使用してシナリオテストを実行 {#perform-scenario-testing-using-programmer-website}

* 環境の設定を完了し、基本的な認証と承認のフローが機能していることを確認したら、より複雑なシナリオのテストに進むことができます。


## 手順 6.  API テストサイトを使用したテストの実行 {#perform-testing-using-api-testing-site}

* Adobe Pass認証のテストを詳しく調べる場合は、[API テストサイト &#x200B;](http://entitlement-prequal.auth.adobe.com/apitest/api.html) を使用することをお勧めします。

API テストサイトについて詳しくは、[Adobe API テストサイトを使用して認証フローと承認フローをテストする方法 &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md) を参照してください。
