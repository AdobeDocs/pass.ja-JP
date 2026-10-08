---
title: iOS/tvOS アプリケーションの登録
description: iOS/tvOS アプリケーションの登録
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 0%
---

# （レガシー） iOS/tvOS アプリケーションの登録 {#iostvos-application-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#Intro}

IOS/tvOS AccessEnabler SDKのバージョン 3.0以降、Adobeのサーバーを使用して認証メカニズムを変更しています。 公開鍵と秘密鍵を使用して依頼者IDに署名する代わりに、SDKがサーバーに対して行うすべての呼び出しに後で使用されるアクセストークンを取得するために使用できるソフトウェアステートメント文字列の概念を導入します。 ソフトウェアステートメントに加えて、アプリケーションのカスタム URL スキームも必要になります。

詳しくは、[動的クライアント登録の概要](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)を参照してください。

## ソフトウェアに関する声明とは？ {#Soft_state}

ソフトウェアステートメントは、アプリケーションに関する情報を含むJWT トークンです。 各アプリケーションには、Adobe システム内のアプリケーションを識別するためにサーバーが使用する一意のソフトウェアステートメントが必要です。 AccessEnabler SDKを初期化する際にソフトウェア ステートメントを渡す必要があり、それを使用してアプリケーションをAdobeに登録します。 登録時に、SDKはクライアント IDとアクセストークンの取得に使用されるクライアント秘密鍵を受け取ります。 SDKが当社のサーバーに対して行う呼び出しには、有効なアクセストークンが必要です。 SDKは、アプリケーションの登録、アクセストークンの取得と更新を担当します。

**注：** ソフトウェア ステートメントはアプリ固有であり、同じソフトウェア ステートメントを複数のアプリケーションで使用することはできません。 プログラマレベルのソフトウェアステートメントも同じである、つまり、単一チャネルでもマルチチャネルでも、単一のアプリケーションにのみ使用できます。 この制限は、カスタムスキームにも適用されます。

## ソフトウェアステートメントを取得するには？ {#obtain}

### AdobeのTVE ダッシュボードにアクセスできる場合：

- ブラウザーを開き、<https://experience.adobe.com/#/pass/authentication>に移動します
- `Channels` セクションに移動し、チャネルを選択します。
- 「`Registered Applications`」タブに移動します。
- `Add new application`をクリックします。
- アプリケーションの名前とバージョンを入力し、使用可能なプラットフォームを選択します。 iOS/tvOSです。
- 変更をサーバーにプッシュし、チャネルの「登録済みアプリケーション」タブに戻ります。
- 登録されたすべてのアプリケーションのリストが表示されます。 作成したアプリケーションの`Download` ボタンをクリックします。 ソフトウェアステートメントをダウンロードする準備が整うまで、数分待つ必要がある場合があります。
- テキストファイルがダウンロードされます。 ソフトウェアステートメントとしてコンテンツを使用します。

詳しくは、[動的クライアント登録管理](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)を参照してください。

### AdobeのTVE ダッシュボードにアクセスできない場合：

<tve-support@adobe.com>へのチケット送信。 チャネル、アプリケーション名、バージョン、プラットフォームなど、必要な情報をすべて含めてください。また、サポートチームの担当者がソフトウェアに関する声明を作成します。

## ソフトウェアステートメントの使用方法 {#use}

ソフトウェア ステートメントを取得したら、Access Enabler コンストラクターのパラメーターとして渡す必要があります。 ソフトウェアステートメントをリモートの場所でホストすることをお勧めします。 これにより、アプリケーションの新しいバージョンをリリースしなくても、ソフトウェアのステートメントを簡単に取り消して変更できます。

## アプリケーションのカスタム URL スキームの生成 {#generating}

### AdobeのTVE ダッシュボードにアクセスできる場合：

- ブラウザーを開き、<https://experience.adobe.com/#/pass/authentication>に移動します
- `Channels` セクションに移動し、チャネルを選択します。
- 「`Custom Schemes`」タブに移動します。
- `Generate a new custom scheme`をクリックします。
- アプリケーション用に新しいカスタムスキームが生成されます。 例：`adbe.1JqxQsYhQOCIrwPjaooY8w://`
- 変更をサーバーにプッシュします。

### AdobeのTVE ダッシュボードにアクセスできない場合：

<tve-support@adobe.com>へのチケット送信。 チャネル IDを含めてください。サポートチームからカスタムスキームを作成します。

## カスタムスキームの使用方法 {#use_custom}

アプリケーションの`info.plist` ファイルに次のコードを追加します。

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
