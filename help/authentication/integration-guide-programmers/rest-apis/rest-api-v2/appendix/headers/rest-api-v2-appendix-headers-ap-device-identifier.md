---
title: Header - AP-Device-Identifier
description: REST API V2 - ヘッダー – AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%
---
# Header - AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>AP-Device-Identifier</b> リクエストヘッダーには、クライアントアプリケーションによって作成されたストリーミングデバイス識別子が含まれています。

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
      <td>Standard</td>
      <td>いいえ</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;type></b>

デバイス識別子タイプ。

次に示すように、サポートされているタイプは1つだけです。

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">タイプ</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>指紋</td>
      <td>
            デバイス識別子は、各デバイスに対してクライアントアプリケーションによって作成および管理される安定した一意の識別子で構成されます。
            <br/>
            クライアントアプリケーションは、デバイス識別子を失ったり変更したりすると認証が無効になるため、永続ストレージにキャッシュする必要があります。 クライアントアプリケーションは、アプリケーションのアンインストール、再インストール、アップグレードなどのユーザーアクションによって生じる値の変更を防ぐ必要があります。
      </td>
   </tr>
</table>


<b>&lt;識別子></b>

デバイス IDの`Base64-encoded`値。

## 例 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
>
> ドキュメントのリソースは、参照目的で提供されます。
>
> ドキュメントリソースは完全なものではなく、プロジェクトで作業するには追加の変更が必要になる場合があります。
> 
> 実際の実装に関係なく、`AP-Device-Identifier` ヘッダーには、[ ディレクティブ ](#directives) セクションで説明されているようにフォーマットされた値を含める必要があります。

### ブラウザー {#browsers}

ブラウザーで実行されているデバイスの`AP-Device-Identifier` ヘッダーを構築するには、クライアントアプリケーションで、ブラウザー、デバイス、ユーザー固有のデータなどの利用可能なデータに基づいて、安定した一意の識別子を計算する必要があります。

_（*） ブラウザーまたはデバイスのフィンガープリント メカニズムを提供するライブラリまたはサービスを統合することをお勧めします。_

### モバイルデバイス {#mobile-devices}

#### iOSとiPadOS {#ios-ipados}

[iOSまたはiPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)を実行しているデバイスの`AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)のApple開発者向けドキュメント。

_（*）指定されたOS値に対してSHA-256 ハッシュ関数を適用することをお勧めします。_

#### Android {#android}

[Android](https://developer.android.com/about/versions)を実行しているデバイスの`AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)のAndroid開発者向けドキュメント。

_（*）指定されたOS値に対してSHA-256 ハッシュ関数を適用することをお勧めします。_

### TV接続デバイス {#tv-connected-devices}

#### tvOS {#tvos}

[tvOS](https://developer.apple.com/documentation/tvos-release-notes)を実行しているデバイスの`AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)のApple開発者向けドキュメント。

_（*）指定されたOS値に対してSHA-256 ハッシュ関数を適用することをお勧めします。_

#### Fire OS {#fireos}

[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)を実行しているデバイスの`AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)のAndroid開発者向けドキュメント。

_（*）指定されたOS値に対してSHA-256 ハッシュ関数を適用することをお勧めします。_

#### Roku OS {#rokuos}

[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)を実行しているデバイスの`AP-Device-Identifier` ヘッダーを作成するには、次のドキュメントを参照してください。

* [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string)のRoku開発者ドキュメント。

_（*）指定されたOS値に対してSHA-256 ハッシュ関数を適用することをお勧めします。_

### その他 {#others}

ドキュメントに記載されていないデバイスプラットフォームの場合、デバイス識別子は、通常、デバイスのハードウェアマニュアルで指定されている、利用可能なハードウェア識別にリンクする必要があります。

使用可能なハードウェア識別子がない場合は、クライアントアプリケーションの属性に基づいて一意に生成された識別子を使用し、永続ストレージにキャッシュする必要があります。
