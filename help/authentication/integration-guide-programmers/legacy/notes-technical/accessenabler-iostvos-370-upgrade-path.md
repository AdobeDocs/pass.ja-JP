---
title: AccessEnabler iOS/tvOS 3.7.0 アップグレードパス
description: AccessEnabler iOS/tvOS 3.7.0 アップグレードパス
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%
---
# （レガシー） AccessEnabler iOS/tvOS 3.7.0 アップグレードパス {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

[新しいAccessEnabler バージョン 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md)からのキーチェーンストレージの変更は、AccessEnabler バージョン 3.7.0より前のキーチェーンストレージの実装と互換性がありません。

新しいAccessEnabler バージョン 3.7.0を採用した1つのアプリケーションのアップグレードパスは、以前のバージョンのキーチェーンストレージからすべてのトークンを移行します。 したがって、エンドユーザー&#x200B;**は、AccessEnabler フレームワークの更新プロセス中に認証/承認セッション**&#x200B;を失うことはありません。

## 既知の制限

以下で説明する制限事項の中には、実装者が遭遇する可能性があります。


1. 通常（Adobe）のSSOは、同じベンダーによって開発されたアプリケーションであっても、AccessEnabler バージョン 3.7.0を使用する1つのアプリケーションと、AccessEnabler バージョン 3.7.0より前の3.7.0を使用する1つのアプリケーションの間では機能しません。

   >[!IMPORTANT]
   >
   >* システムレベル（Apple）のSSOは影響を受けません。
   >
   >* 通常（Adobe）のSSOは、両方のアプリケーションが同じベンダーによって開発され、AccessEnablerのバージョンが3.7.0未満の場合も引き続き機能します。
   >
   >* 通常（Adobe）のSSOは、両方のアプリケーションが同じベンダーによって開発され、AccessEnabler バージョン 3.7.0を使用している場合に機能します。


1. AccessEnabler バージョン 3.7.0を使用して1つのアプリケーションを下位バージョンのAccessEnablerにダウングレードする場合、新しく生成されたトークンは移行されません。 したがって、エンドユーザーは、認証/認証セッションを期待せずに失う可能性があります。

   >[!IMPORTANT]
   >
   >* システムレベル（Apple） SSOで認証されたエンドユーザーは影響を受けません。
   >* AccessEnabler バージョン 3.7.0を使用して新しいアプリケーションに更新する前に既に認証されたエンドユーザーは影響を受けません。

1. AccessEnabler バージョン 3.7.0を使用して1つのアプリケーションを下位バージョンのAccessEnablerにダウングレードする場合、削除されたトークンは確認されません。 したがって、エンドユーザーは、認証/認証セッションの存在を期待せずに経験する可能性があります。
