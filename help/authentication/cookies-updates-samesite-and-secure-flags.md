---
title: Cookie の更新 – SameSite およびセキュアフラグ
description: Cookie の更新 – SameSite およびセキュアフラグ
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Cookie の更新 – SameSite およびセキュアフラグ {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>


## 更新 {#Updates}

この節では、サードパーティ Cookie の処理に関してChrome ブラウザーおよびAdobe Pass認証によって導入された変更内容について説明します。



### Chrome 80 のアップデート {#Chrome}

Chrome バージョン 80 （バージョン 82 を除く）以降では、*SameSite* 属性を指定しない cookie は *SameSite=Lax* として扱われます。 そのため、クロスサイトコンテキストで配信する必要がある Cookie は、*SameSite=None* を明示的に指定し、*Secure* 属性でマークして *HTTPS* 経由で配信する必要があります。 これらの更新に関する詳細は、Chromium の公式ページ（<https://www.chromium.org/updates/same-site> と <https://web.dev/samesite-cookies-explained/>）からも読むことができます。


### Adobe Pass認証の更新 {#Pass-Updates}

Adobe Pass Authentication Service は現在、一部のプラットフォームやバージョンのAdobe Pass Authentication SDK と組み合わせて機能するために、Chromeを含むブラウザーの観点からサードパーティ Cookie と見なされる一部の Cookie に依存しています。 そのため、今後の変更内容に準拠し、これらの古い SDK からクロスサイトコンテキストでこれらの Cookie を引き続き配信するために、Adobe Pass認証サービスは *adobe-pass-2.55.1* バージョンで必要な変更内容を実装します。

*adobe-pass-2.55.1* バージョンからのこれらの変更には、バージョン 80 以降（バージョン 82 を除く）のChrome ブラウザーを使用する場合に、すべてのAdobe Pass Authentication SDK に返されるすべての cookie の *Secure* 属性と *SameSite=None* 属性が追加されることが含まれます。

次の節では、1 人のユーザーがAdobe Pass ブラウザー 80 以降（バージョン 82 を除く）を使用している場合に発生する可能性のある問題を、プラットフォームとChrome Authentication SDK のバージョンのリストで示します。

## トラブルシューティング {#Troubleshooting}

この節を参照する際は、すべてのブラウザーに対して、すべてのAdobe Pass認証サービス cookie の *adobe-pass-2.55.1* バージョンに *Secure* 属性を設定する必要があります。また、*SameSite=None* 属性は、Chrome ブラウザーのバージョン 80 以上（バージョン 82 を除く）にのみ設定する必要があります。


### 一般的なトラブルシューティング {#General}

1. 重要：一部のユーザーエージェントは、*SameSite=None* 属性に対応していないことがわかっています。

   - Chrome 51 からChrome 66 までのChromeのバージョン（両端を含む）。 これらのChrome バージョンでは、*SameSite=None* が設定された Cookie が拒否されます。 これは、Android WebView だけでなく、Chromium 派生ブラウザーの古いバージョンにも影響します。 この動作は、当時の cookie 仕様のバージョンに従って正しく行われていましたが、仕様に新しい「なし」値が追加されたことで、Chrome 67 以降でこの動作が更新されました。 （Chrome 51 より前は、SameSite 属性は完全に無視され、すべての cookie は *SameSite=None* として扱われていました。）
   - バージョン 12.13.2 以前のAndroidの UC ブラウザーのバージョン。古いバージョンでは *SameSite=None* の Cookie を拒否します。 この動作は、当時の cookie 仕様のバージョンに従って正しく動作していましたが、仕様に新しい「なし」値が追加され、UC ブラウザーの新しいバージョンでこの動作が更新されました。
   - macOS 10.14 の Safari および埋め込みブラウザーのバージョンとiOS 12 のすべてのブラウザー。 これらのバージョンでは、*SameSite=None* がマークされた Cookie は、*SameSite=Strict* とマークされたかのように誤って処理されます。 このバグは、iOSおよびMacOSの新しいバージョンで修正されました。


1. 重要：*セキュア* 属性を持つ cookie は *HTTPS* 経由で送信する必要があります。そうでない場合、cookie はAdobe Pass Authentication サービスに到達しません。

   - AccessEnabler JavaScript SDK:
      - 動的クライアント登録を導入する前に、*sp.auth.adobe.com* との通信で、バージョン *2.35* および *3.5.0* に *HTTPS* を使用することが必須です。
   - AccessEnabler iOS/tvOS SDK:
      - 動的クライアント登録を導入する前に、*sp.auth.adobe.com* との通信で *3.0.0* より前のバージョンの場合は *HTTPS* を使用することが必須です。
   - AccessEnabler Android SDK:
      - 動的クライアント登録を導入する前に、*sp.auth.adobe.com* との通信で *3.0.0* より前のバージョンの場合は *HTTPS* を使用することが必須です。
   - AccessEnabler FireOS SDK:
      - *sp.auth.adobe.com* との通信で、バージョン *2.0.4* に *HTTPS* を使用する必要があります。

</br>

### AccessEnabler JavaScript SDK バージョン 2.35 のトラブルシューティング {#235-Troubleshooting}

Chrome 80 以降（バージョン 82 を除く）では、ユーザーの認証フローが影響を受ける可能性があります。 上記の更新が原因でユーザーの認証に問題が発生していないことを確認するために、次の操作を行うことができます。

- *JSESSIONID* cookie がブラウザーで設定され、*SameSite=None* 属性と *Secure* 属性が設定されていることを確認します。
- *https://sp.auth.adobe.com/authenticate/saml* ネットワーク リクエストの *JSESSIONID* cookie が *https://sp.auth.adobe.com/session* ネットワーク リクエストの *JSESSIONID* cookie と一致することを確認します。


### AccessEnabler JavaScript SDK バージョン 3.5.0 のトラブルシューティング {#350-Troubleshooting}

Chrome 80 以降（バージョン 82 を除く）では、ユーザーの認証フローが影響を受ける可能性があります。 上記の更新が原因でユーザーの認証に問題が発生していないことを確認するために、次の操作を行うことができます。

- *JSESSIONID* cookie がブラウザーで設定され、*SameSite=None* 属性と *Secure* 属性が設定されていることを確認します。
- *https://sp.auth.adobe.com/authenticate/saml* ネットワーク リクエストの *JSESSIONID* cookie が *https://sp.auth.adobe.com/session* ネットワーク リクエストの *JSESSIONID* cookie と一致することを確認します。
- *pass\_sfp* cookie がブラウザーに設定され、*SameSite=None* 属性と *Secure* 属性が設定されていることを確認します。
- *pass\_sfp* cookie が *https://sp.auth.adobe.com/session* ネットワーク リクエストで設定されていることを確認します。


Chrome 80 以降（バージョン 82 を除く）では、ユーザーの認証フローが影響を受ける可能性があります。 保護されたリソースを監視するのにユーザーが問題を抱えていないことを確認するために、正常に認証された後、上記の更新により、次のことが可能になります。

- *pass\_sfp* cookie がブラウザーに設定され、*SameSite=None* 属性と *Secure* 属性が設定されていることを確認します。
- *pass\_sfp* cookie が *https://sp.auth.adobe.com/adobe-services/authorize* ネットワーク リクエストで設定されていることを確認します。
- *pass\_sfp* cookie が *https://sp.auth.adobe.com/adobe-services/shortAuthorize* ネットワーク リクエストで設定されていることを確認します。
