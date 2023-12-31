---
title: AccessEnabler iOS/tvOS 3.7.0 のアップグレードパス
description: AccessEnabler iOS/tvOS 3.7.0 のアップグレードパス
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# AccessEnabler iOS/tvOS 3.7.0 のアップグレードパス {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

キーチェーンストレージの変更 [新しい AccessEnabler バージョン 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) は、AccessEnabler バージョン 3.7.0 より前のキーチェーンストレージ実装とは互換性がありません。

新しい AccessEnabler バージョン 3.7.0 を採用する 1 つのアプリケーションのアップグレードパスにより、キーチェーンストレージの以前のバージョンからすべてのトークンが移行されます。 したがって、エンドユーザーは **認証/承認セッションを失うことはありません** AccessEnabler フレームワークの更新プロセス中。

## 既知の制限事項

以下で説明するいくつかの制限が、実装者によって発生する可能性があります。


1. 通常 (Adobe)SSO は、同じベンダーが開発したアプリケーションでも、AccessEnabler バージョン 3.7.0 を使用する 1 つのアプリケーションと、3.7.0 より前のバージョンを使用する 1 つのアプリケーションとの間では機能しません。

   >[!IMPORTANT]
   >
   >* システムレベル (Apple)SSO は影響を受けません。
   >
   >* 通常 (Adobe)SSO は、両方のアプリケーションが同じベンダーによって開発され、3.7.0 未満の AccessEnabler バージョンを使用する場合にも引き続き機能します。
   >
   >* 通常 (Adobe)SSO は、両方のアプリケーションが同じベンダーによって開発され、AccessEnabler バージョン 3.7.0 を使用する場合に機能します。


1. AccessEnabler バージョン 3.7.0 を使用して 1 つのアプリケーションを AccessEnabler の古いバージョンにダウングレードする場合、新しく生成されたトークンは移行されません。 したがって、エンドユーザーは、予期せずに認証/承認セッションが失われる可能性があります。

   >[!IMPORTANT]
   >
   >* システムレベル (Apple)SSO で認証されたエンドユーザーは影響を受けません。
   >* AccessEnabler バージョン 3.7.0 を使用して新しいアプリケーションに更新する前に既に認証済みのエンドユーザーは影響を受けません。

1. AccessEnabler バージョン 3.7.0 を使用して 1 つのアプリケーションを AccessEnabler の古いバージョンにダウングレードする場合、削除されたトークンは確認されません。 したがって、エンドユーザーは、認証/承認セッションが予期せず存在する可能性があります。
