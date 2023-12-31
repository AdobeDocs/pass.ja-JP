---
title: Amazon FireOS アプリケーションの登録
description: Amazon FireOS アプリケーションの登録
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Amazon FireOS アプリケーションの登録 {#amazon-fireos-application-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

## はじめに {#intro}

FireOS AccessEnabler SDK のバージョン 3.0 以降では、Adobeのサーバを使用して認証メカニズムを変更します。 公開鍵と秘密鍵システムを使用して requestorID に署名する代わりに、SDK がサーバーに対しておこなうすべての呼び出しに後で使用されるアクセストークンの取得に使用できる「ソフトウェアステートメント」文字列の概念を導入します。 ソフトウェアステートメントに加えて、アプリケーションのディープリンクを作成する必要もあります。

詳しくは、 [動的クライアントの登録](/help/authentication/dynamic-client-registration.md)

## ソフトウェアステートメントとは {#what}

「ソフトウェアステートメント」は、アプリケーションに関する情報を含む JWT トークンです。 各アプリケーションには固有のソフトウェアステートメントが必要です。このステートメントは、Adobeのシステム内のアプリケーションを識別するために、サーバーで使用されます。 AccessEnabler SDK を初期化する際には、Software Statement を渡す必要があります。この文は、アプリケーションをAdobeに登録するために使用されます。 登録時に、SDK はクライアント ID と、アクセストークンの取得に使用されるクライアント秘密鍵を受け取ります。 SDK がサーバーに対しておこなう呼び出しには、有効なアクセストークンが必要です。 SDK は、アプリケーションの登録、アクセストークンの取得および更新をおこないます。

**注意：** ソフトウェアステートメントはアプリ固有で、個々のソフトウェアステートメントは複数のアプリケーションに対して使用できません。 これは、複数のチャネルへのアクセスを提供するアプリケーションにも当てはまります。

## ソフトウェアステートメントを取得する方法 {#how-to}

### Adobeの TVE ダッシュボードにアクセスできる場合：

1. ブラウザーを開き、に移動します。 `https://console.auth.adobe.com`.

1. 次に移動： **[!UICONTROL Channels]** 」セクションで、チャネルを選択します。

1. 次に移動： **[!UICONTROL Registered Applications]** タブをクリックします。

1. クリック **[!UICONTROL Add new application]**.

1. アプリケーションの名前とバージョンを指定し、使用可能にするプラットフォーム（Android など）を選択します。

1. 次を提供： **[!UICONTROL Domain Name]** プログラマー用に既に設定されているドメインのリストから選択することで、

1. 変更をサーバーにプッシュして、チャネルの **[!UICONTROL Registered Applications]** タブをクリックします。

   すべての登録済みアプリケーションのリストが表示されます。

1. クリック **[!UICONTROL Download]** をクリックします。

   ソフトウェアステートメントのダウンロード準備が整うまで、数分待たなければならない場合があります。

   テキストファイルがダウンロードされます。 その内容をソフトウェアステートメントとして使用します。

詳しくは、 [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)

### Adobeの TVE ダッシュボードへのアクセス権がない場合：

にチケットを送信 [tve-support@adobe.com](mailto:tve-support@adobe.com). チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報をすべて含め、サポートチームの誰かがソフトウェアステートメントを作成します。

## ソフトウェア文の使用方法 {#use}

ソフトウェア文を取得したら、Access Enabler コンストラクタのパラメータとして渡す必要があります。 Adobeは、リモートの場所でソフトウェア文をホストすることを推奨します。 これにより、アプリケーションの新しいバージョンをリリースすることなく、ソフトウェアステートメントを簡単に失効および変更できます。

## ソフトウェア文の使用方法 {#use-both}

アプリケーションのリソースファイル内 `strings.xml` 次のコードを追加します。

```XML
<string name="software_statement">softwarestatement value</string>
```
