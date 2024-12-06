---
title: Adobe Pass Authentication 2.64 リリースノート
description: Adobe Pass Authentication 2.64 リリースノート
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64 リリースノート {#authn-264-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバーサイドと Web クライアント {#ss-web-clients}

* [ビルド番号](#build-no-264)
* [新機能](#new-featres-264)
* [バグの修正](#bug-fixes-264)

### ビルド番号 {#build-no-264}

Adobe Pass認証：adobe-pass-**2.64**

リリース日：2022 年 **月 11 日（PT）～2022 年 11 月 10 日（PT）**

### 新機能 {#new-featres-264}

* インフラストラクチャのアップデート。サーバーの応答時間の短縮とシステムの全体的なパフォーマンスの向上を目的としています。
* 新しいプラットフォーム識別メカニズムの改善。
* SAML アサーションに「in_response_to」パラメーターがない MVPD からの未承諾認証応答をブロックする機能。

### バグの修正 {#bug-fixes-264}

* 一部の従来の TempPass トークンの誤った形式を修正しました。
* 2 番目の画面の認証フローの小さな問題を修正しました。
