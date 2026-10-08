---
title: Amazon FireOS Application Registration
description: Amazon FireOS Application Registration
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%
---
# （レガシー） Amazon FireOS アプリケーションの登録 {#amazon-fireos-application-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

## 概要 {#intro}

FireOS AccessEnabler SDKのバージョン 3.0以降、Adobeのサーバーを使用して認証メカニズムを変更しています。 公開鍵と秘密鍵を使用して依頼者IDに署名する代わりに、SDKがサーバーに対して行うすべての呼び出しに後で使用されるアクセストークンを取得するために使用できるソフトウェアステートメント文字列の概念を導入します。 ソフトウェアステートメントに加えて、アプリケーションのディープリンクも作成する必要があります。

詳しくは、[動的クライアント登録の概要](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)を参照してください。

## ソフトウェアに関する声明とは？ {#what}

ソフトウェアステートメントは、アプリケーションに関する情報を含むJWT トークンです。 各アプリケーションには、Adobe システム内のアプリケーションを識別するためにサーバーが使用する固有のソフトウェアステートメントが必要です。 AccessEnabler SDKを初期化する際にソフトウェア ステートメントを渡す必要があり、それを使用してアプリケーションをAdobeに登録します。 登録時に、SDKはクライアント IDとアクセストークンの取得に使用されるクライアント秘密鍵を受け取ります。 SDKが当社のサーバーに対して行う呼び出しには、有効なアクセストークンが必要です。 SDKは、アプリケーションの登録、アクセストークンの取得と更新を担当します。

**注：** ソフトウェア ステートメントはアプリ固有であり、個別のソフトウェア ステートメントを複数のアプリケーションに使用することはできません。 これは、複数のチャネルへのアクセスを提供するアプリケーションにも適用されます。

## ソフトウェアステートメントを取得するには？ {#how-to}

### AdobeのTVE ダッシュボードにアクセスできる場合：

1. ブラウザーを開き、`https://experience.adobe.com/#/pass/authentication`に移動します。

1. 「**[!UICONTROL Channels]**」セクションに移動し、チャネルを選択します。

1. 「**[!UICONTROL Registered Applications]**」タブに移動します。

1. **[!UICONTROL Add new application]**&#x200B;をクリックします。

1. アプリケーションの名前とバージョンを入力し、アプリケーションを利用できるプラットフォーム（Androidなど）を選択します。

1. 既にプログラマーに設定されているドメインのリストから選択して、**[!UICONTROL Domain Name]**&#x200B;を指定します。

1. 変更をサーバーにプッシュしてから、チャネルの&#x200B;**[!UICONTROL Registered Applications]** タブに戻ります。

   登録されたすべてのアプリケーションのリストが表示されます。

1. 作成したアプリケーションで「**[!UICONTROL Download]**」をクリックします。

   ソフトウェアステートメントをダウンロードする準備が整うまで、数分待つ必要がある場合があります。

   テキストファイルのダウンロード。 その内容をソフトウェアステートメントとして使用します。

詳しくは、[動的クライアント登録管理](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)を参照してください。

### AdobeのTVE ダッシュボードにアクセスできない場合：

チケットを[tve-support@adobe.com](mailto:tve-support@adobe.com)に送信します。 チャネル、アプリケーション名、バージョン、プラットフォームなど、必要なすべての情報を含め、サポートチームの誰かがソフトウェアステートメントを作成します。

## ソフトウェアステートメントの使用方法 {#use}

ソフトウェア ステートメントを取得したら、Access Enabler コンストラクターでパラメーターとして渡す必要があります。 Adobeでは、リモートの場所でソフトウェアステートメントをホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースすることなく、ソフトウェアステートメントを簡単に取り消して変更できます。

## ソフトウェアステートメントの使用方法 {#use-both}

アプリケーションのリソース ファイル `strings.xml`に次のコードを追加します。

```XML
<string name="software_statement">softwarestatement value</string>
```
