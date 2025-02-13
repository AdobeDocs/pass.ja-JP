---
title: Adobe Pass Authentication 2.66 リリースノート
description: Adobe Pass Authentication 2.66 リリースノート
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66 リリースノート {#authn-266-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-266}

* [ビルド番号](#build-number-266)
* [リリースの概要](#release-overview-266)

### ビルド番号 {#build-number-266}

Adobe Pass認証：adobe-pass-**2.66.0.1**
リリース日：**07/11/2023 - 07/13/2023**

### リリースの概要 {#release-overview-266}

このリリースでは、新しい REST API の内部アップデートを継続しました。

#### バグの修正 {#release-overview-bugfixes-266}

* SAML ベースの MVPD のログアウトフローを修正しました。ログアウトリクエストに RelayState パラメーターがありませんでした。 リリース後の設定のアップデートを対象に、影響を受ける MVPD のログアウトフローを復元します。
* SOAP認証エンドポイントの設定で SSL 証明書を更新できるようになりました。
* 一部の ESM レポートの「プログラマー」フィールドに間違ったデータが記録されるコーナーケースを修正しました。
