---
title: クライアント情報（デバイス、接続、およびアプリケーション）を渡す
description: クライアント情報（デバイス、接続、およびアプリケーション）を渡す
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# クライアント情報（デバイス、接続、およびアプリケーション）を渡す {#pass-client-info}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


## 範囲 {#pass-client-info-scope}

このドキュメントでは、クライアント情報（デバイス、接続、アプリケーション）をプログラマーアプリケーションからAdobe Pass Authentication REST API または SDK に渡すための詳細とクックブックを集計します。

クライアント情報を提供するメリットは次のとおりです。

* HBA をサポートできる一部のデバイスタイプと MVPD の場合に、Home Base Authentication(HBA) を適切に有効にする機能。
* 一部のデバイスタイプで TTL を適切に適用する機能（例：TV 接続デバイスでの認証セッションの TTL を長く設定する）。
* Entitlement Service Monitoring(ESM) を使用して、デバイスの種類をまたいで分類レポートでビジネス指標を適切に集計する機能。
* 様々なビジネス・ルールを適切に適用する機能のブロックを解除します ( 例： 最適化 ) を使用できます。

## 概要 {#pass-client-info-overview}

クライアント情報は、次の要素で構成されます。

* **デバイス** ユーザーが Programmer コンテンツを使用しようとするデバイスのハードウェアおよびソフトウェア属性に関する情報。
* **接続** ユーザーがAdobe Pass Authentication サービスや Programmer サービス（サーバー間実装など）に接続する際の、デバイスの接続属性に関する情報。
* **アプリ** ユーザーが Programmer コンテンツを使用しようとする登録済みアプリケーションに関する情報。

クライアント情報は、次の表に示すキーで構築された JSON オブジェクトです。

>[!NOTE]
>
>次の **keys** が **必須** クライアント情報 JSON オブジェクトに送信される **モデル**, **osName**.
>
>次のキーは、 **制限** 値： `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | キー | 制限 | 説明 | 可能な値 |
|---|---|---|---|---|
|            | primaryHardwareType | #はい | デバイスの主要なハードウェアタイプ。 | #値は制限されています： Camera DataCollectionTerminal Desktop EmbeddedNetworkModule eReader GamesConsole GeolocationTracker Glasses MediaPlayer MobilePhone PaymentTerminal PluginMonSetTopBox TV タブレット Wiressot Wristwatch Unknown |
| #mandatory | モデル | いいえ | デバイスのモデル名。 | 例： iPhone、SM-G930V、AppleTV など。 |
|            | version | いいえ | デバイスのバージョン。 | 例： 2.0.1 など |
|            | 製造元 | いいえ | デバイスの製造会社または組織。 | 例：Samsung、LG、ZTE、Huawei、Motorola、Appleなど。 |
|            | ベンダー | いいえ | デバイスの販売会社/組織。 | 例：Apple、Samsung、LG、Googleなど。 |
| #mandatory | osName | #はい | デバイスのオペレーティングシステム (OS) 名。 | #値は制限されています： Android Chrome OS Linux Mac OS X OpenBSD Roku OS Windows iOS tvOS webOS |
|            | osFamily | はい | デバイスのオペレーティングシステム (OS) グループ名。 | #値は制限されています： Android BSD Linux PlayStation OS Roku OS Symbian Tizen Windows iOS macOS tvOS webOS |
|            | osVendor | いいえ | デバイスのオペレーティングシステム (OS) サプライヤー。 | Amazon Apple Google LG Microsoft Mozilla Nintendo Nokia Roku Samsung Sony Tizen Project |
|            | osVersion | いいえ | デバイスのオペレーティングシステム (OS) のバージョン。 | 例： 10.2、9.0.1 など。 |
|            | browserName | #はい | ブラウザーの名前。 | #値は制限されています： Android ブラウザ Chrome Edge Firefox Internet Explorer Opera Safari SeaMonkey Symbian ブラウザ |
|            | browserVendor | #はい | ブラウザーの組織（会社）。 | #値は制限されます： Amazon Apple Google Microsoft Motorola Mozilla Nentendo Nokia Samsung Sony Ericsson |
|            | browserVersion | いいえ | デバイスのブラウザーのバージョン。 | 例： 60.0.3112 |
|            | userAgent | いいえ | デバイスのユーザーエージェント。 | 例：Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | いいえ | デバイスの物理画面の幅。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | いいえ | デバイスの物理的な画面の高さ。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | いいえ | デバイスの物理画面のピクセル密度。 | 例： 294 |
|            | diagonalScreenSize | いいえ | デバイスの物理画面の対角線の寸法（インチ単位）。 | 例： 5.5、10.1 |
|            | connectionIp | いいえ | HTTP リクエストの送信に使用されるデバイスの IP。 | 例： 8.8.4.4 |
|            | connectionPort | いいえ | HTTP リクエストの送信に使用されるデバイスのポート。 | 例： 53124 |
|            | connectionType | いいえ | ネットワーク接続のタイプ。 | 例： WiFi、LAN、3G、4G、5G |
|            | connectionSecure | #はい | ネットワーク接続のセキュリティ状態。 | #値は制限されます：true — セキュアネットワークの場合は false — パブリックホットスポットの場合は |
|            | applicationId | いいえ | アプリケーションの一意の ID。 | 例： CNN |

## API リファレンス {#api-ref}

この節では、Adobe Pass Authentication REST API または SDK を使用する際にクライアント情報を処理する API を示します。

### REST API {#rest-api}

Adobe Pass Authentication Services では、次の方法でクライアント情報を受信できます。

* As a **header: &quot;X-Device-Info&quot;**
* As a **クエリパラメータ： &quot;device_info&quot;**
* As a **post パラメータ： &quot;device_info&quot;**

>[!IMPORTANT]
>
>3 つのシナリオのすべてで、ヘッダーまたはパラメーターのペイロードは、 **Base64 エンコード済みと URL エンコード済み**.

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDK は、上書きされない限りAdobe Pass Authentication Services に渡されるクライアント情報 JSON オブジェクトをデフォルトで構築します。

AccessEnabler JavaScript SDK は、 **上書きのみ** クライアント情報の JSON オブジェクトから、 [setRequestor](/help/authentication/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))&#39;s *applicationId* options パラメーター。

>[!CAUTION]
>
>The `applicationId` パラメータ値は、プレーンテキストの文字列値である必要があります。
>プログラマーアプリケーションが applicationId を渡す場合、残りのクライアント情報キーは AccessEnabler JavaScript SDK によって引き続き計算されます。

#### iOS/tvOS SDK {#ios-tvos-sdk}

AccessEnabler iOS/tvOS SDK は、上書きされない限りAdobe Pass Authentication Services に渡されるクライアント情報 JSON オブジェクトをデフォルトで構築します。

AccessEnabler iOS/tvOS SDK は、 **全体を上書きする** クライアント情報 JSON オブジェクトを [setOptions](/help/authentication/iostvos-sdk-api-reference.md#setoptions)&#39;s device_info パラメーター。

>[!CAUTION]
>
>The *device_info* パラメータ値は必ず指定してください **Base64 エンコード済み** *NSString* の値です。
>
>プログラマーアプリケーションが *device_info*&#x200B;を指定した場合、AccessEnabler iOS/tvOS SDK で計算されたすべてのクライアント情報キーが上書きされます。 したがって、キーの値をできるだけ多く計算して渡すことが非常に重要です。 実装について詳しくは、 [概要](#pass-client-info-overview) テーブルと [iOS/tvOS クックブック](#ios-tvos).

#### Android/FireOS SDK {#and-fire-os-sdk}

The `AccessEnabler` Android/FireOS SDK は、デフォルトでクライアント情報の JSON オブジェクトをビルドします。このオブジェクトは、上書きされない限りAdobe Pass Authentication Services に渡されます。

The `AccessEnabler` Android/FireOS SDK は、 **全体を上書きする** クライアント情報 JSON オブジェクトを [setOptions](/help/authentication/android-sdk-api-reference.md#setOptions)&#39;s/[setOptions](/help/authentication/amazon-fireos-native-client-api-reference.md#fire_setOption)&#39;s `device_info` パラメーター。

>[!NOTE]
>
>The `device_info` パラメータ値は必ず指定してください **Base64 エンコード済み** 文字列値。

>[!IMPORTANT]
>
>プログラマーアプリケーションが `device_info`を指定した場合、 `AccessEnabler` Android/FireOS SDK は上書きされます。 したがって、キーの値をできるだけ多く計算して渡すことが非常に重要です。 実装について詳しくは、 [概要](#pass-client-info-overview) テーブルと [Android](#android) および [FireOS](#fire-tv) クックブック。

## クックブック {#cookbooks}

この節では、異なるデバイスタイプの場合に、クライアント情報 JSON オブジェクトを構築するためのクックブックを示します。

>[!IMPORTANT]
>
>次のマークが付いたキー  **!** は必ず送信されます。

### Android {#android}

デバイス情報は、次のように構築できます。

|   | キー | ソース | 値（例） |
|---|---------------|-----------------------------|---------------|
| ! | モデル | Build.MODEL | GT-I9505 |
|   | ベンダー | Build.BRAND | samsung |
|   | 製造元 | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | ハードコード | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

接続情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

アプリケーション情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報を同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード済み**. また、Adobe Pass Authentication REST API の場合、値は **URL エンコード済み**.

**サンプルコード**

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
**リソース：**
* 公共教室 [ビルド](https://developer.android.com/reference/android/os/Build.html){target=_blank} （Java 開発者向けドキュメント）を参照してください。

### FireTV {#fire-tv}

デバイス情報は、次のように構築できます。

|   | キー | ソース | 値（例： ） |
|---|---------------|-----------------------------|--------------|
| ! | モデル | Build.MODEL | AFTM |
|   | ベンダー | Build.BRAND | Amazon |
|   | 製造元 | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | もんとや |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | ハードコード | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

接続情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

アプリケーション情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報を同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード済み**. また、Adobe Pass Authentication REST API の場合、値は **URL エンコード済み**.

>[!NOTE]
>
**リソース：**
* 公共教室 [ビルド](https://developer.android.com/reference/android/os/Build.html){target=_blank} （Android 開発者向けドキュメント）を参照してください。
* [FireTV デバイスの識別](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

デバイス情報は、次のように構築できます。

|   | キー | ソース | 値（例） |
|---|---------------|------------------------|--------------|
| ! | モデル | uname.machine | iPhone |
|   | ベンダー | ハードコード | Apple |
|   | 製造元 | ハードコード | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Reachability currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


アプリケーション情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報を同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトを Base64 エンコードする必要があります。 また、Adobe Pass Authentication REST API の場合は、値を URL エンコードする必要があります。

**サンプルコード**

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
**リソース：**
* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
* [到達性について](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

デバイス情報は、次のように構築できます。

| キー | ソース | 値（例） |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | モデル | ハードコード | &quot;Roku&quot; |
|     | ベンダー | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | 製造元 | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | ハードコード | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

接続情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | ハードコード | 接続が接続されている場合は true |

アプリケーション情報は、次の方法で構築できます。

|   | キー | ソース | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報を同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード済み**. また、Adobe Pass Authentication REST API の場合は、値を URL エンコードする必要があります。

>[!NOTE]
>
詳しくは、 [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

デバイス情報は、次のように構築できます。

|   | キー | ソース | 値（例） |
|---|---|---|---|
| ! | モデル | EasClientDeviceInformation.SystemProductName |                 |
|   | ベンダー | ハードコード | Microsoft |
|   | 製造元 | ハードコード | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

接続情報は、次の方法で構築できます。

|   | キー | ソース | 例 |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;なし&quot;、&quot;Wpa&quot;など |

アプリケーション情報は、次の方法で構築できます。

| キー | ソース | 値（例） |
|---|---|---|
| applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報を同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード済み**. また、Adobe Pass Authentication REST API の場合、値は **URL エンコード済み**.

**リソース**

* [EasClientDeviceInformation クラス](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation クラス](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
