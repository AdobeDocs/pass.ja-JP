---
title: Android 10 アプリでの Enabler Android SDK シングルサインオン (SSO) へのアクセス
description: Android 10 アプリでの Enabler Android SDK シングルサインオン (SSO) へのアクセス
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Android 10 アプリでの Enabler Android SDK シングルサインオン (SSO) へのアクセス {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要

Adobe Pass認証を利用したアプリ間のシングルサインオン (SSO) は、Android OS を使用するデバイスで、Access Enabler Android SDK を通じて使用できます。 Android デバイスでシングルサインオン (SSO) を提供するには、Access Enabler Android SDK バージョン 3.2.1（最新）以前のバージョンで、Android ストレージ実装に保存された共有データベースファイルを使用します。このファイルは、すべてのAdobe Pass Authentication を利用したアプリでアクセスできます。

ただし、最新の Android 10 リリースのGoogleでは、「ユーザーがファイルをより制御し、ファイルを整理するために、Android 10（API レベル 29）以降を対象とするアプリは、デフォルトで外部ストレージデバイスまたはスコープストレージにアクセスできます。 このようなアプリは、アプリ固有のディレクトリのみを表示できます `\[...\]`&quot;. これらの Android 10 ストレージの変更に関する詳細については、 [Android 向けのデータおよびファイルストレージに関するドキュメント](https://developer.android.com/training/data-storage/files/external-scoped).

これらの変更の結果、Access Enabler Android バージョンで提供されるシングルサインオン (SSO) が変更されました。 **3.2.1 SDK（最新）** および以前のバージョンは、次の節で説明するように、Android 10 デバイスで影響を受ける可能性があります。

詳しくは、 [Roku SSO の概要](/help/authentication/roku-sso-overview.md).

## 動作

アプリの **target SDK レベル** または **android:requestLegacyExternalStorage** manifest 属性は、Access Enabler Android バージョン 3.2.1 SDK（最新）および以前のバージョンで提供されるシングルサインオン (SSO) は、現在、次のように動作します。

- アプリのターゲット **Android 9（API レベル 28）** またはそれ以下 **-\>** シングルサインオン (SSO) **動く**
- アプリのターゲット **Android 10** **（API レベル 29）** および **設定** 値 **requestLegacyExternalStorage を true に設定** をアプリのマニフェストファイルに追加します。 **-\>** シングルサインオン (SSO) **動く**
- アプリのターゲット **Android 10** **（API レベル 29）** および **未設定** 値 **requestLegacyExternalStorage を true に設定** をアプリのマニフェストファイルに追加します。 **-\>** シングルサインオン (SSO) **動作しない**


>[!TIP]
>
> Adobe Pass Authentication Access Enabler Android SDK がスコープストレージとの互換性を完全に保つ前に、アプリのターゲット SDK レベルまたは requestLegacyExternalStorage マニフェスト属性（パブリックで説明）に基づいて、一時的にオプトアウトできます [Android ドキュメント](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
