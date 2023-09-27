---
title: Adobe Pass Authentication 2.66 リリースノート
description: Adobe Pass Authentication 2.66 リリースノート
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66 リリースノート {#authn-266-rn}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバー側および Web クライアント {#server-side-web-clients-266}

* [ビルド番号](#build-number-266)
* [リリースの概要](#release-overview-266)

### ビルド番号 {#build-number-266}

Adobe Pass認証：adobe-pass-**2.66.0.1**
リリース日： **07/11/2023 - 07/13/2023**

### リリースの概要 {#release-overview-266}

このリリースでは、新しい REST API の内部アップデートを継続しました。

#### バグの修正 {#release-overview-bugfixes-266}

* ログアウトリクエストに RelayState パラメーターがない SAML ベースの MVPD のログアウトフローを修正しました。 リリース後に設定の更新をターゲットにして、影響を受ける MVPD のログアウトフローを復元します。
* SOAP 認証エンドポイントの設定で SSL 証明書を更新する機能が追加されました。
* 一部の ESM レポートの「Programmer」フィールドに誤ったデータが記録されるコーナーケースを修正しました。
