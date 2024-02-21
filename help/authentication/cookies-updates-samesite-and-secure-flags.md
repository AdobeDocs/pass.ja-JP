---
title: Cookie の更新 — SameSite および Secure フラグ
description: Cookie の更新 — SameSite および Secure フラグ
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Cookie の更新 — SameSite および Secure フラグ {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>


## 更新 {#Updates}

この節では、サードパーティ Cookie を処理するために Chrome ブラウザーとAdobe Pass Authentication で導入された変更について説明します。



### Chrome 80 アップデート {#Chrome}

Chrome バージョン 80（バージョン 82 を除く）以降、 *SameSite* 属性は、 *SameSite=Lax*. したがって、クロスサイトコンテキストで配信する必要がある cookie では、 *SameSite=None*&#x200B;また、 *セキュア* 属性と配信される *HTTPS*. これらの更新に関する詳細は、Chromium の公式ページを参照してください。 <https://www.chromium.org/updates/same-site> また、 <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass認証の更新 {#Pass-Updates}

Adobe Pass認証サービスは、現在、Adobe Pass認証 SDK の一部のプラットフォームおよびバージョンと組み合わせて機能するため、ブラウザーの観点から見ればサードパーティ cookie と見なされる cookie（Chrome を含む）に依存しています。 したがって、今後の変更に準拠し、これらの古い SDK からクロスサイトコンテキストでこれらの cookie を引き続き配信するために、Adobe Pass Authentication サービスは、 *adobe-pass-2.55.1* バージョン。

これらの変更は、 *adobe-pass-2.55.1* バージョンに *セキュア* および *SameSite=None* バージョン 80 以降（バージョン 82 を除く）の Chrome ブラウザーを使用している場合に、すべてのAdobe Pass Authentication SDK に渡されるすべての cookie の属性。

次の節では、1 人のユーザーが Chrome ブラウザー 80 以降（バージョン 82 を除く）を使用している場合に発生する可能性のある、プラットフォームとAdobe Pass Authentication SDK のバージョンのリストに関する問題を説明します。

## トラブルシューティング {#Troubleshooting}

この節を参照する際は、すべてのAdobe Pass Authentication Service の Cookie に *セキュア* 属性セット *adobe-pass-2.55.1* すべてのブラウザーのバージョン。 *SameSite=None* 属性は、Chrome ブラウザーのバージョン 80 以降（バージョン 82 を除く）に対してのみ設定する必要があります。


### 一般的なトラブルシューティング {#General}

1. 一部のユーザーエージェントは *SameSite=None* 属性。

   - Chrome 51 から Chrome 66（両端を含む）までの Chrome のバージョン。 これらの Chrome バージョンでは、 *SameSite=None*. これは、Chromium 派生ブラウザーの古いバージョンや Android WebView にも影響します。 この動作は、当時の cookie 仕様のバージョンに応じて正しくなりましたが、仕様に新しい「なし」の値が追加されると、Chrome 67 以降で更新されました。 (Chrome 51 以前は、 SameSite 属性は完全に無視され、すべての cookie が次のように扱われていました。 *SameSite=None*.)
   - Android 版の UC Browser のバージョン ( バージョン12.13.2より前 )。古いバージョンでは、 *SameSite=None*. この動作は、当時の cookie 仕様のバージョンに応じて正しく行われましたが、仕様に新しい「なし」の値が追加されると、UC Browser の新しいバージョンでこの動作が更新されました。
   - macOS 10.14 での Safari および埋め込みブラウザーのバージョンと、iOS 12 でのすべてのブラウザー。 これらのバージョンでは、 *SameSite=None* まるで彼らが印を付けたかのように *SameSite=Strict*. このバグは、iOSとMacOSの新しいバージョンで修正されました。


1. 重要： cookie には *セキュア* 属性は、 *HTTPS*&#x200B;を含めない場合、cookie はAdobe Pass Authentication サービスに到達しません。

   - AccessEnabler JavaScript SDK:
      - との通信が必須 *sp.auth.adobe.com* uses *HTTPS* バージョンの *2.35* および *3.5.0*&#x200B;を追加してから、Dynamic Client の登録を導入する必要があります。
   - AccessEnabler iOS/tvOS SDK:
      - との通信が必須 *sp.auth.adobe.com* uses *HTTPS* （以前のバージョン） *3.0.0*&#x200B;を追加してから、Dynamic Client の登録を導入する必要があります。
   - AccessEnabler Android SDK:
      - との通信が必須 *sp.auth.adobe.com* uses *HTTPS* （以前のバージョン） *3.0.0*&#x200B;を追加してから、Dynamic Client の登録を導入する必要があります。
   - AccessEnabler FireOS SDK:
      - との通信が必須 *sp.auth.adobe.com* uses *HTTPS* バージョンの *2.0.4*.

</br>

### AccessEnabler JavaScript SDK バージョン 2.35 のトラブルシューティング {#235-Troubleshooting}

ユーザーの認証フローは、Chrome 80 以降（バージョン 82 を除く）で影響を受ける可能性があります。 上記の更新のため、ユーザーの認証に問題がないことを確認するには、次の操作を行います。

- 以下を確認します。 *JSESSIONID* cookie がブラウザーに設定され、 *SameSite=None* および *セキュア* 属性セット。
- 以下を確認します。 *JSESSIONID* からの cookie *https://sp.auth.adobe.com/authenticate/saml* network request matches *JSESSIONID* からの cookie *https://sp.auth.adobe.com/session* ネットワークリクエスト。


### AccessEnabler JavaScript SDK バージョン 3.5.0 のトラブルシューティング {#350-Troubleshooting}

ユーザーの認証フローは、Chrome 80 以降（バージョン 82 を除く）で影響を受ける可能性があります。 上記の更新のため、ユーザーの認証に問題がないことを確認するには、次の操作を行います。

- 以下を確認します。 *JSESSIONID* cookie がブラウザーに設定され、 *SameSite=None* および *セキュア* 属性セット。
- 以下を確認します。 *JSESSIONID* からの cookie *https://sp.auth.adobe.com/authenticate/saml* network request matches *JSESSIONID* からの cookie *https://sp.auth.adobe.com/session* ネットワークリクエスト。
- 以下を確認します。 *pass\_sfp* cookie がブラウザーに設定され、 *SameSite=None* および *セキュア* 属性セット。
- 以下を確認します。 *pass\_sfp* Cookie が *https://sp.auth.adobe.com/session* ネットワークリクエスト。


ユーザーの認証フローは、Chrome 80 以降（バージョン 82 を除く）で影響を受ける可能性があります。 上記の更新により、ユーザーが保護されたリソースを正常に認証後に監視する際に問題が発生しないようにするには、次の手順を実行します。

- 以下を確認します。 *pass\_sfp* cookie がブラウザーに設定され、 *SameSite=None* および *セキュア* 属性セット。
- 以下を確認します。 *pass\_sfp* Cookie が *https://sp.auth.adobe.com/adobe-services/authorize* ネットワークリクエスト。
- 以下を確認します。 *pass\_sfp* Cookie が *https://sp.auth.adobe.com/adobe-services/shortAuthorize* ネットワークリクエスト。
