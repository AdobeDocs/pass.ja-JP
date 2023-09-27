---
title: Amazon fireTV SSO — プログラマーキックオフガイド
description: Amazon fireTV SSO — プログラマーキックオフガイド
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Amazon fireTV SSO — プログラマーキックオフガイド {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

## はじめに {#intro}

このドキュメントでは、新しい **Adobe Pass認証の fireTV SDK** （fireTV アプリケーション内） この新しい SDK は、Amazonの fireTV プラットフォームで OS レベルの統合を活用し、次の機能を提供します。 **シングルサインオン** サポート。 シングルサインオンを活用するには、Clientless API から新しい fireTV SDK にアプリケーションを移行するために、自分の側で少しの努力が必要です。 次に、認証フローにいくつかの変更点を示します。

## アーキテクチャの概要と OS レベルの統合 {#high}

Amazon fireTV プラットフォームの TV Everywhere アプリケーション間でシングルサインオンを実現し、このプラットフォームの全体的なエクスペリエンスを向上させるために、コア SDK を fireTV OS レベルで統合することにしました。 プログラマは、Adobeが提供するスタブライブラリに対してコンパイルする必要があります。 実際の機能は、Amazonの fireTV OS にあるAdobeのライブラリによって提供されます。

Amazonが OS レベルのライブラリを組み込んだ fireTV シミュレーターを提供するまで、開発は実際の fireTV デバイスを使用してのみ可能になります。

## メリット {#bene}

* すべてのAdobeを利用した TV の間のシングルサインオンAmazon fireTV プラットフォーム上のすべてのアプリケーションとすべての統合 MVPD との間のシングルサインオン。
* HBA（サポートされる MVPD を使用）のメリットを得る機能。
* 新しい SDK バージョンがリリースされるたびにアプリケーションを更新する必要なく、最新の fireTV SDK を使用できます。
* すべての TVE アプリケーションは、AccessEnabler ライブラリのローカルコピーを作成する必要がなくなるので、共有システムライブラリを使用するメリットが得られます。 これにより、すべてのアプリケーションで同じ SDK バージョンが使用されていることも確認できます。
* 単一画面認証 — 登録コードや 2 番目の画面ワークフローは不要。

## クライアントレス API ベースのアプリから FireTV SDK ベースのアプリへの移行 {#migra1}

Clientless API から FireTV SDK に移行する場合は、Clientless API に関連するコードベースを削除し、新しい FireTV SDK を統合する必要があります。

クライアントレス API ベースのアプリと比較して、新しい fireTV SDK では認証が最初の画面に移行するので、2 番目の画面認証は不要になりました。

これには、ユーザーが fireTV デバイスで TV プロバイダを選択できるように、Programmers がアプリに MVPD ピッカーを追加する必要があります。 MVPD を選択すると、ユーザーは fireTV デバイス上の MVPD ログインページを表示します。

fireTV の通常のシナリオ、HBA、SSO のシナリオを説明するユーザーフローのワイヤフレームは、次の場所で確認できます。 [Amazon Fire TV - MVVPD ログインユーザーフロー](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Android SDK ベースのアプリから FireTV SDK ベースのアプリへの移行 {#migra2}

この新しい fireTV SDK は、既存の Android SDK および現在のドキュメントと非常に似ています **アドビの Android SDK の統合** <!--http://tve.helpdocsonline.com/android-technical-overview-->は、fireTV SDK ドキュメントの準備が整うまで使用できます。 アドビの Android SDK を使用する Android アプリケーションが既にある場合、fireTV アプリケーションでの fireTV SDK の統合は簡単である必要があります。

既存の Android SDK と比較すると、fireTV SDK では、MVPD ログインページの管理/表示と AuthN トークンの取得のタスクが AccessEnabler ライブラリによって内部的に実行されるので、認証プロセスの開発がより簡単になります。

## よくある質問 {#faq}

1. どのように **SSO** 仕事？

   * SSO は、同じAmazon fireTV デバイスで新しい fireTV SDK を使用するAdobe Pass Authentication を利用したすべてのプログラマーアプリケーションで機能します。
   * クライアントレス REST API で実装されたプログラマーアプリと fireTV SDK で実装されたアプリの間の SSO **はサポートされません**

1. fireTV SSO の MVPD カバレッジは何ですか？

   * **すべての MVPD** Adobe Pass Authentication による統合は、fireTV SDK で技術的に SSO がサポートされます。

1. 新しい SDK を使用する以外に、その他の機能 **ワークフローの変更** プログラマーは気づいておく必要がありますか？

   * プログラマーは、fireTV プラットフォーム用の MVPD ピッカーを実装する必要があります。

1. 認証に変更がある場合は、 **TTL**?

   * 認証 TTL に関する動作は変更されません。
   * 最初の有効な認証トークンが SSO の実行に使用され、この場合、SSO を通じて認証される他のすべてのアプリケーションは、期限が切れるまで同じ TTL を使用します。 したがって、あるアプリケーションから別のアプリケーションに移動すると、2 つ目のアプリケーションは、認証される最初のアプリケーションの TTL を共有します。

1. How **劣化 API** 仕事？

   * 劣化 API に変更を加える必要はありません。ユーザーエクスペリエンスは、Android デバイスと同じになります。

1. 方法 **TempPass** フローに影響があるか

   * TempPass フローは単一の画面であり、他のネイティブデバイスと同様に動作します。

1. その他のAdobe機能は、以前と同様に動作しますか？

   * すべてのAdobe Pass認証機能は、Android デバイスと同様に fireTV で動作します。
