---
title: Adobe Pass Authentication 2.64 リリースノート
description: Adobe Pass Authentication 2.64 リリースノート
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64 リリースノート {#authn-264-rn}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## サーバー側および Web クライアント {#ss-web-clients}

* [ビルド番号](#build-no-264)
* [新機能](#new-featres-264)
* [バグの修正](#bug-fixes-264)

### ビルド番号 {#build-no-264}

Adobe Pass認証：adobe-pass-**2.64**

リリース日： **11/08/2022 - 11/10/2022**

### 新機能 {#new-featres-264}

* サーバの応答時間の短縮を目的としたインフラストラクチャの更新により、システム全体のパフォーマンスが向上します。
* 新しいプラットフォーム識別メカニズムの改善。
* SAML アサーションに「in_response_to」パラメーターがない MVPD からの未承諾認証応答をブロックする機能。

### バグの修正 {#bug-fixes-264}

* 一部のレガシー TempPass トークンの形式が正しくない問題を修正しました。
* 2 画面目の認証フローに関するマイナーな問題に対処しました。
