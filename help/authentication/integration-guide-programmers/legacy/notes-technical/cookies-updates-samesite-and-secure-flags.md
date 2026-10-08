---
title: Cookieの更新 – 同一サイトとセキュアフラグ
description: Cookieの更新 – 同一サイトとセキュアフラグ
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 0%
---
# （レガシー） Cookieの更新 – 同一サイトとセキュアフラグ {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>


## 更新 {#Updates}

この節では、サードパーティ Cookieを処理するためにChrome ブラウザーとAdobe Pass認証が導入した変更点について説明します。



### Chrome 80のアップデート {#Chrome}

Chrome バージョン 80 （バージョン 82を除く）以降、*SameSite*&#x200B;属性を指定しないCookieは、*SameSite=Lax*&#x200B;であるかのように扱われます。 したがって、クロスサイトコンテキストで配信する必要があるCookieは、*SameSite=None*&#x200B;を明示的に指定し、*Secure*&#x200B;属性でマークして&#x200B;*HTTPS*&#x200B;経由で配信する必要があります。 これらの更新に関する詳細は、公式のchromium ページから読むことができます：<https://www.chromium.org/updates/same-site>、また<https://web.dev/samesite-cookies-explained/>。


### Adobe Pass認証の更新 {#Pass-Updates}

Adobe Pass認証サービスは、現在、いくつかのプラットフォームおよびバージョンのAdobe Pass認証SDKと組み合わせて機能するために、Chromeを含むブラウザーの観点からサードパーティ Cookieと見なされるいくつかのCookieに依存しています。 したがって、今後の変更に準拠し、これらの古いSDKからクロスサイトコンテキストでこれらのCookieを引き続き配信するために、Adobe Pass認証サービスは&#x200B;*adobe-pass-2.55.1* バージョンで必要な変更を実装します。

*adobe-pass-2.55.1* バージョンからのこれらの変更には、バージョン 80以降（バージョン 82を除く）以降のChrome ブラウザーを使用する場合にすべてのAdobe Pass Authentication SDKに渡されるすべてのCookieに&#x200B;*Secure*&#x200B;および&#x200B;*SameSite=None*&#x200B;属性を追加することが含まれます。

次の節では、1人のユーザーがChrome ブラウザー80以降（バージョン 82を除く）を使用している場合に備えて、プラットフォームとAdobe Pass認証SDKのバージョンのリストに関する潜在的な問題について説明します。

## トラブルシューティング {#Troubleshooting}

この節では、すべてのAdobe Pass Authentication Service Cookieには、すべてのブラウザーに対して&#x200B;*adobe-pass-2.55.1* バージョンで&#x200B;*Secure*&#x200B;属性を設定する必要がありますが、*SameSite=None*&#x200B;属性はChrome ブラウザーのバージョン 80以降（バージョン 82を除く）にのみ設定する必要があることに注意してください。


### 一般的なトラブルシューティング {#General}

1. 一部のユーザーエージェントは、*SameSite=None*&#x200B;属性と互換性がないことが判明していることに注意してください。

   - ChromeのChrome 51からChrome 66までのバージョン（両端を含む）。 これらのChrome バージョンは、*SameSite=None*&#x200B;のCookieを拒否します。 これは、古いバージョンのChromium派生ブラウザーとAndroid WebViewにも影響します。 このビヘイビアーは、当時のcookie仕様のバージョンに従って修正されましたが、新しい「なし」値が仕様に追加されたことで、このビヘイビアーはChrome 67以降で更新されました。 （Chrome 51より前は、SameSite属性は完全に無視され、すべてのCookieは&#x200B;*SameSite=None*&#x200B;であるかのように扱われていました）。
   - バージョン 12.13.2以前のAndroidのUC ブラウザーのバージョン。 古いバージョンでは、*SameSite=None*&#x200B;のCookieは拒否されます。 この動作は、当時のcookie仕様のバージョンに従って正しかったのですが、新しい「なし」値が仕様に追加されたことで、この動作は新しいバージョンのUC ブラウザーで更新されました。
   - SafariのバージョンとMacOS 10.14の組み込みブラウザー、およびiOS 12のすべてのブラウザー。 これらのバージョンでは、*SameSite=None*&#x200B;とマークされたCookieを、*SameSite=Strict*&#x200B;とマークされたかのように誤って処理します。 このバグは、iOSおよびMacOSの新しいバージョンで修正されました。


1. *Secure*&#x200B;属性を持つCookieは、*HTTPS*&#x200B;経由で送信する必要があります。そうしないと、CookieはAdobe Pass Authentication サービスに届きません。

   - AccessEnabler JavaScript SDK:
     - 動的クライアント登録を導入する前に、*sp.auth.adobe.com*&#x200B;との通信で、バージョン *2.35*&#x200B;および&#x200B;*3.5.0*&#x200B;に&#x200B;*HTTPS*&#x200B;を使用することが必須です。
   - AccessEnabler iOS/tvOS SDK:
     - *sp.auth.adobe.com*&#x200B;との通信で、*3.0.0*&#x200B;より前のバージョンで&#x200B;*HTTPS*&#x200B;を使用してから、動的クライアント登録を導入することが必須です。
   - AccessEnabler Android SDK:
     - *sp.auth.adobe.com*&#x200B;との通信で、*3.0.0*&#x200B;より前のバージョンで&#x200B;*HTTPS*&#x200B;を使用してから、動的クライアント登録を導入することが必須です。
   - AccessEnabler FireOS SDK:
     - *sp.auth.adobe.com*&#x200B;との通信で、バージョン *2.0.4*&#x200B;に&#x200B;*HTTPS*&#x200B;を使用することが必須です。

</br>

### AccessEnabler JavaScript SDK バージョン 2.35 トラブルシューティング {#235-Troubleshooting}

Chrome 80以降（バージョン 82を除く）では、ユーザーの認証フローが影響を受ける可能性があります。 上記の更新により、ユーザーが認証に問題を抱えていないことを確認するために、次のことが可能です。

- *JSESSIONID* Cookieがブラウザーで設定されており、*SameSite=None*&#x200B;および&#x200B;*Secure*&#x200B;属性が設定されていることを確認してください。
- *https://sp.auth.adobe.com/authenticate/saml* ネットワーク要求の&#x200B;*JSESSIONID* Cookieが、*https://sp.auth.adobe.com/session* ネットワーク要求の&#x200B;*JSESSIONID* Cookieと一致することを確認します。


### AccessEnabler JavaScript SDK バージョン 3.5.0 トラブルシューティング {#350-Troubleshooting}

Chrome 80以降（バージョン 82を除く）では、ユーザーの認証フローが影響を受ける可能性があります。 上記の更新により、ユーザーが認証に問題を抱えていないことを確認するために、次のことが可能です。

- *JSESSIONID* Cookieがブラウザーで設定されており、*SameSite=None*&#x200B;および&#x200B;*Secure*&#x200B;属性が設定されていることを確認してください。
- *https://sp.auth.adobe.com/authenticate/saml* ネットワーク要求の&#x200B;*JSESSIONID* Cookieが、*https://sp.auth.adobe.com/session* ネットワーク要求の&#x200B;*JSESSIONID* Cookieと一致することを確認します。
- *pass\_sfp* Cookieがブラウザーで設定されており、*SameSite=None*&#x200B;および&#x200B;*Secure*&#x200B;属性が設定されていることを確認してください。
- *pass\_sfp* Cookieが&#x200B;*https://sp.auth.adobe.com/session* ネットワーク要求で設定されていることを確認してください。


Chrome 80以降（バージョン 82を除く）では、ユーザーの認証フローが影響を受ける可能性があります。 上記の更新により、ユーザーが正常に認証された後、保護されたリソースを視聴する際に問題が発生しないようにするために、次のことが可能です。

- *pass\_sfp* Cookieがブラウザーで設定されており、*SameSite=None*&#x200B;および&#x200B;*Secure*&#x200B;属性が設定されていることを確認してください。
- *pass\_sfp* Cookieが&#x200B;*https://sp.auth.adobe.com/adobe-services/authorize* ネットワーク要求で設定されていることを確認してください。
- *pass\_sfp* Cookieが&#x200B;*https://sp.auth.adobe.com/adobe-services/shortAuthorize* ネットワーク要求で設定されていることを確認してください。
