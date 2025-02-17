---
title: Adobe Pass Authentication Android 3.7.3 リリースノート
description: Adobe Pass Authentication Android 3.7.3 リリースノート
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Adobe Pass Authentication Android 3.7.3 リリースノート {#android-sdk-373-rn}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このページでは、このリリースの新機能、変更点および既知の問題について説明します。

## ビルド番号 {#build-number-373}

Adobe Pass認証：Android 3.7.3

リリース日：**09/19/2023**

## リリースの概要 {#release-overview-373}

* Android 14 および API レベル 34 をターゲットにしたアプリケーションをサポートする変更点
   * [Android 14 ランタイム登録済みブロードキャストの受信機 ](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported) に必要なフラグを追加します。
* Emulator API 32 以降でMVPD ログイン用に ChromeCustomTabs が開かない問題を修正しました
   * メモ：SDK &lt;3.7.3 でこの問題を回避するには、MVPDにログインする前に、エミュレーターでChrome アプリを開いて設定を完了する必要があります

## リリースパッケージ {#release-package-373}

Android SDK v3.7.3 は、（こちら [ からダウンロードでき ](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) す。
