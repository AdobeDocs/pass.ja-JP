---
title: Apple SSO の概要
description: Apple SSO の概要
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Apple SSO の概要 {#apple-sso-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#Introduction}

Appleは、ユーザーがデバイスシステムレベルで TV プロバイダーアカウントにログインできる API を提供し、アプリ単位の認証を不要にします。

したがって、AppleとAdobe Pass Authentication は提携し、iPhone、iPad、Appleの各テレビ所有者を対象に、TV Everywhere エコシステムでプラットフォームのシングルサインオン（SSO）ユーザーエクスペリエンスを構築しました。

Apple デバイスでシングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、完了する必要のある前提条件のリストがあります。

</br>

## 前提条件 {#Prerequisites}

前提条件は、プログラマー、MVPD、Adobe Pass認証、Appleなど、TVE ビジネスに関与する 1 つまたは複数のエンティティに適用される場合があります。

</br>

### プログラマ {#Programmer}

シングルサインオン（SSO）ユーザーエクスペリエンスを活用するには、1 人のプログラマーが次の操作を行う必要があります。

1. Xcode バージョン 8 およびiOS/tvOS バージョン 10 以降を使用してください。

1. Apple開発者アカウントに [ ビデオ購読者のシングルサインオン権限 ](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) が設定されている。 Appleに連絡して、Apple チーム ID の [ ビデオ購読者のアカウントフレームワーク ](https://developer.apple.com/documentation/videosubscriberaccount) を有効にしてください。

1. [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) を使用して、目的の統合（Channel x MVPD）および目的のプラットフォーム（iOS/tvOS）ごとにシングルサインオン（はい）を有効にします。

1. Apple Authentication team が提供する次の 2 つのソリューションのいずれかを使用して、Adobe Pass SSO ワークフローを統合します。

   - Adobe Pass認証 REST API は、iOS、iPadOS または tvOS で動作するクライアントアプリケーションのエンドユーザーに対して、Platform シングルサインオン（SSO）認証をサポートできます。 [Apple SSO クックブック（REST API） ](/help/authentication/apple-sso-cookbook-rest-api.md) も参照してください。

   - Adobe Pass Authentication AccessEnabler iOS/tvOS SDK は、iOS、iPadOS、tvOS で動作するクライアントアプリケーションのエンドユーザーに対して、Platform Single Sign-On （SSO）認証をサポートします。 [Apple SSO クックブック（iOS/tvOS SDK） ](/help/authentication/apple-sso-cookbook-iostvos-sdk.md) も参照してください。

   - **<u>プロのヒント：</u>** ユーザーの購読情報にアクセスするには、ユーザーは、デバイスのカメラまたはマイクへのアクセスを提供するのと同様に、続行する権限をアプリケーションに与える必要があります。 この権限はアプリケーションごとにリクエストされる必要があり、デバイスはユーザーの選択を保存します。 お客様は、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションにアクセスして、アプリの設定（TV プロバイダーの権限へのアクセス）を変更することができます。

   - **<u>ヒント：</u>** アプリケーションがフォアグラウンド状態に入ったときにユーザーの許可を要求することをお勧めしますが、アプリケーションはユーザー認証を要求する前にいつでもユーザーの購読情報の [ アクセス許可 ](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) を確認できるので、これは提案にすぎません。 また、AccessEnabler iOS/tvOS SDK API は、必要に応じて自動的にユーザーのアクセス権をリクエストします。

   - **<u>ヒント：</u>** シングルサインオン（SSO）ユーザーエクスペリエンスの利点を説明することで、購読情報へのアクセス許可を拒否するユーザーにインセンティブを与えることをお勧めします。 お客様は、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションにアクセスして、アプリの設定（TV プロバイダーの権限へのアクセス）を変更することができます。

その結果、次のユーザーフローに沿ったエクスペリエンスが作成されます。アプリケーションの開発を開始する前に、これを参照することをお勧めします。

- [iPhone/iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) のユーザーフロー
- [Apple TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) ユーザーフロー


>[!IMPORTANT]
>
> iOS/tvOS のシングルサインオン機能が **有効** の場合 **および** Apple **オンボード（サポート）またはピッカーの場合** MVPD のApple SSO ワークフローからの認証/ログアウトフローには、AppleとAdobe Passの両方の認証ソリューションが含まれ、その他のすべてのフロー（認証、事前認証、メタデータなど）が含まれます は、Adobe Pass Authentication によってのみ処理されます。


>[!IMPORTANT]
>
> iOS/tvOS のシングルサインオン機能が **無効** の場合 **または** Apple **オンボードされていない（サポートされていない）場合）** MVPD では、認証/ログアウトフローは、Apple SSO ワークフローから、Adobe Pass Authentication のみで処理される通常のワークフローにフォールバックします。


>[!IMPORTANT]
>
> Apple SSO ワークフローの主なメリットの 1 つは、one screen authentication user flow です。このフローは、tvOS のシングルサインオン機能が **有効** の場合や、Appleがオンボード **サポート）されている場合****Apple TV で配信でき** す。


### MVPD {#MVPD}

シングルサインオン（SSO）のユーザーエクスペリエンスを活用するには、次の手順を実行します
MVPD には次の条件があります。



1. Apple側でApple SSO ワークフローにオンボーディングされる。 オンボーディングプロセスを容易にするには、Appleにお問い合わせください。
1. ユーザーログインフォームを処理できるJavaScript TVML アプリケーションを提供します。 適切なドキュメントを受け取るには、Appleにお問い合わせください。
1. オンボーディングプロセス中にAppleによって割り当てられたプロバイダー ID を表す文字列値を指定します。 設定の変更を行うには、Adobe Pass Authentication にお問い合わせください。

</br>

## FAQ {#FAQ}

1. Appleの SSO ワークフローで問題が発生した場合、AccessEnabler iOS/tvOS SDK を使用しているアプリケーションは通常の認証フローにフォールバックできますか？
   - これは可能ですが、[Adobe Pass TVE ダッシュボード ](https://experience.adobe.com/#/pass/authentication) で設定変更を行う必要があります。 目的の統合（Channel x MVPD）と目的のプラットフォーム（iOS/tvOS）では、*シングルサインオンを有効にする* を *いいえ* に設定する必要があります。
   - AccessEnabler iOS/tvOS SDK を使用している場合、アプリケーションは [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API を呼び出した後にのみ、設定の変更を認識します。
1. 別のデバイスまたは別のアプリケーションでの Platform SSO を介したログインの結果として認証が発生したタイミングをアプリケーションに通知しますか？
   - この情報は利用できません。
1. 同じデバイス上の Platform SSO を介したログインの結果として認証が発生したタイミングをアプリケーションに通知しますか？
   - この情報は、ユーザーメタデータキー *tokenSource* の一部として利用でき、文字列値（この場合は「Apple」）を返す必要があります。
1. アプリケーションに統合されていない MVPD を使用して、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションに移動してログインするとどうなりますか？
   - ユーザーがアプリケーションを起動しても、Apple SSO ワークフロー経由でユーザーが認証されることはありません。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。
1. iOS/tvOS プラットフォームの [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) で *Enable Single Sign-On* が *NO* に設定されている MVPD を使用して、iOS/iPadOS の *`Settings -> TV Provider`* または tvOS の *`Settings -> Accounts -> TV Provider`* のセクションに移動してログインするとどうなりますか？
   - ユーザーがアプリケーションを起動しても、Apple SSO ワークフロー経由でユーザーが認証されることはありません。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。
1. Appleでオンボードされていない（サポートされていない） MVPD がApple ピッカーに表示されている場合はどうなりますか。
   - ユーザーがアプリケーションを起動すると、ユーザーは、認証フローを完了せずに、Apple SSO ワークフローを介してのみ MVPD を選択します。 したがって、アプリケーションは通常の認証フローにフォールバックする必要がありますが、既に選択されている MVPD を使用することもできます。
1. Appleでオンボードされていない（サポートされていない） MVPD がある場合はどうなりますか？
   - Appleの SSO ワークフローを使って「その他の TV プロバイダー」のピッカーオプションを選択します。 したがって、アプリケーションは通常の認証フローにフォールバックし、独自の MVPD ピッカーを提示する必要があります。
1. [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) を介して劣化する MVPD がある場合、どうなりますか？
   - ユーザーがアプリケーションを起動すると、ユーザーは、Apple SSO ワークフローではなく、低下メカニズムを介して認証されます。
   - AccessEnabler iOS/tvOS SDK を使用している場合、アプリケーションは *N010* 警告コードを通じて通知されます。
1. MVPD ユーザー ID は、Apple SSO と非Apple SSO の間で変わりますか？
   - ユーザー ID は変更されないことが想定されますが、選択したプロバイダーごとに確認する必要があります。
1. 認証 TTL に何か変更はありますか？
   - Adobe Pass認証では、各 MVPD との統合にプログラマーが必要とする TTL を引き続き考慮します。
   - Apple SSO を使用して、あるプログラマーアプリケーションから別のプログラマーアプリケーションに移動する場合、2 番目のアプリケーションは、対応するプログラマー x MVPD 統合の TTL を持ちます（認証する最初のアプリケーションの TTL は共有されません）

|                                      | Adobe Pass認証 TTL の有効期限が切れました | Adobe Pass認証の TTL が有効です |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Apple デバイストークン TTL の有効期限が切れました** | ユーザーが認証されません（MVPD ピッカーが表示されます） | ユーザーが認証され、TTL はAdobe Pass認証トークンの残り時間です |
| **Apple デバイストークン TTL が有効です** | ユーザーはサイレントに認証され、TVE Dashboard で指定された TTL を持つ別のAdobe Pass認証トークンを取得します | ユーザーが認証され、TTL はAdobe Pass認証トークンの残り時間です |

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
