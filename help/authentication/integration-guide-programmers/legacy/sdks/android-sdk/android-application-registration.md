---
title: Android アプリケーションの登録
description: Android アプリケーションの登録
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---

# （従来の）Android アプリケーションの登録 {#android-application-registration}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#intro}

Android AccessEnabler SDKのバージョン 3.0 以降では、Adobeのサーバで認証メカニズムを変更しています。 公開鍵と秘密鍵を使用して requestorID に署名する代わりに、SDKがサーバーに対して行うすべての呼び出しに後で使用されるアクセストークンを取得するために使用できる Software Statement 文字列の概念を導入します。 ソフトウェアのステートメントに加えて、アプリケーションのディープリンクを作成する必要があります。

詳しくは、[&#x200B; 動的なクライアント登録の概要 &#x200B;](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) を参照してください。

## ソフトウェア ステートメントとは {#what}

ソフトウェアステートメントは、アプリケーションに関する情報を含む JWT トークンです。 各アプリケーションには、Adobeのシステム内のアプリケーションを特定するためにサーバが使用する固有のソフトウェア・ステートメントが必要です。

`AccessEnabler` SDKを初期化する際には、ソフトウェア ステートメントを渡す必要があります。 アプリケーションをAdobeに登録するために使用されます。 登録時に、SDKはクライアント ID とクライアントシークレットを受け取り、アクセストークンの取得に使用されます。 SDKからAdobeサーバーに対して行う呼び出しには、有効なアクセストークンが必要です。 SDKは、アプリケーションの登録、アクセストークンの取得および更新を行います。

>[!NOTE]
>
>ソフトウェア ステートメントはアプリ固有であり、個々のソフトウェア ステートメントは複数のアプリケーションに使用できません。 プログラマーレベルのソフトウェアステートメントは同じ制約を持ち、シングルチャネルかマルチチャネルかに関わらず、単一のアプリケーションにのみ使用できることに注意してください。

## ソフトウェアに関する声明の入手方法 {#how-to-get-ss}

ソフトウェア ステートメントを取得する方法を次に示します。

### ダッシュボードの TVE Adobeにアクセスできる場合

1. ブラウザーを開き、[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) に移動します。

1. 「」セクション **[!UICONTROL Channels]** 移動し、チャネルを選択します。

1. 「**[!UICONTROL Registered Applications]**」タブに移動します。

1. 「**[!UICONTROL Add new application]**」をクリックします。

1. アプリケーションに名前を付け、バージョンを指定します。

1. アプリケーションを使用できるプラットフォーム（この場合はAndroid）を選択します。

1. プログラ **[!UICONTROL Domain Name]** ー用に既に設定されているドメインのリストから選択して、プロパティを指定します。

1. 変更をサーバーにプッシュし、チャネルの「**[!UICONTROL Registered Applications]**」タブに戻ります。

   すべての登録済みアプリケーションのリストが表示されます。 作成したアプリケーションで「**[!UICONTROL Download]**」を選択します。 ソフトウェアのステートメントをダウンロードする準備が整うまで、数分待つ必要がある場合があります。

   テキストファイルがダウンロードされます。 その内容をソフトウェアのステートメントとして使用します。

詳しくは、[Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management) を参照してください。

### ダッシュボードの TVE Adobeへのアクセス権がない場合

`tve-support@adobe.com` にチケットを送信します。 チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報を含めます。 サポートチームの誰かが、ソフトウェアのステートメントを作成します。

## ソフトウェア ステートメントの使い方 {#how-to-use-ss}

ソフトウェア ステートメントを取得したら、それを Access Enabler コンストラクタのパラメータとして渡す必要があります。 ソフトウェア ステートメントはリモートの場所でホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースすることなく、ソフトウェア ステートメントを簡単に取り消して変更できます。

## アプリケーションのディープリンクの作成と使用 {#create}

Androidでは、ソフトウェア ステートメントの作成時に選択したドメイン名の逆の値をディープリンク値として使用します

作成されたディープリンクは、Android デバイスで一意の値にする必要があります。 複数のアプリケーションが同じディープリンク値を使用すると、認証フローとログアウトフローが干渉します。

## ソフトウェアのステートメントとディープリンクの使用方法 {#use-both}

アプリケーションのリソースファイルに、次のコード `strings.xml` 追加します。

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
