---
title: Apple SSOの概要
description: Apple SSOの概要
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# Apple SSOの概要 {#apple-sso-overview}

>[!IMPORTANT]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

Appleを使用すると、デバイスのシステムレベルでTV プロバイダーアカウントにログインできるので、アプリごとの認証が不要になります。

Adobe Pass Authenticationは、Appleと提携し、iPhone、iPad、Apple TV オーナー向けに、TV Everywhere エコシステムにおけるパートナーシングルサインオン（SSO）ユーザーエクスペリエンスを構築しました。

Apple デバイスでシングルサインオン（SSO）ユーザーエクスペリエンスを利用するには、次の前提条件のリストを満たす必要があります。

最終的な結果は、次のユーザーフローに沿ったエクスペリエンスを作成する必要があります。アプリケーションの開発を開始する前に参照することをお勧めします。

* IPhoneおよびiPad[&#128279;](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)のデバイスのシングルサインオン （SSO）  ユーザーフロー。
* Apple TV[&#128279;](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) デバイスのシングルサインオン （SSO）  ユーザーフロー。

## 前提条件 {#apple-sso-prerequisites}

オンボーディングの前提条件は、プログラマー、MVPD、Adobe Pass認証、Appleなど、TVE事業に関わる1つまたは複数のエンティティに適用されます。

### プログラマー {#apple-sso-prerequisites-programmer}

シングルサインオン（SSO）ユーザーエクスペリエンスを利用するには、1人のプログラマーが次の条件を満たす必要があります。

* Apple チーム IDの一部として[Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)を有効にし、Apple デベロッパーアカウントの一部として[Video Subscriber Single Sign-On Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on)を設定するには、Appleにお問い合わせください。

  * Xcode バージョン 8以上およびiOS/tvOS バージョン 10以上を使用してください。

* `Enable Single Sign On` プロパティを`Yes`に設定して、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)を介して、目的の統合およびプラットフォーム（iOS/tvOS）ごとにシングルサインオン（SSO）を有効にします。

| Adobe シングルサインオンを有効にする | Apple **オンボーディング済み（サポート済み）** MVPD | Apple **ピッカー** MVPD | Apple **オンボーディングされていません（サポートされていません）** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| はい（有効） | 認証フローとログアウトフローには、AppleとAdobe Passの両方の認証ソリューションが含まれますが、その他のすべてのフロー（認証、事前認証、メタデータなど）も含まれます。 Adobe Pass Authenticationのみがサービスを提供します。 | 認証フローとログアウトフローは、Adobe Pass認証のみでサービスされる通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみでサービスされる通常のフローにフォールバックします。 |
| いいえ（無効） | 認証フローとログアウトフローは、Adobe Pass認証のみでサービスされる通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみでサービスされる通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみでサービスされる通常のフローにフォールバックします。 |

* IOS、iPadOS、またはtvOSで動作するクライアントアプリケーションのエンドユーザーに対して、Adobe Pass Authenticationが提供する次のいずれかのソリューションを使用して、シングルサインオン（SSO）ユーザーフローを統合します。

  * Adobe Pass Authentication REST API V2は、パートナーシングルサインオン（SSO）をサポートしています。

    [Apple SSO クックブック （REST API V2） &#x200B;](apple-sso-cookbook-rest-api-v2.md)のドキュメントを参照してください。

  * 従来のAdobe Pass Authentication REST API V1は、パートナーシングルサインオン（SSO）をサポートしています。

    [&#x200B; （Legacy） Apple SSO Cookbook （REST API V1） &#x200B;](../../../../legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)のドキュメントを参照してください。

  * 従来のAdobe Pass Authentication AccessEnabler iOS/tvOS SDKは、パートナーシングルサインオン（SSO）をサポートしています。

    [&#x200B; （Legacy） Apple SSO Cookbook （iOS/tvOS SDK） &#x200B;](../../../../legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)のドキュメントを参照してください。

### MVPD {#apple-sso-prerequisites-mvpd}

シングルサインオン（SSO）のユーザーエクスペリエンスを利用するには、MVPDで次の操作を行う必要があります。

* Appleに連絡して、Apple側でオンボーディングプロセスを開始してください。

  * ユーザーのログインフォームを処理できるJavaScript TVML アプリケーションの統合および開発方法に関する技術ドキュメントをリクエストします。

* Adobe Pass Authenticationに連絡して、Adobe側でオンボーディングプロセスを開始します。

  * オンボーディングプロセス中にAppleによって割り当てられたTV プロバイダー識別子を表す文字列値を指定します。

## FAQ {#FAQ}

* Apple SSO ワークフローに問題が発生した場合、Adobe Pass Authentication AccessEnabler iOS/tvOS SDKを使用しているアプリケーションは、通常の認証フローにフォールバックできますか？

  これは可能ですが、目的の統合とプラットフォーム（iOS/tvOS）の&#x200B;**NO**&#x200B;で&#x200B;**シングルサインオンを有効にする**&#x200B;を設定するには、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)を通じて設定を変更する必要があります。 クライアントアプリケーションは、[setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) APIを呼び出した後にのみ設定変更を承認することに注意してください。


* Apple SSOを介したログインの結果、認証がいつ行われたのかをアプリケーションに知らせますか？

  この情報は、ユーザーメタデータキー&#x200B;*tokenSource*&#x200B;の一部として利用できます。この場合、文字列値である「Apple」を返す必要があります。


* 別のアプリケーションでApple SSOを使用してログインした結果、認証がいつ発生したかをアプリケーションに知らせますか？

  この情報は利用できません。


* IOS/iPadOSの&#x200B;*`Settings -> TV Provider`*&#x200B;またはtvOSの&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;のセクションに、アプリケーションと統合されていないMVPDを使用してログインした場合はどうなりますか？

  ユーザーがアプリケーションを起動すると、Apple SSO ワークフローを介してユーザーが認証されることはありません。 したがって、アプリケーションは通常の認証フローにフォールバックして、独自のMVPD ピッカーを表示する必要があります。


* IOS/iPadOSの&#x200B;*`Settings -> TV Provider`*&#x200B;またはtvOSの&#x200B;*`Settings -> Accounts -> TV Provider`* セクションにログインし、**NO**&#x200B;に[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)を通じて&#x200B;**シングル サインオンを有効にする**&#x200B;が設定されているMVPDを使用してiOS/tvOS プラットフォームにログインした場合はどうなりますか？

  ユーザーがアプリケーションを起動すると、Apple SSO ワークフローを介してユーザーが認証されることはありません。 したがって、アプリケーションは通常の認証フローにフォールバックして、独自のMVPD ピッカーを表示する必要があります。


* Appleでオンボーディングされていない（サポートされていない）MVPDがApple ピッカーに表示されている場合、どうなりますか？

  ユーザーがアプリケーションを起動すると、認証フローを完了することなく、Apple SSO ワークフロー経由でのみMVPDを選択します。 したがって、アプリケーションは通常の認証フローにフォールバックする必要がありますが、既に選択されているMVPDを使用できます。


* ユーザーがAppleでオンボーディングされていない（サポートされていない）MVPDを持っている場合はどうなりますか？

  ユーザーがアプリケーションを起動すると、Apple SSO ワークフローを介して「その他のTV プロバイダー」ピッカーオプションが選択されます。 したがって、アプリケーションは通常の認証フローにフォールバックして、独自のMVPD ピッカーを表示する必要があります。


* ユーザーが[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)の中で劣化したMVPDを持っている場合はどうなりますか？

  ユーザーがアプリケーションを起動すると、Apple SSO ワークフローではなく、劣化メカニズムを使用してユーザーが認証されます。 エクスペリエンスはユーザーにとってシームレスである必要がありますが、Adobe Pass Authentication AccessEnabler iOS/tvOS SDKを使用している場合は、*N010*&#x200B;の警告コードを通じてアプリケーションに通知されます。


* Apple SSOとMVPD以外のSSO認証フローの間で、Apple ユーザーIDは変更されますか？

  ユーザーIDは変更されませんが、選択した各プロバイダーについて確認する必要があります。


* 認証TTLに変更はありますか？

  Adobe Pass Authenticationは、各MVPDとの統合に必要なプログラマが必要とするTTLを引き続き尊重します。 Apple SSOを介して別のプログラマーアプリケーションにプログラマーアプリケーションから移動する場合、2番目のアプリケーションには、対応するプログラマーx MVPD統合のTTLがあります（認証した最初のアプリケーションのTTLは共有されません）

|                                      | Adobe Pass認証TTLの有効期限が切れました | Adobe Pass Authentication TTL有効 |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Appleのデバイストークン TTLが期限切れになりました** | ユーザーが認証されていません（MVPD ピッカーが表示されます） | ユーザーは認証され、TTLはAdobe Pass認証トークン/プロファイルの残りの時間です |
| **Appleのデバイストークン TTLが有効です** | ユーザーはサイレント認証され、TVE ダッシュボードで指定されたTTLを持つ別のAdobe Pass認証トークン/プロファイルを取得します | ユーザーは認証され、TTLはAdobe Pass認証トークン/プロファイルの残りの時間です |
