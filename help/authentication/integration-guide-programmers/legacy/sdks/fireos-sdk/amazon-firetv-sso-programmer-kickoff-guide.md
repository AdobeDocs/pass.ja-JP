---
title: Amazon fireTV SSO - プログラマーキックオフガイド
description: Amazon fireTV SSO - プログラマーキックオフガイド
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%
---
# （レガシー） Amazon fireTV SSO - プログラマーキックオフガイド {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

## 概要 {#intro}

このドキュメントでは、新しい&#x200B;**Adobe Pass認証のfireTV SDK**&#x200B;をfireTV アプリケーションに統合するために必要な情報について説明します。 この新しいSDKは、AmazonのFireTV プラットフォームでのOS レベルの統合を活用して、**シングルサインオン**&#x200B;のサポートを提供します。 シングルサインオンのメリットを得るには、クライアントレス APIから新しいfireTV SDKにアプリケーションを移行するために、お客様の側から少し労力が必要です。 認証フローには、以下で詳しく説明する変更がいくつかあります。

## 高度なアーキテクチャとOS レベルの統合 {#high}

AmazonのFireTV プラットフォーム上のTV Everywhere アプリケーション間でのシングルサインオンを実現し、このプラットフォームでの全体的な体験を向上させるために、私たちはfireTV OS レベルで当社のコアSDKを統合することを決定しました。 プログラマーは、Adobeが提供するスタブライブラリに対してコンパイルする必要があります。 実際の機能は、AmazonのfireTV OSにあるAdobeのライブラリによって提供されます。

Amazonが私たちのライブラリーをOS レベルで組み込んだfireTV シミュレーターを提供するまでは、開発は実際のfireTV デバイスを使用してのみ可能でした。

## Adobe Workfrontの利点 {#bene}

* 統合されたMVPDを備えたAmazon fireTV プラットフォーム上の、Adobeを活用したTV Everywhere アプリケーション間のシングルサインオンです。
* HBA （サポートされているMVPD）のメリットを享受できます。
* 新しいSDK バージョンがリリースされるたびにアプリケーションを更新することなく、最新のfireTV SDKを使用できます。
* すべてのTVE アプリは、AccessEnabler ライブラリのローカルコピーを持つ必要がなくなるため、共有システムライブラリを使用するメリットを享受できます。 また、すべてのアプリケーションで同じSDK バージョンが使用されます。
* シングルスクリーン認証 – 登録コードと2番目の画面ワークフローは必要ありません。

## クライアントレス API ベースのアプリからfireTV SDK ベースのアプリへの移行 {#migra1}

クライアントレス APIからfireTV SDKに移行する場合は、Clientless APIに関連するコードベースを削除し、新しいfireTV SDKを統合する必要があります。

クライアントレス API ベースのアプリと比較すると、新しいfireTV SDKでは、認証が最初の画面に移動するため、2番目の画面認証は不要になりました。

これは、ユーザーがFireTV デバイスでテレビのプロバイダーを直接選択できるように、プログラマーがアプリにMVPDピッカーを追加する必要があります。 MVPDを選択すると、fireTV デバイスにMVPD ログインページが表示されます。

fireTVの通常、HBA、SSOのシナリオを示すユーザーフローのワイヤーフレームは、[Amazon Fire TV - MVVPD サインインユーザーフロー](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/)にあります。

## Android SDK ベースのアプリからfireTV SDK ベースのアプリへの移行 {#migra2}

この新しいfireTV SDKは、既存のAndroid SDKと非常によく似ており、**Android SDKの統合** <!--http://tve.helpdocsonline.com/android-technical-overview-->用の現在のドキュメントは、fireTV SDK ドキュメントの準備が整うまで使用できます。 Android SDKを使用するAndroid アプリケーションを既にお持ちの場合、fireTV アプリケーションへのfireTV SDKの統合は簡単です。

既存のAndroid SDKと比較すると、fireTV SDKでは、MVPD ログインページの管理/表示とAuthN トークンの取得のタスクがAccessEnabler ライブラリによって内部で実行されるため、認証プロセスが簡単になります。

## FAQ {#faq}

1. **SSO**&#x200B;の仕組みはどのようになりますか？

   * SSOは、同じAmazon fireTV デバイスで新しいfireTV SDKを使用しているAdobe Pass Authenticationを搭載するすべてのプログラマーアプリケーションで機能します
   * クライアントレス REST APIに実装されたプログラマーアプリとfireTV SDK **に実装されたアプリの間のSSOはサポートされません**

1. FireTV SSOのMVPDのカバレッジは何ですか？

   * Adobe Pass Authenticationによって統合されたすべてのMVPD **は、fireTV SDKで技術的にSSO サポートされます。**

1. 新しいSDKを使用する以外に、プログラマーが認識すべき他の&#x200B;**ワークフローの変更**&#x200B;は何ですか？

   * プログラマーは、FireTV プラットフォーム用のMVPD ピッカーを実装する必要があります。

1. 認証&#x200B;**TTL**&#x200B;に変更はありますか？

   * 認証TTLに関する動作に変更はありません。
   * 最初の有効な認証トークンはSSOの実行に使用され、この場合、SSOを通じて認証されるすべての他のアプリケーションは、有効期限が切れるまで同じTTLを使用します。 つまり、あるアプリケーションから別のアプリケーションに移動する場合、2番目のアプリケーションは、認証する最初のアプリケーションのTTLを共有します。

1. **劣化API**&#x200B;の仕組み？

   * デプロイメント APIに変更は必要ありません。ユーザーエクスペリエンスは、Android デバイスと同じです。

1. **TempPass**&#x200B;のフローはどのように影響しますか？

   * TempPassのフローはシングルスクリーンで、他のネイティブデバイスと同じように動作します。

1. 他のAdobe機能は以前と同じように機能しますか？

   * すべてのAdobe Pass認証機能は、Android デバイスと同様にFireTVで動作します。
