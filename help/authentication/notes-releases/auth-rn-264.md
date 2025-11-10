---
title: Adobe Pass Authentication 2.64 リリースノート
description: Adobe Pass Authentication 2.64 リリースノート
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64 リリースノート {#authn-264-rn}

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#server-side-web-clients-264}

* [ビルド番号](#build-number-264)
* [リリースの概要](#release-overview-264)

### ビルド番号 {#build-number-264}

Adobe Pass認証：adobe-pass-**2.64**

リリース日：2022 年 **月 11 日（PT）～2022 年 11 月 10 日（PT）**

### リリースの概要 {#release-overview-264}

* インフラストラクチャのアップデート。サーバーの応答時間の短縮とシステムの全体的なパフォーマンスの向上を目的としています。
* 新しいプラットフォーム識別メカニズムの改善。
* SAML アサーションに「in_response_to」パラメーターがない MVPD からの未承諾認証応答をブロックする機能。

#### バグの修正

* 一部の従来の TempPass トークンの誤った形式を修正しました。
* 2 番目の画面の認証フローの小さな問題を修正しました。
