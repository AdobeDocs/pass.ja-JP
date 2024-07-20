---
title: AccessEnabler iOS/tvOS 3.7.0 アップグレード・パス
description: AccessEnabler iOS/tvOS 3.7.0 アップグレード・パス
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# AccessEnabler iOS/tvOS 3.7.0 アップグレード・パス {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

[ 新しい AccessEnabler バージョン 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) からのキーチェーン・ストレージの変更は、AccessEnabler バージョン 3.7.0 以前からのキーチェーン・ストレージの実装と互換性がありません。

新しい AccessEnabler バージョン 3.7.0 を採用する 1 つのアプリケーションのアップグレード・パスでは、すべてのトークンが以前のバージョンの Keychain ストレージから移行されます。 そのため、AccessEnabler フレームワークの更新プロセス中にエンド・ユーザーに認証/承認セッションの消失が発生することは **ありません**。

## 既知の制限事項

実装者によっては、次に示すいくつかの制限事項が発生する場合があります。


1. 通常の（Adobe） SSO は、同じベンダーで開発されたアプリケーションであっても、AccessEnabler バージョン 3.7.0 を使用している 1 つのアプリケーションと、AccessEnabler バージョン 3.7.0 より前のバージョンを使用している 1 つのアプリケーションでは動作しません。

   >[!IMPORTANT]
   >
   >* システムレベル（Apple）の SSO は影響を受けません。
   >
   >* 両方のアプリケーションが同じベンダーによって開発され、AccessEnabler のバージョンが 3.7.0 より前の場合は、通常の（Adobe） SSO が引き続き機能します。
   >
   >* 両方のアプリケーションが同じベンダーによって開発され、AccessEnabler バージョン 3.7.0 を使用している場合は、通常の（Adobe） SSO が動作します。


1. AccessEnabler バージョン 3.7.0 を使用しているアプリケーションを下位バージョンの AccessEnabler にダウングレードすると、新しく生成されたトークンは移行されません。 そのため、エンドユーザーが予期せずに認証/承認セッションが失われる可能性があります。

   >[!IMPORTANT]
   >
   >* システムレベル（Apple） SSO で認証されたエンドユーザーは影響を受けません。
   >* AccessEnabler バージョン 3.7.0 を使用して新しいアプリケーションに更新する前に認証済みのエンド・ユーザーは影響を受けません。

1. AccessEnabler バージョン 3.7.0 を使用しているアプリケーションを下位バージョンの AccessEnabler にダウングレードすると、削除されたトークンは確認されません。 そのため、エンドユーザーが予期せず、認証/承認セッションが存在する可能性があります。
