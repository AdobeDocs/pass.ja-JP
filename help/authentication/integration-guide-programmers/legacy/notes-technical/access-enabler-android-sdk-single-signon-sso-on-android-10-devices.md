---
title: Android 10 アプリケーションでのAndroid SDK シングルサインオン（SSO）へのアクセス
description: Android 10 アプリケーションでのAndroid SDK シングルサインオン（SSO）へのアクセス
exl-id: dedade15-c451-4757-b684-d3728e11dd87
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%
---
# （従来）Android 10 アプリでのAndroid SDK シングルサインオン （SSO）へのアクセス {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要

Adobe Pass認証を活用したアプリ間のシングルサインオン（SSO）は、Android OSを使用しているデバイスでは、Access Enabler Android SDKを通じて利用できます。 Android デバイスでシングルサインオン（SSO）を提供するために、Access Enabler Android SDK バージョン 3.2.1 （最新）および以前のバージョンでは、Android ストレージ実装に保存された共有データベースファイルを使用します。このファイルには、すべてのAdobe Pass Authentication搭載アプリからアクセスできます。

ただし、最新のAndroid 10 リリースのGoogleでは、「ファイルをより詳細に制御し、ファイルの混乱を制限するため、Android 10 （API レベル 29）以降をターゲットとするアプリには、デフォルトで外部ストレージデバイスまたはスコープストレージへのスコープアクセスが許可されます。 このようなアプリは、アプリ固有のディレクトリ `\[...\]`&quot;のみを表示できます。 これらのAndroid 10 ストレージの変更に関する詳細については、[Androidのデータおよびファイルストレージに関するドキュメント &#x200B;](https://developer.android.com/training/data-storage/files/external-scoped)を参照してください。

これらの変更の結果、次の節で説明するように、Access Enabler Android バージョン **3.2.1 SDK（最新）**&#x200B;および以前のバージョンで提供されるシングルサインオン（SSO）が、Android 10 デバイスで影響を受ける可能性があります。

## 動作

アプリの&#x200B;**[!UICONTROL target SDK level]**&#x200B;または&#x200B;**android:requestLegacyExternalStorage**&#x200B;の使用に応じて、Access Enabler Android バージョン 3.2.1 SDK（最新）および以前のバージョンで提供されるシングルサインオン（SSO）は、現在、次のように動作します。

- アプリのターゲットは&#x200B;**Android 9 （API レベル 28）**&#x200B;以下&#x200B;**-\>**&#x200B;のシングルサインオン（SSO） **です**
- お使いのアプリは&#x200B;**Android 10** **（API レベル 29）**&#x200B;をターゲットとしており、**アプリのマニフェストファイル**-\>**の** requestLegacyExternalStorageの値を&#x200B;**に設定**&#x200B;します&#x200B;**&#x200B;**
- お使いのアプリは&#x200B;**Android 10** **（API レベル 29）**&#x200B;をターゲットにしており、**が** アプリのマニフェスト ファイル **-\>**&#x200B;の値を&#x200B;**requestLegacyExternalStorageからtrue**&#x200B;に設定していません。シングル サインオン （SSO） **は機能しません**

>[!TIP]
>
> Adobe Pass Authentication Access Enabler Android SDKがスコープ付きストレージと完全に互換性を持つ前に、アプリのターゲット SDK レベルまたはrequestLegacyExternalStorage マニフェスト属性に基づいて、一時的にオプトアウトできます。詳しくは、公開[Android ドキュメント &#x200B;](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage)を参照してください。
