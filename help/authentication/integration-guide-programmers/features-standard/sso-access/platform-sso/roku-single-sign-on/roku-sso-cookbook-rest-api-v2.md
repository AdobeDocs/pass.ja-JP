---
title: Roku SSO クックブック（REST API V2）
description: Roku SSO クックブック（REST API V2）
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO クックブック（REST API V2） {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

Adobe Pass認証 REST API V2 は、RokuOS で動作するクライアントアプリケーションのエンドユーザーに対して、Platform シングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の [REST API V2 概要の拡張機能として機能し &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) 概要の概要と、[&#x200B; プラットフォーム ID フローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) の実装方法を説明するドキュメントを提供します。

## プラットフォーム ID フローを使用した Roku のシングルサインオン {#cookbook}

Adobe Pass Authentication は Roku と連携して、ログインユーザーエクスペリエンスを向上させ、TV Everywhere アプリケーション全体で TV 購読者のシングルサインオン（SSO）を容易にします。

### 前提条件 {#prerequisites}

プラットフォーム ID フローを使用して Roku のシングルサインオンを続行する前に、Roku SSO が有効になっていることを確認します。 Roku SSO は、プログラマーまたはMVPDが SSO をリクエストしていない限り、デフォルトで有効になっています。

各プログラマーは、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/pass/authentication) を使用して、特定の統合に対して Roku プラットフォームのシングルサインオン（SSO）を有効または無効にすることができます。

### ワークフロー {#workflow}

**クライアントからサーバーへ**

クライアントからサーバーへのアーキテクチャを利用して REST API V2 を統合するプログラマーアプリケーションの場合、Roku SSO は変更せずにシームレスに機能します。

RokuOS は、Adobe Pass Authentication エンドポイントに送信されるすべてのリクエストに 2 つの HTTP ヘッダーを自動的に追加します。

**サーバー間**

サーバー間アーキテクチャを使用して REST API V2 を統合するプログラマーアプリケーションの場合、プログラマーは、Roku チームと調整して、ドメインに向けられたすべての API フローに含まれるようにこれらのヘッダーを設定する必要があります。

クロスアプリケーションおよびクロスデバイス SSO を有効にするには、アプリケーションによって渡されるデバイス ID の代わりに、Roku が提供するサブスクライバー ID を使用する必要があります。

詳しくは、次のドキュメントを参照してください。

* [ヘッダー – X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [ヘッダー – AP デバイス識別子](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

必要なヘッダーのフォーマットについて詳しくは、Adobe担当者にお問い合わせください。

### よくある質問 {#faqs}

* **SSO はどのように機能しますか？**

  SSO は、同じ Roku ユーザーに関連付けられたすべての Roku デバイス上で、Adobe Pass Authentication を利用したすべてのプログラマーアプリケーションで機能します。 すべての MVPD が Roku SSO を許可するわけではありません。


* **認証 TTL に変更はありますか？**

  最初の有効な認証トークンは、SSO の実行に使用されます。この場合、SSO 経由で認証される他のすべてのアプリケーションは、有効期限が切れるまで同じ TTL を使用します。 したがって、あるアプリケーションから別のアプリケーションに移動する際に、2 番目のアプリケーションは、認証する最初のアプリケーションの TTL を共有します。


* **他のAdobe機能は以前と同様に機能しますか？**

  すべてのAdobe Pass認証機能は、以前と同様に機能します。


* **Roku プラットフォームで SSO のメリットを享受しているプログラマーのオプトイン/オプトアウトプロセスはありますか？**

  これは、Adobeの TVE Dashboard の設定変更になります。 各プログラマーは、特定の統合に対して、Roku プラットフォームで SSO を有効または無効にできます。


* **一般的な問題を教えてください。**

  プログラマーは、Adobeの REST API に基づく現在の実装が Roku の platform-SSO を妨げないことを確認する必要があります。

  考えられる問題とその解決方法の一覧を以下に示します。

| 問題 | 考えられる原因 | 可能な解決策 |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Adobeに Roku SSO ヘッダーが送信されていません | Adobe Pass認証ドメインへの呼び出しに HTTPS ではなく HTTP を使用 | HTTPS を使用 |
| MVPDのロゴが表示されない/SSO トークンで更新されない | UI はローカルストレージに依存 | 認証を確認した後、アプリケーションは UI （および必要に応じてローカルストレージ）を更新する |
| AuthZ なしでトリガーされたログアウト | アプリケーション設計 | バックグラウンドでログアウトを実行しないようにアプリケーションを更新する必要があります |
