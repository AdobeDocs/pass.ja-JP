---
title: Android Application Registration
description: Android Application Registration
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%
---
# （レガシー） Android Application Registration {#android-application-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#intro}

Android AccessEnabler SDKのバージョン 3.0以降、Adobeのサーバーを使用して認証メカニズムを変更しています。 公開鍵と秘密鍵を使用して依頼者IDに署名する代わりに、SDKがサーバーに対して行うすべての呼び出しに後で使用されるアクセストークンを取得するために使用できるソフトウェアステートメント文字列の概念を導入します。 ソフトウェアステートメントに加えて、アプリケーションのディープリンクも作成する必要があります。

詳しくは、[動的クライアント登録の概要](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)を参照してください。

## ソフトウェアに関する声明とは？ {#what}

ソフトウェアステートメントは、アプリケーションに関する情報を含むJWT トークンです。 各アプリケーションには、Adobe システム内のアプリケーションを識別するためにサーバーが使用する一意のソフトウェアステートメントが必要です。

`AccessEnabler` SDKを初期化する際には、ソフトウェアステートメントを渡す必要があります。 Adobeへの登録に使用されます。 登録時に、SDKは、アクセストークンの取得に使用されるクライアント IDとクライアント秘密鍵を受け取ります。 SDKがAdobe サーバーに対して行う呼び出しには、有効なアクセストークンが必要です。 SDKは、アプリケーションの登録、アクセストークンの取得および更新を担当します。

>[!NOTE]
>
>ソフトウェアステートメントはアプリ固有であり、個別のソフトウェアステートメントを複数のアプリケーションに使用することはできません。 プログラマレベルのソフトウェアステートメントは同じ制約を持つことに注意してください。単一チャネルでもマルチチャネルでも、単一アプリケーションにのみ使用できます。

## ソフトウェアステートメントの取得方法 {#how-to-get-ss}

ソフトウェア・ステートメントを取得する方法を以下に示します。

### AdobeのTVE ダッシュボードにアクセスできる場合

1. ブラウザーを開き、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)に移動します。

1. 「**[!UICONTROL Channels]**」セクションに移動し、チャネルを選択します。

1. 「**[!UICONTROL Registered Applications]**」タブに移動します。

1. **[!UICONTROL Add new application]**&#x200B;をクリックします。

1. アプリケーションに名前を付け、バージョンを指定します。

1. アプリケーションを利用できるプラットフォーム（この場合はAndroid）を選択します。

1. 既にプログラマーに設定されているドメインのリストから選択して、**[!UICONTROL Domain Name]**&#x200B;を指定します。

1. 変更をサーバーにプッシュしてから、チャネルの&#x200B;**[!UICONTROL Registered Applications]** タブに戻ります。

   登録されたすべてのアプリケーションのリストが表示されます。 作成したアプリケーションで&#x200B;**[!UICONTROL Download]**&#x200B;を選択します。 ソフトウェアステートメントをダウンロードする準備が整うまで、数分待つ必要がある場合があります。

   テキストファイルのダウンロード。 その内容をソフトウェアステートメントとして使用します。

詳しくは、[動的クライアント登録管理](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)を参照してください。

### AdobeのTVE ダッシュボードにアクセスできない場合

`tve-support@adobe.com`へのチケット送信。 チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報を含めます。 サポートチームの担当者が、ソフトウェアに関する声明を作成します。

## ソフトウェアステートメントの使用方法 {#how-to-use-ss}

ソフトウェア ステートメントを取得したら、Access Enabler コンストラクターでパラメーターとして渡す必要があります。 ソフトウェアステートメントをリモートの場所でホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースすることなく、ソフトウェアステートメントを簡単に取り消して変更できます。

## アプリケーションのディープリンクの作成と使用 {#create}

Androidでは、ソフトウェアステートメントの作成時に選択したドメイン名の逆のディープリンク値として使用します

作成されたディープリンクは、Android デバイスで一意の値を持つ必要があります。 複数のアプリケーションで同じディープリンク値を使用すると、認証フローとログアウトフローが干渉します。

## ソフトウェアステートメントとディープリンクの使用方法 {#use-both}

アプリケーションのリソース ファイル `strings.xml`に次のコードを追加します。

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
