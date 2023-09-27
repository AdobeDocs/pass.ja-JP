---
title: iOS/tvOS のアプリ登録
description: iOS/tvOS のアプリ登録
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# iOS/tvOS のアプリ登録 {#iostvos-application-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#Intro}

iOS/tvOS AccessEnabler SDK のバージョン 3.0 以降は、Adobeのサーバを使用して認証メカニズムを変更します。 公開鍵と秘密鍵システムを使用して requestorID に署名する代わりに、SDK がサーバーに対しておこなうすべての呼び出しに後で使用されるアクセストークンを取得するために使用できる Software 文字列の概念を導入します。 ソフトウェアステートメントに加えて、アプリケーションのカスタム URL スキームも必要です。

詳しくは、 [動的クライアントの登録](/help/authentication/dynamic-client-registration.md)

## ソフトウェアステートメントとは {#Soft_state}

「ソフトウェアステートメント」は、アプリケーションに関する情報を含む JWT トークンです。 各アプリケーションには固有のソフトウェア文が必要です。この文は、Adobeのシステム内のアプリケーションを識別するために、サーバーで使用されます。 AccessEnabler SDK を初期化する際には、Software Statement を渡す必要があります。この文は、アプリケーションをAdobeに登録するために使用されます。 登録時に、SDK はクライアント ID と、アクセストークンの取得に使用されるクライアント秘密鍵を受け取ります。 SDK がサーバーに対しておこなう呼び出しには、有効なアクセストークンが必要です。 SDK は、アプリケーションの登録、アクセストークンの取得および更新をおこないます。

**注意：** ソフトウェアステートメントは、アプリ固有で、同じソフトウェアステートメントを複数のアプリケーションで使用することはできません。 プログラマレベルのソフトウェアステートメントも同じに従うので、単一のチャネルとマルチチャネルのどちらでも、単一のアプリケーションに対してのみ使用できます。 この制限は、カスタムスキームにも適用されます。

## ソフトウェアステートメントを取得する方法 {#obtain}

### Adobeの TVE ダッシュボードにアクセスできる場合：

- ブラウザーを開き、に移動します。 <https://console.auth.adobe.com>
- に移動します。 `Channels` 」セクションで、チャネルを選択します。
- に移動します。 `Registered Applications` タブ。
- クリック： `Add new application`.
- アプリケーションの名前とバージョンを指定し、使用可能なプラットフォームを選択します。 iOS/tvOS を使用しています。
- 変更をサーバーにプッシュし、チャネルの「登録済みアプリケーション」タブに戻ります。
- すべての登録済みアプリケーションのリストが表示されます。 次をクリック：   `Download` 」ボタンをクリックします。 ソフトウェアステートメントをダウンロードする準備が整うまで、数分待たなければならない場合があります。
- テキストファイルがダウンロードされます。 その内容をソフトウェアステートメントとして使用します。

詳しくは、 [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md).

### Adobeの TVE ダッシュボードへのアクセス権がない場合：

にチケットを送信 <tve-support@adobe.com>. チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報をすべて含め、当社のサポートチームの誰かがソフトウェアステートメントを作成します。

## ソフトウェアステートメントの使用方法 {#use}

ソフトウェア文を取得したら、Access Enabler コンストラクタでパラメータとして渡す必要があります。 リモートの場所でソフトウェアステートメントをホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースしなくても、ソフトウェアステートメントを簡単に失効および変更できます。

## アプリケーションのカスタム URL スキームの生成 {#generating}

### Adobeの TVE ダッシュボードにアクセスできる場合：

- ブラウザーを開き、に移動します。 <https://console.auth.adobe.com>
- に移動します。 `Channels` 」セクションで、チャネルを選択します。
- に移動します。 `Custom Schemes` タブ。
- クリック： `Generate a new custom scheme`.
- アプリケーション用に新しいカスタムスキームが生成されます。 例： `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- 変更をサーバーにプッシュします。

### Adobeの TVE ダッシュボードへのアクセス権がない場合：

にチケットを送信 <tve-support@adobe.com>. チャネル ID を含め、サポートチームの誰かがカスタムスキームを作成します。

## カスタムスキームの使用方法 {#use_custom}

アプリケーションの `info.plist` ファイルに次のコードを追加します。

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
