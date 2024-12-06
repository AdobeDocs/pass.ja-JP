---
title: Apple SSO の概要
description: Apple SSO の概要
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1256'
ht-degree: 0%

---

# Apple SSO の概要 {#apple-sso-overview}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

Appleを使用すると、デバイスシステムレベルで TV プロバイダーアカウントにログインできるので、アプリごとに認証する必要がなくなります。

Adobe Pass Authentication はAppleと提携し、iPhone、iPadおよびApple TV のオーナー向けに、TV Everywhere エコシステムでのパートナーシングルサインオン（SSO）ユーザーエクスペリエンスを構築しました。

Apple デバイスでシングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、以下に記載する前提条件を満たす必要があります。

最終的には、次のユーザーフローに沿ったエクスペリエンスが作成されます。アプリケーションの開発を開始する前に、を参照することをお勧めします。

* シングルサインオン（SSO） [iPhoneおよびiPadのユーザーフロー ](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) デバイス。
* シングルサインオン（SSO） [Apple TV のユーザーフロー ](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) デバイス。

## 前提条件 {#apple-sso-prerequisites}

オンボーディングの前提条件は、プログラマー、MVPD、Adobe Pass認証、Appleなど、TVE ビジネスに関与する 1 つまたは複数のエンティティに当てはまる場合があります。

### プログラマ {#apple-sso-prerequisites-programmer}

シングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、1 人のプログラマーが次の操作を行う必要があります。

* Appleに連絡して、[ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) をApple チーム ID の一部として有効にし、[ ビデオ購読者のシングルサインオン権限 ](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) をApple開発者アカウントの一部として設定してください。

   * Xcode バージョン 8 以降およびiOS/tvOS バージョン 10 以降を使用してください。

* `Enable Single Sign On` プロパティを `Yes` に設定して、[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) から目的の統合およびプラットフォーム（iOS/tvOS）ごとにシングルサインオン（SSO）を有効にします。

| Adobeによるシングルサインオンの有効化 | Apple **オンボード（サポート）** MVPD | Apple **ピッカー** MVPD | Apple **オンボードされない（サポートされない）** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| はい（有効） | 認証フローとログアウトフローには、AppleとAdobe Pass認証ソリューションの両方が含まれますが、その他のすべてのフロー（認証、事前認証、メタデータなど）は、Adobe Pass認証のみで処理されます。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 |
| いいえ（無効） | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 |

* iOS、iPadOS、tvOS のいずれかで動作するクライアントアプリケーションのエンドユーザーに対して、Adobe Pass Authentication が提供する次のいずれかのソリューションを使用して、シングルサインオン（SSO）ユーザーフローを統合します。

   * Adobe Pass認証 REST API V2 は、パートナーシングルサインオン（SSO）をサポートしています。

     [Apple SSO クックブック（REST API V2） ](apple-sso-cookbook-rest-api-v2.md) のドキュメントを参照してください。

   * Adobe Pass認証 REST API V1 は、パートナーシングルサインオン（SSO）をサポートしています。

     [Apple SSO クックブック（REST API V1） ](apple-sso-cookbook-rest-api-v1.md) ドキュメントを参照してください。

   * Adobe Pass Authentication AccessEnabler iOS/tvOS SDK は、パートナーシングルサインオン（SSO）をサポートしています。

     [Apple SSO クックブック（iOS/tvOS SDK） ](apple-sso-cookbook-iostvos-sdk.md) のドキュメントを参照してください。

### MVPD {#apple-sso-prerequisites-mvpd}

シングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、1 つの MVPD が次の要件を満たす必要があります。

* Appleに連絡して、Apple側でのオンボーディングプロセスを開始してください。

   * ユーザーログインフォームを処理できるJavaScript TVML アプリケーションを統合および開発する方法に関する技術ドキュメントをリクエストします。

* Adobe Pass認証に連絡して、Adobe側のオンボーディングプロセスを開始します。

   * オンボーディングプロセス中にAppleによって割り当てられた TV プロバイダーの ID を表す文字列値を指定します。

## FAQ {#FAQ}

* Appleの SSO ワークフローで問題が発生した場合、Adobe Pass Authentication AccessEnabler iOS/tvOS SDK を使用しているアプリケーションで通常の認証フローにフォールバックできますか？

  可能ですが、ご利用の統合およびプラットフォーム（iOS/tvOS](https://experience.adobe.com/#/pass/authentication) に応じて「**シングルサインオンを有効にする**」を「いいえ **に設定するため、[Adobe Pass TVE ダッシュボード** で設定変更を行う必要があります。 クライアントアプリケーションは、[setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) API を呼び出した後にのみ、設定の変更を確認することに注意してください。


* Apple SSO を使用したログインの結果として認証が発生したタイミングをアプリケーションに知らせますか？

  この情報は、ユーザーメタデータキー *tokenSource* の一部として利用でき、文字列値（この場合は「Apple」）を返す必要があります。


* 別のアプリケーションでApple SSO を使用してログインした結果、いつ認証が発生したかを知ることができますか？

  この情報は利用できません。


* アプリケーションに統合されていない MVPD を使用して、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションに移動してログインするとどうなりますか？

  ユーザーがアプリケーションを起動しても、Apple SSO ワークフロー経由でユーザーが認証されることはありません。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。


* iOS/tvOS プラットフォームの **6}Adobe Pass TVE Dashboard** を使用して、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* に移動し、**Enable Single Sign On](https://experience.adobe.com/#/pass/authentication) が {NO** に設定されている MVPD を使用してログインすると、どうなりますか？[

  ユーザーがアプリケーションを起動しても、Apple SSO ワークフロー経由でユーザーが認証されることはありません。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。


* Appleでオンボードされていない（サポートされていない） MVPD がApple ピッカーに表示されている場合はどうなりますか。

  ユーザーがアプリケーションを起動すると、ユーザーは、認証フローを完了せずに、Apple SSO ワークフローを介してのみ MVPD を選択します。 したがって、アプリケーションは通常の認証フローにフォールバックする必要がありますが、既に選択されている MVPD を使用することもできます。


* Appleでオンボードされていない（サポートされていない） MVPD がある場合はどうなりますか？

  Appleの SSO ワークフローを使って「その他の TV プロバイダー」のピッカーオプションを選択します。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。


* [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) を介して劣化する MVPD がある場合、どうなりますか？

  ユーザーがアプリケーションを起動すると、ユーザーは、Apple SSO ワークフローではなく、低下メカニズムを介して認証されます。 Adobe Pass Authentication AccessEnabler iOS/tvOS SDK を使用している場合は、*N010* の警告コードを通じてアプリケーションに通知されますが、エクスペリエンスはシームレスである必要があります。


* MVPD ユーザー ID は、Apple SSO とApple以外の SSO 認証フローの間で変わりますか？

  ユーザー ID は変更されないことが想定されますが、選択したプロバイダーごとに確認する必要があります。


* 認証 TTL に何か変更はありますか？

  Adobe Pass認証では、各 MVPD との統合にプログラマーが必要とする TTL を引き続き考慮します。 Apple SSO を使用して、あるプログラマーアプリケーションから別のプログラマーアプリケーションに移動する場合、2 番目のアプリケーションは、対応するプログラマー x MVPD 統合の TTL を持ちます（認証する最初のアプリケーションの TTL は共有されません）

|                                      | Adobe Pass認証 TTL の有効期限が切れました | Adobe Pass認証の TTL が有効です |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Apple デバイストークン TTL の有効期限が切れました** | ユーザーが認証されません（MVPD ピッカーが表示されます） | ユーザーが認証され、TTL はこのAdobe Pass認証トークン/プロファイルの残り時間です |
| **Apple デバイストークン TTL が有効です** | ユーザーはサイレントに認証され、TVE Dashboard で指定された TTL を持つ別のAdobe Pass認証トークン/プロファイルを取得します | ユーザーが認証され、TTL はこのAdobe Pass認証トークン/プロファイルの残り時間です |
