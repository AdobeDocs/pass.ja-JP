---
title: Amazon fireTV SSO - プログラマー向けキックオフガイド
description: Amazon fireTV SSO - プログラマー向けキックオフガイド
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# （従来の）Amazon fireTV SSO - プログラマー向けキックオフガイド {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

## 概要 {#intro}

このドキュメントでは、新しい **Adobe Pass認証の fireTV SDK** を fireTV アプリケーションに組み込むために必要な情報について説明します。 この新しいSDKは、Amazonの fireTV プラットフォームの OS レベルの統合を利用して、**シングルサインオン** をサポートしています。 シングルサインオンのメリットを活用するには、クライアントレス API から新しい fireTV SDKにアプリケーションを移行する際に、お客様側から少し手間がかかります。 認証フローには、以下で詳しく説明するいくつかの変更があります。

## アーキテクチャの概要と OS レベルの統合 {#high}

Amazonの fireTV プラットフォーム上で TV Everywhere アプリ間のシングルサインオンを実現し、このプラットフォームでの全体的なエクスペリエンスを向上させるために、私たちは FireTV OS レベルで私たちのコアSDKを統合することにしました。 プログラマーは、Adobeが提供するスタブライブラリに対してコンパイルする必要があります。 実際の機能は、Amazon FireTV OS に存在するAdobeのライブラリによって提供されます。

Amazonが OS レベルで私たちのライブラリを組み込んだ fireTV シミュレータを提供するまで、開発は実際の fireTV デバイスを使用することでのみ可能です。

## 利点 {#bene}

* すべての統合された MVPD を備えたAmazon fireTV プラットフォーム上のすべてのAdobeを利用した TV Everywhere アプリケーションの間でシングルサインオン。
* HBA （サポートされる MVPD を使用）のメリットを活用できます。
* 新しいSDK版がリリースされるたびにアプリを更新する必要なく、最新の fireTV SDKを使用できます。
* すべての TVE アプリケーションは、AccessEnabler ライブラリのローカル・コピーを使用する必要がないため、共有システム・ライブラリを使用するメリットがあります。 これにより、すべてのアプリケーションが同じSDKのバージョンを使用していることも確認できます。
* 単一の画面認証 – 登録コードや第 2 画面ワークフローは不要です。

## クライアントレス API ベースのアプリから fireTV SDK ベースのアプリへの移行 {#migra1}

クライアントレス API から fireTV SDKに移行するには、クライアントレス API に関連するコードベースを削除し、新しい fireTV SDKを統合する必要があります。

クライアントレス API ベースのアプリと比較すると、新しい fireTV SDKでは、認証が 1 番目の画面に移動するため、2 番目の画面の認証は必要なくなりました。

そのため、プログラマーはアプリにMVPDピッカーを追加して、fireTV デバイスで TV プロバイダーを選択できるようにする必要があります。 MVPDを選択すると、fireTV 端末のMVPDログインページが表示されます。

FireTV での通常、HBA、および SSO シナリオを示すユーザーフローのワイヤーフレームは、[Amazon Fire TV - MVVPD Sign-in User Flow](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/) にあります。

## Android SDK ベースのアプリから fireTV SDK ベースのアプリへの移行 {#migra2}

この新しい fireTV SDKは、既存のAndroid SDKと非常に似ています。また、**Android SDKの統合** に関する現在のドキュメントは、fireTV SDKのドキュメントを準備す <!--http://tve.helpdocsonline.com/android-technical-overview--> まで使用できます。 Android SDKを使用するAndroid アプリケーションが既にある場合は、fireTV アプリケーションに fireTV SDKを統合するのは簡単です。

FireTV SDKでは、既存のAndroid SDKと比較して、MVPDのログインページの管理/提示や AuthN トークンの取得のタスクは AccessEnabler ライブラリによって内部的に実行されるため、認証プロセスを簡単に作成できます。

## よくある質問 {#faq}

1. **SSO** の仕組み

   * SSO は、同じAmazon fireTV デバイスで新しい fireTV SDKを使用している、Adobe Pass Authentication を利用したすべてのプログラマーアプリケーションで機能します
   * クライアントレス REST API で実装されたプログラマーアプリと fireTV SDKで実装されたアプリとの SSO **はサポートされません**

1. FireTV SSO のMVPDのカバレッジは何ですか？

   * Adobe Pass Authentication によって統合された **すべての MVPD** は、fireTV SDKで技術的に SSO サポートされます。

1. 新しいSDKを使用する以外に、プログラマーは他にどのような **ワークフローの変更** に注意する必要がありますか？

   * プログラマーは、fireTV プラットフォーム用のMVPD ピッカーを実装する必要があります。

1. 認証 **TTL** に変更はありますか？

   * 認証 TTL に関する動作に変更はありません。
   * 最初の有効な認証トークンは、SSO の実行に使用されます。この場合、SSO 経由で認証される他のすべてのアプリケーションは、有効期限が切れるまで同じ TTL を使用します。 したがって、あるアプリケーションから別のアプリケーションに移動する際に、2 番目のアプリケーションは、認証する最初のアプリケーションの TTL を共有します。

1. **degradation API** の仕組み

   * Degradation API に変更は必要ありません。ユーザーエクスペリエンスはAndroid デバイスと同じです。

1. **TempPass** フローはどのような影響を受けますか？

   * TempPass フローは単一の画面で、他のネイティブデバイスと同様に動作します。

1. 他のAdobe機能は以前と同様に機能しますか？

   * すべてのAdobe Pass認証機能は、fireTV ではAndroid デバイスと同様に機能します。
