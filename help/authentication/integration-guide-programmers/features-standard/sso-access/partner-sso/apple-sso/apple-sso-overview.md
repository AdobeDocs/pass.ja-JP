---
title: Apple SSO の概要
description: Apple SSO の概要
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 3%

---

# Apple SSO の概要 {#apple-sso-overview}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

Appleを使用すると、デバイスシステムレベルで TV プロバイダーアカウントにログインできるので、アプリごとに認証する必要がなくなります。

Adobe Pass Authentication はAppleと提携し、iPhone、iPadおよびApple TV のオーナー向けに、TV Everywhere エコシステムでのパートナーシングルサインオン（SSO）ユーザーエクスペリエンスを構築しました。

Apple デバイスでシングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、以下に記載する前提条件を満たす必要があります。

最終的には、次のユーザーフローに沿ったエクスペリエンスが作成されます。アプリケーションの開発を開始する前に、を参照することをお勧めします。

* シングルサインオン（SSO） [iPhoneおよびiPadのユーザーフロー &#x200B;](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) デバイス。
* シングルサインオン（SSO） [Apple TV のユーザーフロー &#x200B;](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) デバイス。

## 前提条件 {#apple-sso-prerequisites}

オンボーディングの前提条件は、プログラマー、MVPD、Adobe Pass認証、Appleなど、TVE ビジネスに関与する 1 つまたは複数のエンティティに当てはまる場合があります。

### プログラマ {#apple-sso-prerequisites-programmer}

シングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、1 人のプログラマーが次の操作を行う必要があります。

* Appleに連絡して、[&#x200B; ビデオ購読者のアカウントフレームワーク &#x200B;](https://developer.apple.com/documentation/videosubscriberaccount) をApple チーム ID の一部として有効にし、[&#x200B; ビデオ購読者のシングルサインオン権限 &#x200B;](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) をApple開発者アカウントの一部として設定してください。

   * Xcode バージョン 8 以降およびiOS/tvOS バージョン 10 以降を使用してください。

* `Enable Single Sign On` プロパティを `Yes` に設定して、[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) から目的の統合およびプラットフォーム（iOS/tvOS）ごとにシングルサインオン（SSO）を有効にします。

| Adobeによるシングルサインオンの有効化 | Apple **オンボード（サポート）** MVPD | Apple **ピッカー** MVPD | Apple **オンボードされない（サポートされない）** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| はい（有効） | 認証フローとログアウトフローには、AppleとAdobe Pass認証ソリューションの両方が含まれますが、その他のすべてのフロー（認証、事前認証、メタデータなど）は含まれます。 は、Adobe Pass Authentication によってのみ処理されます。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 |
| いいえ（無効） | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 | 認証フローとログアウトフローは、Adobe Pass認証のみが提供する通常のフローにフォールバックします。 |

* iOS、iPadOS、tvOS のいずれかで動作するクライアントアプリケーションのエンドユーザーに対して、Adobe Pass Authentication が提供する次のいずれかのソリューションを使用して、シングルサインオン（SSO）ユーザーフローを統合します。

   * Adobe Pass認証 REST API V2 は、パートナーシングルサインオン（SSO）をサポートしています。

     [Apple SSO クックブック（REST API V2） &#x200B;](apple-sso-cookbook-rest-api-v2.md) のドキュメントを参照してください。

   * 従来のAdobe Pass認証 REST API V1 は、パートナーシングルサインオン（SSO）をサポートしています。

     [&#x200B; （従来の）Apple SSO クックブック（REST API V1） &#x200B;](../../../../legacy/sso-access/apple-sso-cookbook-rest-api-v1.md) ドキュメントを参照してください。

   * 従来のAdobe Pass Authentication AccessEnabler iOS/tvOS SDKは、パートナーシングルサインオン（SSO）をサポートしています。

     [&#x200B; （従来の）Apple SSO クックブック（iOS/tvOS SDK） &#x200B;](../../../../legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md) のドキュメントを参照してください。

### MVPD {#apple-sso-prerequisites-mvpd}

シングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、1 つのMVPDで次の操作を行う必要があります。

* Appleに連絡して、Apple側でのオンボーディングプロセスを開始してください。

   * ユーザーログインフォームを処理できるJavaScript TVML アプリケーションを統合および開発する方法に関する技術ドキュメントをリクエストします。

* Adobe Pass認証に連絡して、Adobe側でのオンボーディングプロセスを開始してください。

   * オンボーディングプロセス中にAppleによって割り当てられた TV プロバイダーの ID を表す文字列値を指定します。

## FAQ {#FAQ}

* Appleの SSO ワークフローで問題が発生した場合、Adobe Pass Authentication AccessEnabler iOS/tvOS SDKを使用しているアプリケーションで通常の認証フローにフォールバックできますか？

  可能ですが、ご利用の統合およびプラットフォーム（iOS/tvOS[&#128279;](https://experience.adobe.com/#/pass/authentication) に応じて「**シングルサインオンを有効にする**」を「いいえ **に設定するため、Adobe Pass TVE ダッシュボード** で設定変更を行う必要があります。 クライアントアプリケーションは、[setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) API を呼び出した後にのみ、設定の変更を確認することに注意してください。


* Apple SSO を使用したログインの結果として認証が発生したタイミングをアプリケーションに知らせますか？

  この情報は、ユーザーメタデータキー *tokenSource* の一部として利用でき、文字列値（この場合は「Apple」）を返す必要があります。


* 別のアプリケーションでApple SSO を使用してログインした結果、いつ認証が発生したかを知ることができますか？

  この情報は利用できません。


* アプリケーションに統合されていないMVPDを使用して、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションに移動してログインするとどうなりますか？

  ユーザーがアプリケーションを起動しても、Apple SSO ワークフロー経由でユーザーが認証されることはありません。 そのため、アプリケーションは通常の認証フローにフォールバックし、独自のMVPD ピッカーを表示する必要があります。


* iOS/tvOS プラットフォームの **6&rbrace;Adobe Pass TVE ダッシュボード** を使用して、「シングルサインオンを有効にする」が **いいえ**&#x200B;[&#128279;](https://experience.adobe.com/#/pass/authentication) に設定されているMVPDを使用して、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションに移動してログインすると、どうなりますか？

  ユーザーがアプリケーションを起動しても、Apple SSO ワークフロー経由でユーザーが認証されることはありません。 そのため、アプリケーションは通常の認証フローにフォールバックし、独自のMVPD ピッカーを表示する必要があります。


* Appleでオンボーディングされていない（サポートされていない）MVPDがApple ピッカーには存在する場合、どうなりますか？

  ユーザーがアプリケーションを起動すると、Apple SSO ワークフロー経由でMVPDのみを選択し、認証フローは完了しません。 そのため、アプリケーションは通常の認証フローにフォールバックする必要がありますが、既に選択されているMVPDを使用することもできます。


* Appleでオンボーディングされていない（サポートされていない）MVPDをユーザーが使用している場合はどうなりますか？

  Appleの SSO ワークフローを使って「その他の TV プロバイダー」のピッカーオプションを選択します。 そのため、アプリケーションは通常の認証フローにフォールバックし、独自のMVPD ピッカーを表示する必要があります。


* [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) を介して機能が低下したMVPDがある場合、どうなりますか？

  ユーザーがアプリケーションを起動すると、ユーザーは、Apple SSO ワークフローではなく、低下メカニズムを介して認証されます。 Adobe Pass Authentication AccessEnabler iOS/tvOS SDKを使用している場合は、*N010* の警告コードを通じてアプリケーションに通知されるので、シームレスな操作が可能です。


* MVPD ユーザー ID は、Apple SSO と非Apple SSO 認証フローの間で変わりますか？

  ユーザー ID は変更されないことが想定されますが、選択したプロバイダーごとに確認する必要があります。


* 認証 TTL に何か変更はありますか？

  Adobe Pass認証では、各MVPDとの統合に関してプログラマーが必要とする TTL を引き続き考慮します。 Apple SSO を使用して、あるプログラマーアプリケーションから別のプログラマーアプリケーションに移動する場合、2 番目のアプリケーションは、対応するプログラマー x MVPD統合の TTL を持ちます（認証する最初のアプリケーションの TTL は共有されません）

|                                      | Adobe Pass認証 TTL の有効期限が切れました | Adobe Pass認証の TTL が有効です |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Apple デバイストークン TTL の有効期限が切れました** | ユーザーが認証されません（MVPD ピッカーが表示されます） | ユーザーが認証され、TTL はこのAdobe Pass認証トークン/プロファイルの残り時間です |
| **Apple デバイストークン TTL が有効です** | ユーザーはサイレントに認証され、TVE Dashboard で指定された TTL を持つ別のAdobe Pass認証トークン/プロファイルを取得します | ユーザーが認証され、TTL はこのAdobe Pass認証トークン/プロファイルの残り時間です |
