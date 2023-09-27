---
title: Adobe Pass Authentication Android 3.7.3 リリースノート
description: Adobe Pass Authentication Android 3.7.3 リリースノート
source-git-commit: 284ae59837fd42bc6b711b3041eb6b7ef79aaed3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication Android 3.7.3 リリースノート {#android-sdk-373-release-notes}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-no-android-sdk-373}

Adobe Pass認証：Android 3.7.3

リリース日： 09/19/2023



## リリースの概要 {#overview-android-sdk-373}

* Android 14 および API レベル 34 をターゲットとするアプリケーションをサポートする変更
   * 次の条件で必要なフラグを追加 [Android 14 ランタイム登録ブロードキャストレシーバー](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* エミュレーター API 32 以降で MVPD ログイン用に ChromeCustomTabs が開かない問題を修正しました。
   * 注意： SDK でこの問題を回避するには、エミュレーターで Chrome アプリを開き、MVPD ログインを試みる前に設定を完了する必要があります。


## リリースパッケージ {#rel-pkg-android373}

Android SDK v3.7.3 は、 [ここ](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
