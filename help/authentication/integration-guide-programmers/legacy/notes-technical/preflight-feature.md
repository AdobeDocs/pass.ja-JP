---
title: プリフライト機能、問題の有効化、トラブルシューティング、または特定の方法
description: プリフライト機能、問題の有効化、トラブルシューティング、または特定の方法
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%
---
# （レガシー）プリフライト機能：問題の有効化、トラブルシューティング、または特定の方法 {#preflight-feature}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

Adobe Pass AuthenticationのpreAuthorizeResourcesの計算方法が変更されました。 PreAuthorization APIに新しい実装が追加されました。 この実装は、複数の認証呼び出しのみを行う従来のソリューションに取って代わるものです。
PreAuthorization APIの外部インターフェイスは変更されず、プログラマーアプリケーションで更新は必要ありません。

プリフライトリソースの計算には、次の3つの方法があります。

* **MVPDへの分岐および結合メソッド**：これには、AdobeがMVPDに対して複数の認証コールを実行することが含まれます（ただし、クライアントは1回のプリフライト コールを実行する必要があります）。
* **チャネルラインアップ**: MVPDは、ログインしたユーザーのチャネルラインアップをSAML認証応答で公開し、Adobeはそれに基づいて承認済みリソースを返します。 SAML トレーサーのSAML authN応答は、そのリストを公開する必要があります。
* **マルチチャネル認証**: クライアントとAdobeの認証は、両方とも、一連のリソースに対してMVPDへの1回の呼び出しを行います。

MVPDに関係なく、クライアントアプリケーションはプリフライトエンドポイント（checkPreauthorizedResources API）を1回呼び出し、一連のresourceIDを渡します。 MVPDでサポートされている上記のいずれかの方法に基づいて、Adobeは事前承認済みのresourceIDを返します。

Preflightがfork &amp; join メソッドに基づいている場合、Adobe Pass Authentication バックエンドは、その設定で「max preauthorization calls」の値セットをチェックします。 これはAdobeで設定します。

「max preauthorization calls」設定のデフォルト値は「5」で、fork &amp; join MVPDのプリフライトでは最大5つのリソースのみを送信できます。 5つ以上のリソースを渡すと、例外が発生し、null リストが返されます。 これは期待される動作です。 MVPDがチャネルラインナップまたはマルチチャネル認証をサポートしていない場合は、これを任意の値に設定できますが、複数のfork &amp; join認証の呼び出しが読み込み時間を増やすので、それらを調べた後にのみ設定できます。

したがって、MVPDのプリフライトを有効にする場合やトラブルシューティングを行う場合は、次の点を確認する必要があります。

* MVPDがサポートする方式（fork &amp; join、channel line-upまたはmulti-channel）。
* フォークと結合のみがサポートされている場合は、プリフライト呼び出しで送信するリソース IDの数をプログラマーに尋ねる必要があります。
* MVPDは調べる必要があり、「n」個のfork &amp; join認証呼び出しを行った場合の影響を知る必要があります。 その後、値が5より大きい場合は、configで設定する必要があります。

**制限**

ただし、一部のMVPDのプリフライト呼び出しから、AT&amp;T &amp; TWCなどの一部のMVPDのリソース IDが偽のIDまたはプリフライト呼び出しで送信されるリソース IDのリストに認識されないIDである場合、そのリストに有効で許可されたリソースがあるにもかかわらず、そのリソース IDは取得されないことに注意してください。
