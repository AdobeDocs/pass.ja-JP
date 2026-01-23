---
title: ヘッダー – AP デバイス識別子
description: REST API V2 - ヘッダー – AP デバイス識別子
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 13%

---

# ヘッダー – AP デバイス識別子 {#header-ap-device-identifier}

>[!NOTE]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>AP-Device-Identifier</b> 要求ヘッダーには、クライアント アプリケーションによって作成されたストリーミング デバイスの識別子が含まれています。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
   </tr>
   <tr>
      <td>ヘッダータイプ</td>
      <td>リクエストヘッダー</td>
   </tr>
   <tr>
      <td>標準</td>
      <td>不可</td>
   </tr>
</table>

## ディレクティブ {#directives}

<b>&lt;type></b>

デバイス識別子のタイプ。

以下に示すように、サポートされているタイプは 1 つだけです。

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">タイプ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>指紋</td>
      <td>
            デバイス識別子は、各デバイスに対してクライアントアプリケーションによって作成および管理される、安定した一意の識別子で構成されます。
            <br/>
            クライアントアプリケーションは、デバイス識別子を紛失または変更すると認証が無効になるので、永続ストレージにキャッシュする必要があります。 クライアントアプリケーションは、アプリケーションのアンインストール、再インストール、アップグレードなどのユーザーアクションによって引き起こされる値の変更を防ぐ必要があります。
      </td>
   </tr>
</table>


<b>&lt;identifier></b>

デバイス識別子の `Base64-encoded` 値。

## 例 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## クックブック {#cookbooks}

>[!IMPORTANT]
>
> ドキュメントリソースは、参照用に提供されています。
>
> ドキュメントリソースは完全なものではなく、プロジェクトで機能するために追加の変更が必要になる場合があります。
> 
> 実際の実装に関係なく、`AP-Device-Identifier` ヘッダーには、[ ディレクティブ ](#directives) の節で説明されているように書式設定された値が含まれている必要があります。

### ブラウザー {#browsers}

ブラウザーで実行されるデバイスの `AP-Device-Identifier` ヘッダーを作成するには、クライアントアプリケーションが、ブラウザー、デバイス、ユーザー固有のデータなどの使用可能なデータに基づいて、安定した一意の ID を計算する必要があります。

_（*） ブラウザーまたはデバイスのフィンガープリントのメカニズムを提供するライブラリまたはサービスを統合することをお勧めします。_

### モバイルデバイス {#mobile-devices}

#### iOSと iPadOS {#ios-ipados}

[iOSまたは iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) が稼働するデバイス用の `AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor) に関するApple開発者向けドキュメント。

_（*） OS で指定された値に対して SHA-256 ハッシュ関数を適用することをお勧めします。_

#### Android {#android}

[Android](https://developer.android.com/about/versions) を実行するデバイスの `AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* Android開発者向けドキュメント [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)。

_（*） OS で指定された値に対して SHA-256 ハッシュ関数を適用することをお勧めします。_

### テレビ接続デバイス {#tv-connected-devices}

#### tvOS {#tvos}

[tvOS](https://developer.apple.com/documentation/tvos-release-notes) を実行しているデバイスの `AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor) に関するApple開発者向けドキュメント。

_（*） OS で指定された値に対して SHA-256 ハッシュ関数を適用することをお勧めします。_

#### Fire OS {#fireos}

[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) を実行するデバイスの `AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* Android開発者向けドキュメント [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)。

_（*） OS で指定された値に対して SHA-256 ハッシュ関数を適用することをお勧めします。_

#### Roku OS {#rokuos}

[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) を実行するデバイスの `AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string) に関する Roku 開発者向けドキュメント。

_（*） OS で指定された値に対して SHA-256 ハッシュ関数を適用することをお勧めします。_

### その他 {#others}

ドキュメントで扱っていないデバイスプラットフォームの場合、デバイスの識別子は、利用可能なハードウェア ID （通常はデバイスのハードウェアマニュアルで指定されるもの）に関連付ける必要があります。

使用可能なハードウェア識別子がない場合は、クライアントアプリケーション属性に基づいて一意に生成された識別子を使用し、永続ストレージにキャッシュする必要があります。
