---
title: 環境の設定と事前定期テスト
description: 環境の設定と事前定期テスト
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: c2a5591cd8fea44f66fc25beb1fb40532e18d8a6
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# 環境の設定と事前定期テスト{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このテクニカルノートの目的は、パートナーが環境を設定し、Adobeの事前検証環境にデプロイされた新しいビルドのテストを開始できるようにすることです。

ビルドフレーバーは&#x200B;***実稼動***&#x200B;と&#x200B;***ステージング***&#x200B;の2つがあるので、このドキュメントでは、実稼動設定に焦点を当て、すべての手順がステージングで同じであること、URLのみが異なることを説明します。

手順1および2は、いずれかの試験機で試験環境を設定し、手順3は基本フローの検証で、手順4および5は一部の試験ガイドラインを示します。

>[!IMPORTANT]
>
> テスト環境を変更するたびに（ステージングから実稼動プロファイルへの切り替え、またはその逆）ステップ 1と2を実行することが非常に重要です


## 手順1: IPへのパスドメインの解決 {#resolving-pass-domain-to-an-ip}

* スプーフィングに使用できるロードバランサーIPを見つけるには、次のコマンドを実行します。

* **Windows**&#x200B;上

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

``Choose any IP from **addresses** section (e.g. `52.13.71.11)``

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

``Choose any IP from **addresses** section (e.g. `54.190.212.171)``


* **Linux/Macの場合**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

``Choose any IP from **A records (**e.g `52.13.71.11)``

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

``Choose any IP from **A records (**e.g `54.190.212.171)``

>[!NOTE]
>
>関連性がなく、ユーザーごとに異なる可能性があるため、回答から除外されるドメイン。

>[!IMPORTANT]
>
> これらのIP アドレスは将来的に変更される可能性があり、地理的に異なる地域のユーザーについては同じではない可能性があります。


## 手順2:  プリクオリフィケーション環境を実稼動環境にスプーフィングする {#spoofing-the-prequalification-environment}

* *c:\\windows\\System32\\drivers\\etc\\hosts* ファイル （Windows）または&#x200B;*/etc/hosts* ファイル （Macintosh/Linux/Android上）を編集し、次のファイルを追加します。

* Spoofの制作プロファイル
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Androidでのスプーフィング：** Androidでスプーフィングするには、Android エミュレーターを使用する必要があります。

* スプーフィングが設定されたら、実稼動プロファイルとステージングプロファイルに通常のURLを使用するだけです（つまり、`http://sp.auth-staging.adobe.com`と`http://entitlement.auth-staging.adobe.com`）。実際には、新しいビルドの&#x200B;*事前資格環境/実稼動*&#x200B;をヒットします。


## 手順3:  正しい環境を指していることを確認します {#Verify-you-are-pointing-to-the-right-environment}

**これは簡単な手順です：**

* [使用権限の事前定義環境](https://entitlement-prequal.auth.adobe.com/environment.html)と[使用権限](https://entitlement.auth.adobe.com/environment.html)を読み込みます。 同じ回答を返さなければなりません。


## 手順4:  プログラマーのweb サイトを使用して簡単な認証/認証フローを実行する {#peform-a-simple-auth-flow}

* この手順では、プログラマーのweb サイトのアドレスと有効なMVPD資格情報（認証および承認されたユーザー）が必要です。

## 手順5:  プログラマーのweb サイトを使用してシナリオテストを実行する {#perform-scenario-testing-using-programmer-website}

* 環境設定を完了し、基本的な認証認証フローが機能していることを確認したら、より複雑なシナリオのテストを進めることができます。


## 手順6:  API テストサイトを使用したテストの実行 {#perform-testing-using-api-testing-site}

* Adobe Pass認証のテストについて詳しく説明する場合は、[API テストサイト &#x200B;](http://entitlement-prequal.auth.adobe.com/apitest/api.html)を使用することをお勧めします。

API テストサイトの詳細については、[AdobeのAPI テストサイトを使用して認証と認証フローをテストする方法](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)を参照してください。
