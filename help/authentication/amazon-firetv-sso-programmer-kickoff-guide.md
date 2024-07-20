---
title: Amazon fireTV SSO - プログラマー向けキックオフガイド
description: Amazon fireTV SSO - プログラマー向けキックオフガイド
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Amazon fireTV SSO - プログラマー向けキックオフガイド {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

## 概要 {#intro}

このドキュメントでは、新しい **Adobe Pass認証の fireTV SDK** を fireTV アプリケーションに統合するために必要な情報について説明します。 この新しい SDK は、Amazonの fireTV プラットフォームの OS レベルの統合を利用して、**シングルサインオン** をサポートしています。 シングルサインオンを活用するには、クライアントレス API から新しい fireTV SDK にアプリケーションを移行する際に、お客様側でわずかな作業が必要になります。 認証フローには、以下で詳しく説明するいくつかの変更があります。

## アーキテクチャの概要と OS レベルの統合 {#high}

Amazonの fireTV Everywhere アプリ間のシングルサインオンを実現し、このプラットフォームでの全体的なエクスペリエンスを向上させるために、fireTV OS レベルでコア SDK を統合することにしました。 プログラマーは、Adobeが提供するスタブライブラリに対してコンパイルする必要があります。 実際の機能は、Amazon FireTV OS に存在するAdobeのライブラリによって提供されます。

Amazonが OS レベルで私たちのライブラリを組み込んだ fireTV シミュレータを提供するまで、開発は実際の fireTV デバイスを使用することでのみ可能です。

## 利点 {#bene}

* すべての統合された MVPD を備えたAmazon fireTV プラットフォーム上のすべてのAdobeを利用した TV Everywhere アプリケーションの間でシングルサインオン。
* HBA （サポートされる MVPD を使用）のメリットを活用できます。
* 新しい SDK バージョンがリリースされるたびにアプリケーションを更新する必要なく、最新の fireTV SDK を使用できます。
* すべての TVE アプリケーションは、AccessEnabler ライブラリのローカル・コピーを使用する必要がないため、共有システム・ライブラリを使用するメリットがあります。 これにより、すべてのアプリケーションで同じ SDK バージョンが使用されるようになります。
* 単一の画面認証 – 登録コードや第 2 画面ワークフローは不要です。

## クライアントレス API ベースのアプリから fireTV SDK ベースのアプリへの移行 {#migra1}

クライアントレス API から fireTV SDK に移行するには、クライアントレス API に関連するコードベースを削除し、新しい fireTV SDK を統合する必要があります。

クライアントレス API ベースのアプリと比較すると、新しい fireTV SDK では認証が最初の画面に移動され、2 番目の画面の認証は必要なくなりました。

そのため、プログラマーはアプリに MVPD ピッカーを追加して、ユーザーが fireTV デバイスで TV プロバイダーを選択できるようにする必要があります。 MVPD を選択すると、fireTV デバイスの MVPD ログインページが表示されます。

FireTV での通常、HBA、および SSO シナリオを示すユーザーフローのワイヤーフレームは、[Amazon Fire TV - MVVPD Sign-in User Flow](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/) にあります。

## Android SDK ベースアプリから fireTV SDK ベースアプリへの移行 {#migra2}

この新しい fireTV SDK は、既存のAndroid SDK と非常に似ており、**Android SDK の統合** に関する現在のドキュメントは、fireTV SDK のドキュメントを準備す <!--http://tve.helpdocsonline.com/android-technical-overview--> まで使用できます。 Android SDK を使用するAndroid アプリケーションが既にある場合、fireTV アプリケーションへの fireTV SDK の統合は簡単です。

既存のAndroid SDK と比較して、fireTV SDK では、MVPD ログインページの管理/提示や AuthN トークンの取得のタスクが AccessEnabler ライブラリによって内部的に実行されるため、認証プロセスが容易になります。

## よくある質問 {#faq}

1. **SSO** の仕組み

   * SSO は、同じAmazon fireTV デバイスで新しい fireTV SDK を使用している、Adobe Pass Authentication を利用したすべてのプログラマーアプリケーションで機能します
   * クライアントレス REST API で実装されたプログラマーアプリと fireTV SDK で実装されたアプリの SSO **はサポートされません**

1. FireTV SSO の MVPD のカバレッジは何ですか？

   * Adobe Pass Authentication によって統合された **すべての MVPD** は、fireTV SDK で技術的に SSO サポートされます。

1. 新しい SDK を使用する以外に、プログラマーは他にどのような **ワークフローの変更** に注意する必要がありますか？

   * プログラマーは、fireTV プラットフォーム用の MVPD ピッカーを実装する必要があります。

1. 認証 **TTL** に変更はありますか？

   * 認証 TTL に関する動作に変更はありません。
   * 最初の有効な認証トークンは、SSO の実行に使用されます。この場合、SSO 経由で認証される他のすべてのアプリケーションは、有効期限が切れるまで同じ TTL を使用します。 したがって、あるアプリケーションから別のアプリケーションに移動する際に、2 番目のアプリケーションは、認証する最初のアプリケーションの TTL を共有します。

1. **degradation API** の仕組み

   * Degradation API に変更は必要ありません。ユーザーエクスペリエンスはAndroid デバイスと同じです。

1. **TempPass** フローはどのような影響を受けますか？

   * TempPass フローは単一の画面で、他のネイティブデバイスと同様に動作します。

1. 他のAdobe機能は以前と同様に機能しますか？

   * すべてのAdobe Pass認証機能は、fireTV ではAndroid デバイスと同様に機能します。
