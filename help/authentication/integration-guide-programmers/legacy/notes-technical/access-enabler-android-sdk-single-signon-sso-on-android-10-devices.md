---
title: Android 10 アプリでのAndroid SDK シングルサインオン （SSO）へのアクセス イネーブラ
description: Android 10 アプリでのAndroid SDK シングルサインオン （SSO）へのアクセス イネーブラ
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# （従来）Android 10 アプリでの Access Enabler Android SDKのシングルサインオン（SSO） {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要

Adobe Pass Authentication を利用したアプリ間のシングルサインオン（SSO）は、Android OS を使用しているデバイスでは、Access Enabler Android SDKによって利用できます。 Android デバイスでシングルサインオン （SSO）を提供するために、Access Enabler Android SDK バージョン 3.2.1 （最新）およびそれ以前のバージョンでは、Adobe Pass ストレージ実装に保存された共有データベースファイルを使用します。このファイルには、Android認証を利用したすべてのアプリからアクセスできます。

ただし、最新のAndroid 10 リリースのGoogleでは、「ファイルをより詳細に制御し、ファイルの混乱を制限するために、Android 10 （API レベル 29）以降をターゲットにするアプリには、デフォルトで外部ストレージデバイス（スコープストレージ）へのスコープアクセスが許可されるようになりました。 このようなアプリは、アプリ固有のディレクトリ `\[...\]`」のみを表示できます。 Android 10 のストレージに関するこれらの変更点について詳しくは、[Androidのデータおよびファイルストレージのドキュメント ](https://developer.android.com/training/data-storage/files/external-scoped) を参照してください。

これらの変更の結果、Access Enabler Android バージョン **3.2.1 SDK（最新）で提供されるシングル サインオン（SSO）と以前のバージョンは** 次のセクションで説明するようにAndroid 10 デバイスに影響を与える可能性があります。

## 動作

アプリの **[!UICONTROL target SDK level]** や **android:requestLegacyExternalStorage** マニフェスト属性に応じて、Access Enabler Android バージョン 3.2.1 SDK （最新）および以前のバージョンが提供するシングル サインオン （SSO）は、現在、次のように動作します。

- アプリのターゲットは **Android 9 （API レベル 28）** 以下 **-\>** シングルサインオン （SSO） **です**
- アプリは **Android 10** **（API レベル 29）をターゲットにしており** アプリのマニフェスト ファイル **-\>** Single Sign-On （SSO） **で** requestLegacyExternalStorage の値を true に設定 **** し **動作し** す。
- アプリは **Android 10** **（API レベル 29）を対象としていますが** アプリのマニフェスト ファイル **-\>** Single Sign-On （SSO） **で** requestLegacyExternalStorage の値を true に設定し **い****ので、機能しません**

>[!TIP]
>
> Adobe Pass Authentication Access Enabler Android SDKがスコーピングされたストレージと完全に互換性を持つ前に、公開 [Android ドキュメント ](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) に記載されているように、アプリのターゲット SDK レベルまたは requestLegacyExternalStorage マニフェスト属性に基づいて一時的にオプトアウトできます。
