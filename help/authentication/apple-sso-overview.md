---
title: Apple SSO の概要
description: Apple SSO の概要
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Apple SSO の概要 {#apple-sso-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#Introduction}

Appleは、ユーザーがデバイスのシステムレベルで TV プロバイダーアカウントにログインできる API を提供しており、アプリごとに認証する必要はありません。

したがって、AppleとAdobe Passの認証は、iPhone、iPad、Appleの各 TV 所有者向けの TV Everywhere エコシステムで、プラットフォームのシングルサインオン (SSO) ユーザーエクスペリエンスを作成するために提携しました。

Appleデバイスでのシングルサインオン (SSO) ユーザーエクスペリエンスを活用するには、前提条件のリストが用意されています。このリストは必須です。

</br>

## 前提条件 {#Prerequisites}

前提条件は、TVE ビジネスに関係する 1 つ以上のエンティティ (Programmers、MVPDs、Adobe Pass Authentication、Appleなど ) に適用される場合があります。

</br>

### プログラマー {#Programmer}

シングルサインオン (SSO) のユーザーエクスペリエンスを活用するには、1 人のプログラマーが次の操作を行う必要があります。

1. Xcode バージョン 8 およびiOS/tvOS バージョン 10 以降を使用してください。

1. 以下をおこなう [ビデオ購読者のシングルサインオンの使用権限](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) をApple Developer Account に設定する必要があります。 有効にするには、Appleにお問い合わせください [ビデオ購読者のアカウントフレームワーク](https://developer.apple.com/documentation/videosubscriberaccount) をApple Team ID に追加します。

1. を通じて、必要な統合 (Channel x MVPD) および目的のプラットフォーム (iOS/tvOS) ごとにシングルサインオン (YES) を有効にする [Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/).

1. Adobe Pass認証チームが提供する次の 2 つのソリューションのいずれかを使用して、Apple SSO ワークフローを統合します。

   - Adobe Pass Authentication REST API は、iOS、iPadOS または tvOS で実行されているクライアントアプリケーションのエンドユーザーに対して、プラットフォームのシングルサインオン (SSO) 認証をサポートできます。 詳しくは、 [Apple SSO クックブック (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - Adobe Pass Authentication AccessEnabler iOS/tvOS SDK は、iOS、iPadOS、tvOS で実行されるクライアントアプリケーションのエンドユーザーに対する、プラットフォームのシングルサインオン (SSO) 認証をサポートします。 詳しくは、 [Apple SSO クックブック (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>ヒント：</u>** ユーザーのサブスクリプション情報にアクセスするには、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、ユーザーはアプリケーションに続行の権限を付与する必要があります。 この権限は、アプリケーションごとにリクエストする必要があります。デバイスは、ユーザーの選択内容を保存します。 ユーザーは、アプリケーション設定（TV Provider 権限のアクセス）または次のセクションに移動して、決定を変更できます。 *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* tvOS の場合。

   - **<u>ヒント：</u>** アプリケーションがフォアグラウンド状態になった場合は、ユーザーの権限をリクエストすることをお勧めしますが、アプリケーションはをチェックできるので、これは単なる提案です。 [アクセス権](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) ユーザー認証を必要とする前の任意の時点でのユーザーの購読情報。 また、AccessEnabler iOS/tvOS SDK API は、必要に応じて、ユーザーの権限を自動的に要求します。

   - **<u>ヒント：</u>** シングルサインオン (SSO) ユーザーエクスペリエンスの利点を説明し、サブスクリプション情報へのアクセス権の付与を拒否するユーザーに対しては、その利点を説明することをお勧めします。 ユーザーは、アプリケーション設定（TV Provider 権限のアクセス）または次のセクションに移動して、決定を変更できます。 *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* tvOS の場合。

その結果、次のユーザーフローに従ってエクスペリエンスが作成されます。アプリケーションの開発を開始する前に、これを確認することをお勧めします。

- [IPHONE/IPAD](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) ユーザーフロー
- [Apple TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) ユーザーフロー


>[!IMPORTANT]
>
> シングルサインオン機能が **有効** iOS/tvOS の場合 **および** Appleの場合は **オンボード済み（サポート）またはピッカー** Apple SSO ワークフローからの認証/ログアウトフローは、AppleとAdobe Passの両方の認証ソリューションに影響しますが、他のすべてのフロー（認証、事前認証、メタデータなど）は、 は、Adobe Pass Authentication によってのみサービスされます。


>[!IMPORTANT]
>
> シングルサインオン機能が **無効** iOS/tvOS の場合 **または** Appleの場合は **オンボーディングされていない（サポート対象外）** 認証/ログアウトフローの MVPD は、Apple SSO ワークフローから、Adobe Pass Authentication のみで提供される通常のワークフローにフォールバックします。


>[!IMPORTANT]
>
> Apple SSO ワークフローの主な利点の 1 つは、1 画面認証のユーザーフローで表されます。これは、シングルサインオン機能が使用されている場合にApple TV で配信することもできます **有効** tvOS の場合 **および** Appleの場合は **オンボード（サポート）** MVPDs。


### MVPD {#MVPD}

シングルサインオン (SSO) のユーザーエクスペリエンスを活用するには、1 つの MVPD が次の条件を満たす必要があります。



1. Apple側のApple SSO ワークフローにオンボーディングする。 オンボーディングプロセスを容易にするには、Appleにお問い合わせください。
1. ユーザーログインフォームを処理できる JavaScript TVML アプリケーションを提供します。 適切なドキュメントを受け取るには、Appleにお問い合わせください。
1. オンボーディングプロセス中にAppleによって割り当てられたプロバイダー識別子を表す string 値を指定します。 設定の変更を実行するには、Adobe Pass認証に問い合わせてください。

</br>

## FAQ {#FAQ}

1. Apple SSO ワークフローで問題が発生した場合、AccessEnabler iOS/tvOS SDK を使用するアプリケーションで通常の認証フローにフォールバックできるか。
   - これは可能ですが、 [Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/). The *シングルサインオンを有効にする* は、に設定されている必要があります *いいえ* 目的の統合 (Channel x MVPD) および目的のプラットフォーム (iOS/tvOS) 用。
   - アプリケーションは、を呼び出した後にのみ、設定の変更を確認します。 [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) AccessEnabler iOS/tvOS SDK を使用している場合の API。
1. 別のデバイスまたは別のアプリケーションで、プラットフォーム SSO を介したログインが原因で認証が発生した場合に、アプリケーションはその時点を認識しますか？
   - この情報は利用できません。
1. 同じデバイス上のプラットフォーム SSO を介したログインの結果、アプリケーションはいつ認証が発生したかを把握しますか。
   - この情報は、ユーザーメタデータキーの一部として使用できます。 *tokenSource*&#x200B;を呼び出し、この場合、文字列値「Apple」を返します。
1. ユーザーが *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* アプリケーションと統合されていない MVPD を使用する tvOS セクションで、
   - ユーザーがアプリケーションを起動しても、そのユーザーはApple SSO ワークフローで認証されません。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。
1. ユーザーが *`Settings -> TV Provider`* iOS/iPadOS または *`Settings -> Accounts -> TV Provider`* tvOS セクションで、 *シングルサインオンを有効にする* ～に取り掛かる *いいえ* の [Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/) iOS/tvOS プラットフォームの場合
   - ユーザーがアプリケーションを起動しても、そのユーザーはApple SSO ワークフローで認証されません。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。
1. Appleがオンボーディング（サポート対象外）していない (Appleピッカーに存在する )MVPD がユーザーにある場合はどうなりますか？
   - ユーザーがアプリケーションを起動すると、ユーザーはApple SSO ワークフロー経由でのみ MVPD を選択し、認証フローを完了しません。 したがって、アプリケーションは通常の認証フローにフォールバックする必要がありますが、既に選択されている MVPD を使用する可能性があります。
1. Appleがオンボーディング（サポート対象外）していない MVPD がユーザーに含まれている場合はどうなりますか？
   - ユーザーがアプリケーションを起動すると、Apple SSO ワークフローで「その他の TV プロバイダー」ピッカーオプションが選択されます。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。
1. ユーザーが MVPD を持ち、MVPD が [Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/)?
   - ユーザーがアプリケーションを起動すると、ユーザーの認証は、Apple SSO ワークフローではなく、劣化メカニズムを使用しておこなわれます。
   - アプリケーションは、 *N010* AccessEnabler iOS/tvOS SDK を使用している場合の警告コード。
1. MVPD ユーザー ID は、Apple SSO と非Apple SSO 認証フローの間で変更されますか？
   - ユーザー ID は変更されないことが予想されますが、選択したプロバイダーごとに検証する必要があります。
1. 認証 TTL に変更はありますか。
   - Adobe Pass認証は、各 MVPD との統合に関してプログラマーが必要とする TTL を引き続き尊重します。
   - あるプログラマーアプリケーションから別のプログラマーアプリケーションにApple SSO を通じて移動する場合、2 つ目のアプリケーションは対応する Programmer x MVPD 統合の TTL を持ちます（認証する最初のアプリケーションの TTL は共有されません）。

|                                      | Adobe Pass Authentication TTL 期限切れ | Adobe Pass認証 TTL が有効です |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Appleデバイストークンの TTL が期限切れです** | ユーザーが認証されていません（MVPD ピッカーが表示されます） | ユーザーが認証済みで、TTL はAdobe Pass認証トークンの残り時間です。 |
| **Appleデバイストークンの TTL が有効です** | ユーザーは通知なしで認証され、TVE ダッシュボードで指定された TTL を使用して別のAdobe Pass認証トークンを取得します。 | ユーザーが認証済みで、TTL はAdobe Pass認証トークンの残り時間です。 |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
