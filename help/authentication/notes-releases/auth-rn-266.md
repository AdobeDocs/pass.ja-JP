---
title: Adobe Pass Authentication 2.66 リリースノート
description: Adobe Pass Authentication 2.66 リリースノート
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66 リリースノート {#authn-266-rn}

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-266}

* [ビルド番号](#build-number-266)
* [リリースの概要](#release-overview-266)

### ビルド番号 {#build-number-266}

Adobe Pass認証：adobe-pass-**2.66.0.1**

リリース日：**07/11/2023 - 07/13/2023**

### リリースの概要 {#release-overview-266}

このリリースでは、新しい REST API の内部アップデートを継続しました。

#### バグの修正

* SAML ベースの MVPD のログアウトフローを修正しました。ログアウトリクエストに RelayState パラメーターがありませんでした。 リリース後の設定のアップデートを対象に、影響を受ける MVPD のログアウトフローを復元します。
* SOAP認証エンドポイントの設定で SSL 証明書を更新できるようになりました。
* 一部の ESM レポートの「プログラマー」フィールドに間違ったデータが記録されるコーナーケースを修正しました。
