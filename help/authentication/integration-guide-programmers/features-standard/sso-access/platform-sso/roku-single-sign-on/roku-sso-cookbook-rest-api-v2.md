---
title: Roku SSO クックブック （REST API V2）
description: Roku SSO クックブック （REST API V2）
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 0%
---
# Roku SSO クックブック （REST API V2） {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

Adobe Pass Authentication REST API V2は、RokuOSで動作するクライアントアプリケーションのエンドユーザー向けに、Platform Single Sign-On （SSO）をサポートしています。

このドキュメントは、プラットフォーム ID フロー[&#128279;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)を使用して シングルサインオンを実装する方法を説明するドキュメントと、概要レベルのビューを提供する既存の[REST API V2概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)の拡張機能として機能します。

## プラットフォーム ID フローを使用したRoku シングルサインオン {#cookbook}

Adobe Pass Authenticationは、Rokuと連携して、TV Everywhere アプリケーション全体でTV加入者のログインユーザーエクスペリエンスを向上させ、シングルサインオン（SSO）を促進します。

### 前提条件 {#prerequisites}

プラットフォーム ID フローを使用してRoku シングルサインオンを続行する前に、Roku SSOが有効になっていることを確認します。 Roku SSOは、SSOに対するプログラマーまたはMVPDのリクエストを無効にしない限り、デフォルトで有効になっています。

各プログラマーは、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/pass/authentication)を通じて、特定の統合のためにRoku プラットフォーム上のシングルサインオン（SSO）を有効または無効にできます。

### ワークフロー {#workflow}

**クライアント間**

クライアント間アーキテクチャを利用してREST API V2を統合するプログラマーアプリケーションの場合、Roku SSOは変更なしでシームレスに機能します。

RokuOSは、Adobe Pass認証エンドポイントに送信されたすべてのリクエストに2つのHTTP ヘッダーを自動的に追加します。

**サーバー間**

サーバー間アーキテクチャを使用してREST API V2を統合するプログラマーアプリケーションの場合、プログラマーはRoku チームと連携して、これらのヘッダーをドメインに送信されるすべてのAPI フローに含めるように設定する必要があります。

クロスアプリケーションおよびクロスデバイス SSOを有効にするには、Rokuが提供するサブスクライバーIDを、アプリケーションで渡されたときにデバイス IDの代わりに使用する必要があります。

詳しくは、次のドキュメントを参照してください。

* [Header - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Header - AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

必要なヘッダーのフォーマットの詳細については、Adobe担当者にお問い合わせください。

### FAQ {#faqs}

* **SSOの仕組みはどのようになりますか？**

  SSOは、同じRoku ユーザーに関連するすべてのRoku デバイスで、Adobe Pass認証を利用するすべてのプログラマーアプリケーションで動作します。 すべてのMVPDがRoku SSOを許可するわけではありません。


* **認証TTLに変更はありますか？**

  最初の有効な認証トークンはSSOの実行に使用され、この場合、SSOを通じて認証されるすべての他のアプリケーションは、有効期限が切れるまで同じTTLを使用します。 したがって、あるアプリケーションから別のアプリケーションに移動する場合、2番目のアプリケーションは、認証する最初のアプリケーションのTTLを共有します。


* **他のAdobe機能は以前と同じように機能しますか？**

  すべてのAdobe Pass認証機能は、以前と同じように機能します。


* **Roku プラットフォームでSSOから恩恵を受けるプログラマーのオプトイン/オプトアウトプロセスはありますか？**

  これは、AdobeのTVE ダッシュボードの設定変更になります。 各プログラマーは、特定の統合のためにRoku プラットフォームでSSOを有効または無効にすることができます。


* **一般的な問題は何ですか？**

  プログラマーは、AdobeのREST APIに基づく現在の実装がRokuのPlatform-SSOを妨げないことを確認する必要があります。

  考えられる問題とその解決方法のリストを以下に示します。

| 問題 | 考えられる原因 | 可能な解決策 |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| AdobeにRoku SSO ヘッダーが送信されません | Adobe Pass認証ドメインへの呼び出しにHTTPSの代わりにHTTPを使用する | HTTPSを使用 |
| SSO トークンのMVPD ロゴが表示されない/更新されない | UIはローカルストレージに依存しています | 認証を確認した後、アプリケーションはUI （および必要に応じてローカルストレージ）を更新する必要があります |
| AuthZに対してログアウトがトリガーされない | アプリケーション設計 | アプリケーションを更新して、バックグラウンドでログアウトを実行しないようにします |
