---
title: iOS/tvOS アプリケーションの登録
description: iOS/tvOS アプリケーションの登録
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---


# （従来の）iOS/tvOS アプリケーションの登録 {#iostvos-application-registration}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#Intro}

iOS/tvOS AccessEnabler SDKのバージョン 3.0 以降、Adobeのサーバで認証メカニズムを変更しています。 公開鍵と秘密鍵を使用して requestorID に署名する代わりに、SDKがサーバーに対して行うすべての呼び出しに後で使用されるアクセストークンを取得するために使用できるソフトウェアステートメント文字列の概念を導入します。 ソフトウェアのステートメントに加えて、アプリケーションのカスタム URL スキームも必要です。

詳しくは、[ 動的なクライアント登録の概要 ](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) を参照してください。

## ソフトウェア ステートメントとは {#Soft_state}

ソフトウェアステートメントは、アプリケーションに関する情報を含む JWT トークンです。 各アプリケーションには、Adobeのシステム内のアプリケーションを特定するためにサーバが使用する固有のソフトウェア・ステートメントが必要です。 AccessEnabler SDKを初期化する際に Software Statement を渡す必要があります。このステートメントは、アプリケーションをAdobeに登録する際に使用されます。 登録時に、SDKはクライアント ID と、アクセストークンの取得に使用されるクライアント秘密鍵を受け取ります。 SDKからサーバーへの呼び出しには、有効なアクセストークンが必要です。 SDKは、アプリケーションの登録、アクセストークンの取得と更新を行います。

**注：** ソフトウェア ステートメントはアプリ固有であり、同じソフトウェア ステートメントを複数のアプリケーションで使用することはできません。 プログラマーレベルのソフトウェアステートメントも同じことに従うことに注意してください。つまり、シングルチャネルかマルチチャネルかに関わらず、単一のアプリケーションにのみ使用できます。 この制限は、カスタムスキームにも適用されます。

## ソフトウェアに関する声明の入手方法 {#obtain}

### ダッシュボードの TVE Adobeにアクセスできる場合：

- ブラウザーを開き、<https://experience.adobe.com/#/pass/authentication> に移動します。
- 「」セクション `Channels` 移動し、チャネルを選択します。
- 「」タブに移動 `Registered Applications` ます。
- 「`Add new application`」をクリックします。
- アプリケーションの名前とバージョンを指定して、   それが利用可能になるプラットフォーム。 ここではiOS/tvOS です。
- 変更をサーバーにプッシュし、チャネルの「登録済みアプリケーション」タブに戻ります。
- すべての登録済みアプリケーションのリストが表示されます。 「」をクリックします   作成 `Download` たアプリケーションのボタン。 ソフトウェアのステートメントをダウンロードする準備が整うまで、数分待つ必要がある場合があります。
- テキストファイルがダウンロードされます。 その内容をソフトウェアのステートメントとして使用します。

詳しくは、[ 動的なクライアント登録管理 ](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management) を参照してください。

### ダッシュボードの TVE Adobeへのアクセス権がない場合：

<tve-support@adobe.com> にチケットを送信します。 チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報をすべて含めてください。サポートチームの誰かが、ソフトウェアのステートメントを作成します。

## ソフトウェア ステートメントの使用方法 {#use}

ソフトウェア・ステートメントを取得したら、それをパラメータとして Access Enabler コンストラクタに渡す必要があります。 ソフトウェア ステートメントはリモートの場所でホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースしなくても、ソフトウェアのステートメントを簡単に取り消して変更できます。

## アプリケーションのカスタム URL スキームの生成 {#generating}

### ダッシュボードの TVE Adobeにアクセスできる場合：

- ブラウザーを開き、<https://experience.adobe.com/#/pass/authentication> に移動します。
- 「」セクション `Channels` 移動し、チャネルを選択します。
- 「」タブに移動 `Custom Schemes` ます。
- 「`Generate a new custom scheme`」をクリックします。
- アプリケーションに対して新しいカスタムスキームが生成されます。 例：`adbe.1JqxQsYhQOCIrwPjaooY8w://`
- 変更をサーバーにプッシュします。

### ダッシュボードの TVE Adobeへのアクセス権がない場合：

<tve-support@adobe.com> にチケットを送信します。 チャネル ID を含めてください。サポートチームの誰かがカスタムスキームを作成します。

## カスタムスキームの使用方法 {#use_custom}

アプリケーションの `info.plist` ファイルに、次のコードを追加します。

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
