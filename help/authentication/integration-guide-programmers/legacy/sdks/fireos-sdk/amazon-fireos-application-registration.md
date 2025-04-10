---
title: Amazon FireOS アプリケーションの登録
description: Amazon FireOS アプリケーションの登録
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# （従来の）Amazon FireOS アプリケーションの登録 {#amazon-fireos-application-registration}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

## 概要 {#intro}

FireOS AccessEnabler SDKのバージョン 3.0 以降、Adobeのサーバで認証メカニズムを変更しています。 公開鍵と秘密鍵を使用して requestorID に署名する代わりに、SDKがサーバーに対して行うすべての呼び出しに後で使用されるアクセストークンを取得するために使用できる Software Statement 文字列の概念を導入します。 ソフトウェアのステートメントに加えて、アプリケーションのディープリンクを作成する必要があります。

詳しくは、[ 動的クライアント登録の概要 ](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) を参照してください。

## ソフトウェア ステートメントとは {#what}

ソフトウェアステートメントは、アプリケーションに関する情報を含む JWT トークンです。 各アプリケーションには、Adobeのシステム内のアプリケーションを特定するためにサーバが使用する固有のソフトウェア・ステートメントが必要です。 AccessEnabler SDKを初期化する際に Software Statement を渡す必要があります。このステートメントは、アプリケーションをAdobeに登録する際に使用されます。 登録時に、SDKはクライアント ID と、アクセストークンの取得に使用されるクライアント秘密鍵を受け取ります。 SDKからサーバーへの呼び出しには、有効なアクセストークンが必要です。 SDKは、アプリケーションの登録、アクセストークンの取得と更新を行います。

**注：** ソフトウェア ステートメントはアプリ固有であり、個々のソフトウェア ステートメントは複数のアプリケーションに使用することはできません。 これは、複数のチャネルへのアクセスを提供するアプリケーションにも適用されます。

## ソフトウェアに関する声明の入手方法 {#how-to}

### ダッシュボードの TVE Adobeにアクセスできる場合：

1. ブラウザーを開き、`https://experience.adobe.com/#/pass/authentication` に移動します。

1. **[!UICONTROL Channels]** セクションに移動し、チャネルを選択します。

1. 「**[!UICONTROL Registered Applications]**」タブに移動します。

1. 「**[!UICONTROL Add new application]**」をクリックします。

1. アプリケーションの名前とバージョンを指定し、使用できるプラットフォーム（Androidなど）を選択します。

1. プログラ **[!UICONTROL Domain Name]** ー用に既に設定されているドメインのリストから選択して、プロパティを指定します。

1. 変更をサーバーにプッシュし、チャネルの「**[!UICONTROL Registered Applications]**」タブに戻ります。

   すべての登録済みアプリケーションのリストが表示されます。

1. 作成 **[!UICONTROL Download]** たアプリケーションで「」をクリックします。

   ソフトウェア明細書をダウンロードする準備が整うまで、数分待つ必要がある場合があります。

   テキストファイルがダウンロードされます。 その内容をソフトウェアのステートメントとして使用します。

詳しくは、[Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management) を参照してください。

### ダッシュボードの TVE Adobeへのアクセス権がない場合：

[tve-support@adobe.com](mailto:tve-support@adobe.com) にチケットを送信します。 チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報をすべて含めてください。サポートチームの誰かが、ソフトウェアのステートメントを作成します。

## ソフトウェア ステートメントの使い方 {#use}

ソフトウェア ステートメントを取得したら、それを Access Enabler コンストラクタのパラメータとして渡す必要があります。 Adobeでは、ソフトウェアステートメントをリモートの場所にホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースすることなく、ソフトウェア ステートメントを簡単に取り消して変更できます。

## ソフトウェア ステートメントの使い方 {#use-both}

アプリケーションのリソースファイルに、次のコード `strings.xml` 追加します。

```XML
<string name="software_statement">softwarestatement value</string>
```
