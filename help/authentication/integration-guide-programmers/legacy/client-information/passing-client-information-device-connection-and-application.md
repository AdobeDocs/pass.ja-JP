---
title: クライアント情報（デバイス、接続、アプリケーション）の受け渡し
description: クライアント情報（デバイス、接続、アプリケーション）の受け渡し
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1725'
ht-degree: 2%
---
# （レガシー）クライアント情報（デバイス、接続、アプリケーション）の引き継ぎ {#pass-client-info}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 範囲 {#pass-client-info-scope}

このドキュメントでは、プログラマーアプリケーションからAdobe Pass Authentication REST APIまたはSDKにクライアント情報（デバイス、接続、アプリケーション）を渡すための詳細とクックブックを集計します。

顧客情報を提供する利点は次のとおりです。

* 一部のデバイスタイプおよびHBAをサポートできるMVPDの場合に、ホームベース認証（HBA）を適切に有効にする機能。
* 一部のデバイスタイプの場合にTTLを適切に適用する機能（例えば、TV接続デバイスの認証セッション用に長いTTLを設定する）。
* エンタイトルメントサービスモニタリング（ESM）を使用して、デバイスタイプをまたいで分割されたレポートでビジネス指標を適切に集計する機能。
* 様々なビジネスルールを適切に適用する機能（例： 劣化）が発生する可能性があります。

## 概要 {#pass-client-info-overview}

クライアント情報は次の要素で構成されます。

* ユーザーがプログラマーコンテンツを使用しようとしているデバイスのハードウェア属性とソフトウェア属性に関する&#x200B;**デバイス**&#x200B;情報。
* ユーザーがAdobe Pass Authentication サービスやProgrammer サービス（サーバー間の実装など）に接続しているデバイスの接続属性に関する&#x200B;**Connection**&#x200B;情報。
* ユーザーがプログラマーのコンテンツを利用しようとしている場所から登録済みアプリケーションに関する&#x200B;**アプリケーション**&#x200B;情報。

クライアント情報は、次の表に示すキーで構築されたJSON オブジェクトです。

>[!NOTE]
>
>次の&#x200B;**キー**&#x200B;は、クライアント情報JSON オブジェクトで送信される&#x200B;**必須**&#x200B;です：**モデル**、**osName**。
>
>次のキーには&#x200B;**制限**&#x200B;値があります：`primaryHardwareType`、`osName`、`osFamily`、`browserName`、`browserVendor`、`connectionSecure`。

|   | キー | 制限付き | 説明 | 使用可能な値 |
|---|---|---|---|---|
|            | primaryHardwareType | #はい | デバイスの主なハードウェアタイプ。 | #値は制限されています：Camera DataCollectionTerminal Desktop EmbeddedNetworkModule eReader GamesConsole GeolocationTracker Glasses MediaPlayer MobilePhone PaymentTerminal PluginModem SetTopBox TV Tablet WirelessHotspot Wristwatch Unknown |
| #mandatory | モデル | いいえ | デバイスのモデル名。 | iPhone、SM-G930V、AppleTVなど |
|            | バージョン | いいえ | デバイスのバージョン。 | e.g. 2.0.1など |
|            | メーカー | いいえ | デバイスの製造会社/組織。 | 例：Samsung、LG、ZTE、Huawei、Motorola、Appleなど |
|            | ベンダー | いいえ | デバイスの販売会社/組織。 | 例：Apple、Samsung、LG、Googleなど |
| #mandatory | osName | #はい | デバイスのオペレーティングシステム（OS）名。 | #値は制限されています：Android Chrome OS Linux Mac OS X OpenBSD Roku OS Windows iOS tvOS webOS |
|            | osFamily | はい | デバイスのOS グループ名。 | #値は制限されています：Android BSD Linux PlayStation OS Roku OS Symbian Tizen Windows iOS macOS tvOS webOS |
|            | osVendor | いいえ | デバイスのオペレーティングシステム（OS）サプライヤー。 | GoogleLGMicrosoftMozilla任天堂ノキア六三星ソニータイゼンプロジェクト |
|            | osVersion | いいえ | デバイスのオペレーティングシステム（OS）バージョン。 | 例：10.2、9.0.1など |
|            | browserName | #はい | ブラウザーの名前。 | #値は制限されています：Android Browser Chrome Edge Firefox Internet Explorer Opera Safari SeaMonkey Symbian Browser |
|            | browserVendor | #はい | ブラウザーのビルド会社/組織。 | #値は制限されています：GoogleMicrosoftモトローラ MozillaのNetscape ニンテンドーノキア Samsung ソニーEricsson |
|            | browserVersion | いいえ | デバイスのブラウザーバージョン。 | e.g. 60.0.3112 |
|            | userAgent | いいえ | デバイスのユーザーエージェント。 | e.g. Mozilla/5.0 （Macintosh; Intel Mac OS X 10_12_3） AppleWebKit/602.4.8 （KHTML, like Gecko） Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | いいえ | デバイスの物理スクリーン幅。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | いいえ | デバイスの物理的な画面の高さ。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | いいえ | デバイスの物理的な画面ピクセル密度。 | e.g. 294 |
|            | diagonalScreenSize | いいえ | デバイスの物理的な画面の対角寸法（インチ）。 | e.g. 5.5, 10.1 |
|            | connectionIp | いいえ | HTTP リクエストの送信に使用されるデバイスのIP。 | e.g. 8.8.4.4 |
|            | connectionPort | いいえ | HTTP リクエストの送信に使用されるデバイスのポート。 | e.g. 53124 |
|            | connectionType | いいえ | ネットワーク接続タイプ。 | 例：WiFi、LAN、3G、4G、5G |
|            | connectionSecure | #はい | ネットワーク接続のセキュリティ状態。 | #値は制限されています：true - セキュアネットワークの場合はfalse - パブリックホットスポットの場合 |
|            | applicationId | いいえ | アプリケーションの一意のID。 | e.g. CNN |

## API参照 {#api-ref}

この節では、Adobe Pass Authentication REST APIまたはSDKを使用する場合のクライアント情報の処理を担当するAPIについて説明します。

### REST API {#rest-api}

Adobe Pass Authentication servicesでは、次の方法でクライアント情報を受信できます。

* **ヘッダーとして：&quot;X-Device-Info&quot;**
* **クエリパラメーターとして：&quot;device_info&quot;**
* **post パラメーターとして：&quot;device_info&quot;**

>[!IMPORTANT]
>
>3つのシナリオすべてで、ヘッダーまたはパラメーターのペイロードは&#x200B;**Base64 エンコードおよびURL エンコード済み**&#x200B;である必要があります。

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDKは、デフォルトでクライアント情報JSON オブジェクトを作成します。このオブジェクトは、上書きされない限り、Adobe Pass Authentication サービスに渡されます。

AccessEnabler JavaScript SDKでは、[setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))の&#x200B;*applicationId* オプション パラメーターを使用して、クライアント情報JSON オブジェクトの「applicationId」キーのみを&#x200B;**上書きすることがサポートされています**。

>[!CAUTION]
>
>`applicationId` パラメーター値は、プレーンテキスト文字列値である必要があります。
>プログラマーアプリケーションがapplicationIdを渡すことにした場合、残りのクライアント情報キーはAccessEnabler JavaScript SDKによって計算されます。

#### iOS/tvOS SDK {#ios-tvos-sdk}

AccessEnabler iOS/tvOS SDKは、デフォルトでクライアント情報JSON オブジェクトを作成します。このオブジェクトは、上書きされない限り、Adobe Pass Authentication サービスに渡されます。

AccessEnabler iOS/tvOS SDKでは、[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions)のdevice_info パラメーターを使用して&#x200B;**クライアント情報JSON オブジェクト全体を**&#x200B;上書きすることがサポートされています。

>[!CAUTION]
>
>*device_info* パラメーター値は、**Base64 エンコードされた** *NSString*&#x200B;値である必要があります。
>
>プログラマーアプリケーションが&#x200B;*device_info*&#x200B;を渡すと判断した場合、AccessEnabler iOS/tvOS SDKで計算されたすべてのクライアント情報キーが上書きされます。 したがって、できるだけ多くのキーの値を計算して渡すことが非常に重要です。 実装の詳細については、[概要](#pass-client-info-overview)の表と[iOS/tvOS cookbook](#ios-tvos)を参照してください。

#### Android/FireOS SDK {#and-fire-os-sdk}

`AccessEnabler` Android/FireOS SDKは、デフォルトでクライアント情報JSON オブジェクトをビルドします。このオブジェクトは、上書きされない限り、Adobe Pass Authentication サービスに渡されます。

`AccessEnabler` Android/FireOS SDKでは、[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)の/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption)の`device_info` パラメーターを使用して、**クライアント情報JSON オブジェクト全体を**&#x200B;上書きすることがサポートされています。

>[!NOTE]
>
>`device_info` パラメーター値は、**Base64 エンコードされた**&#x200B;文字列値である必要があります。

>[!IMPORTANT]
>
>プログラマーアプリケーションが`device_info`を渡すと決定した場合、`AccessEnabler` Android/FireOS SDKで計算されたすべてのクライアント情報キーが上書きされます。 したがって、できるだけ多くのキーの値を計算して渡すことが非常に重要です。 実装の詳細については、[概要](#pass-client-info-overview) テーブルと[Android](#android)および[FireOS](#fire-tv) クックブックを参照してください。

## Cookbooks {#cookbooks}

この節では、異なるデバイスタイプの場合にクライアント情報JSON オブジェクトを構築するためのクックブックを紹介します。

>[!IMPORTANT]
>
>**!**&#x200B;でマークされたキー 送信するには必須です。

### Android {#android}

デバイス情報は、次のように構築できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------------------------|---------------|
| ! | モデル | Build.MODEL | GT-I9505 |
|   | ベンダー | Build.BRAND | samsung |
|   | メーカー | Build.MANUFACTURER | samsung |
| ! | バージョン | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | ハードコードされた | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

接続情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコードされた | CNN |

>[!IMPORTANT]
>
>デバイス、接続、およびアプリケーション情報は、同じJSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは&#x200B;**Base64 エンコード済み**&#x200B;である必要があります。 また、Adobe Pass Authentication REST APIの場合、値は&#x200B;**URL エンコード**&#x200B;である必要があります。

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
>**リソース：**
>* パブリッククラス [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} （Java開発者ドキュメント）を参照してください。

### FireTV {#fire-tv}

デバイス情報は、次のように構築できます。

|   | キー | Source | 値（例：） |
|---|---------------|-----------------------------|--------------|
| ! | モデル | Build.MODEL | AFTM |
|   | ベンダー | Build.BRAND | Amazon |
|   | メーカー | Build.MANUFACTURER | Amazon |
| ! | バージョン | Build.DEVICE | モントーヤ |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | ハードコードされた | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

接続情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコードされた | CNN |

>[!IMPORTANT]
>
>デバイス、接続、およびアプリケーション情報は、同じJSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは&#x200B;**Base64 エンコード済み**&#x200B;である必要があります。 また、Adobe Pass Authentication REST APIの場合、値は&#x200B;**URL エンコード**&#x200B;である必要があります。

>[!NOTE]
>
>**リソース：**
>* Android開発者ドキュメントのパブリッククラス [&#x200B; ビルド &#x200B;](https://developer.android.com/reference/android/os/Build.html){target=_blank}。
>* [FireTV デバイスの特定](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

デバイス情報は、次のように構築できます。

|   | キー | Source | 値（例） |
|---|---------------|------------------------|--------------|
| ! | モデル | uname.machine | iPhone |
|   | ベンダー | ハードコードされた | Apple |
|   | メーカー | ハードコードされた | Apple |
| ! | バージョン | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Reachability currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコードされた | CNN |

>[!IMPORTANT]
>
>デバイス、接続、およびアプリケーション情報は、同じJSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトはBase64でエンコードする必要があります。 また、Adobe Pass Authentication REST APIの場合、値はURL エンコードされている必要があります。

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
>**リソース：**
>* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [到達可能性について](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### 六区 {#roku}

デバイス情報は、次のように構築できます。

| キー | Source | 値（例） |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | モデル | ハードコードされた | 「六」 |
|     | ベンダー | ifDeviceInfo.GetModelDetails （）.VendorName | 「シャープ」「ロク」 |
|     | メーカー | ifDeviceInfo.GetModelDetails （）.VendorName | 「シャープ」「ロク」 |
| ! | バージョン | ifDeviceInfo.GetModelDetails （）.ModelNumber | 「5303X」 |
|     | displayWidth | ifDeviceInfo.GetDisplaySize （）.w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize （）.h | 1080 |
| ! | osName | ハードコードされた | 「六」 |
| ! | osVersion | ifDeviceInfo.getVersion （） |                 |

接続情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType （） | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | ハードコードされた | 接続が有線の場合はtrue |

アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコードされた | CNN |

>[!IMPORTANT]
>
>デバイス、接続、およびアプリケーション情報は、同じJSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは&#x200B;**Base64 エンコード済み**&#x200B;である必要があります。 また、Adobe Pass Authentication REST APIの場合、値はURL エンコードされている必要があります。

>[!NOTE]
>
>詳細については、[ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)を参照してください

### XBOX 1/360 {#xbox}

デバイス情報は、次のように構築できます。

|   | キー | Source | 値（例） |
|---|---|---|---|
| ! | モデル | EasClientDeviceInformation.SystemProductName |                 |
|   | ベンダー | ハードコードされた | Microsoft |
|   | メーカー | ハードコードされた | Microsoft |
| ! | バージョン | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

接続情報は、次の方法で作成できます。

|   | キー | Source | 例 |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;None&quot;, &quot;Wpa&quot;など |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---|---|---|
| applicationId | ハードコードされた | CNN |

>[!IMPORTANT]
>
>デバイス、接続、およびアプリケーション情報は、同じJSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは&#x200B;**Base64 エンコード済み**&#x200B;である必要があります。 また、Adobe Pass Authentication REST APIの場合、値は&#x200B;**URL エンコード**&#x200B;である必要があります。

**リソース**

* [EasClientDeviceInformation クラス](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation クラス](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
