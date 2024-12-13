---
title: クライアント情報（デバイス、接続、アプリケーション）を渡す
description: クライアント情報（デバイス、接続、アプリケーション）を渡す
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1644'
ht-degree: 1%

---

# （レガシー）クライアント情報（デバイス、接続、アプリケーション）の受け渡し {#pass-client-info}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。


## 対象範囲 {#pass-client-info-scope}

このドキュメントでは、クライアント情報（デバイス、接続、アプリケーション）をプログラマーアプリケーションからAdobe Pass認証の REST API または SDK に渡すための詳細とクックブックを集約します。

クライアント情報を提供する利点は次のとおりです。

* 一部のデバイス・タイプや HBA をサポートする MVPD の場合に、HBA （Home Base Authentication）を適切に有効化する機能。
* 一部のデバイスタイプの場合に TTL を適切に適用する機能（例えば、テレビに接続されたデバイスの認証セッション用に長い TTL を設定するなど）。
* ESM （Entitlement Service Monitoring）を使用して、複数のデバイスタイプにわたる分類レポートでビジネス指標を適切に集計する機能。
* 様々なビジネスルールを適切に適用する機能のブロックを解除します（例： （特定のデバイスタイプで低下）。

## 概要 {#pass-client-info-overview}

クライアント情報は、以下で構成されます。

* **デバイス** ユーザーがプログラマーのコンテンツを使用しようとしているデバイスのハードウェア属性とソフトウェア属性に関する情報。
* **接続** ユーザーがAdobe Pass Authentication Services やプログラマーサービスに接続する元となるデバイスの接続属性に関する情報（サーバー間実装など）。
* **アプリケーション** ユーザーがプログラマーコンテンツを使用しようとする登録済みアプリケーションに関する情報。

クライアント情報は、次の表に示すキーで作成された JSON オブジェクトです。

>[!NOTE]
>
>次の **キー** は、クライアント情報 JSON オブジェクトで送信する **必須** です：**model**、**osName**。
>
>次のキーには **制限付き** の値があります：`primaryHardwareType`、`osName`、`osFamily`、`browserName`、`browserVendor`、`connectionSecure`。

|   | キー | 制限付き | 説明 | 可能な値 |
|---|---|---|---|---|
|            | primary ハードウェアタイプ | #はい | デバイスの主要なハードウェアの種類。 | #値は制限されています。                                                                     カメラ                                                      DataCollectionTerminal                                                      デスクトップ                                                      EmbeddedNetworkModule                                                      eReader                                                      ゲーム コンソール                                                      GeolocationTracker                                                      眼鏡                                                      MediaPlayer                                                      MobilePhone                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      タブレット                                                      WirelessHotspot                                                      腕時計                                                      不明 |
| #mandatory | モデル | 不可 | デバイスのモデル名。 | 例：iPhone、SM-G930V、AppleTV など |
|            | version | 不可 | デバイスのバージョン。 | 例：2.0.1 など |
|            | 製造元 | 不可 | デバイスの製造会社/組織。 | 例：Samsung、LG、ZTE、Huawei、Motorola、Appleなど |
|            | ベンダー | 不可 | デバイスの販売会社/組織。 | 例：Apple、Samsung、LG、Googleなど |
| #mandatory | osName | #はい | デバイスのオペレーティングシステム（OS）名。 | #値は制限されています。                                                   Android                   CHROME OS                   Linux                   MAC OS                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | はい | デバイスのオペレーティングシステム（OS）グループ名。 | #値は制限されています。                                                   Android                   BSD                   Linux                   PlayStation OS                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | 不可 | デバイスのオペレーティングシステム（OS）のサプライヤ。 | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   任天堂                   Nokia                   Roku                   Samsung                   ソニー                   Tizen プロジェクト |
|            | osVersion | 不可 | デバイスのオペレーティングシステム（OS）のバージョン。 | 例：10.2、9.0.1 など |
|            | browserName | #はい | ブラウザーの名前。 | #値は制限されています。                                                   Androidブラウザー                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   海猿                   Symbian ブラウザ |
|            | browserVendor | #はい | ブラウザーの建物会社/組織。 | #値は制限されています。                                                   Amazon                   Apple                   Google                   Microsoft                   モトローラ                   Mozilla                   Netscape                   任天堂                   Nokia                   Samsung                   ソニーエリクソン |
|            | browserVersion | 不可 | デバイスのブラウザーのバージョン。 | 例：60.0.3112 |
|            | userAgent | 不可 | デバイスのユーザーエージェント。 | 例：Mozilla/5.0 （Macintosh、Intel Mac OS X 10_12_3） AppleWebKit/602.4.8 （KHTML、Gecko など） Version/10.0.3 Safari/602.4.8 |
|            | displayWidth | 不可 | デバイスの物理的な画面の幅。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | 不可 | デバイスの物理的な画面の高さ。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | 不可 | デバイスの物理的な画面のピクセル密度。 | 例：294 |
|            | diagonalScreenSize | 不可 | デバイスの物理的な画面の斜めの寸法（インチ）。 | 例：5.5、10.1 |
|            | connectionIp | 不可 | HTTP リクエストの送信に使用するデバイスの IP。 | 例 8.8.4.4 |
|            | connectionPort | 不可 | HTTP リクエストの送信に使用するデバイスのポート。 | 例：53124 |
|            | connectionType | 不可 | ネットワーク接続タイプ。 | 例：WiFi、LAN、3G、4G、5G |
|            | connectionSecure | #はい | ネットワーク接続のセキュリティの状態。 | #値は制限されています。                                                   true – 安全なネットワークの場合                   false – 公共のホットスポットの場合 |
|            | applicationId | 不可 | アプリケーションの一意の ID。 | 例：CNN |

## API リファレンス {#api-ref}

この節では、Adobe Pass認証 REST API または SDK を使用する際にクライアント情報を処理する API について説明します。

### REST API {#rest-api}

Adobe Pass認証サービスは、次の方法でクライアント情報の受け取りをサポートしています。

* **ヘッダー：「X-Device-Info」** として
* **クエリパラメーター：&quot;device_info&quot;として**
* **post パラメーターとして：&quot;device_info&quot;**

>[!IMPORTANT]
>
>3 つのシナリオすべてで、ヘッダーまたはパラメーターのペイロードを **Base64 でエンコードし、URL でエンコード** する必要があります。

**SDK**

#### JavaScriptSDK {#js-sdk}

AccessEnabler JavaScript SDKは、デフォルトでクライアント情報 JSON オブジェクトを構築します。このオブジェクトは、上書きされない限り、Adobe Pass Authentication サービスに渡されます。

AccessEnabler JavaScript SDKでは **setRequestor** の *applicationId* オプション パラメータを使用して、クライアント情報 JSON オブジェクトの [applicationId](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)) キーをサポートします上書きのみ）。

>[!CAUTION]
>
>`applicationId` パラメーター値は、プレーンテキストの文字列値である必要があります。
>プログラマーアプリケーションが applicationId を渡すことにした場合、残りのクライアント情報キーは引き続き AccessEnabler JavaScript SDKによって計算されます。

#### iOS/tvOS SDK {#ios-tvos-sdk}

AccessEnabler iOS/tvOS SDKは、デフォルトでクライアント情報の JSON オブジェクトを作成します。このオブジェクトは、上書きされない限り、Adobe Pass Authentication サービスに渡されます。

AccessEnabler iOS/tvOS SDKでは、[setOptions **の device_info パラメータを使用して** クライアント情報の JSON オブジェクトをオーバーライド ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions) できます。

>[!CAUTION]
>
>*device_info* パラメーター値は **Base64 エンコード** *NSString* 値にする必要があります。
>
>プログラマーアプリケーションが *device_info* を渡すことにした場合、AccessEnabler iOS/tvOS SDKによって計算されたすべてのクライアント情報キーが上書きされます。 したがって、できるだけ多くのキーの値を計算して渡すことが非常に重要です。 実装について詳しくは、[ 概要 ](#pass-client-info-overview) 表および [iOS/tvOS クックブック ](#ios-tvos) を参照してください。

#### Android/FireOS SDK {#and-fire-os-sdk}

`AccessEnabler` Android/FireOS SDKは、デフォルトでクライアント情報 JSON オブジェクトを作成し、上書きされない限り、これをAdobe Pass認証サービスに渡します。

`AccessEnabler` Android/FireOS SDKは、**setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions) の/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption) の `device_info` パラメーターを使用して** クライアント情報の JSON オブジェクトを上書き [ サポートしています。

>[!NOTE]
>
>`device_info` パラメーター値は、**Base64 エンコード** 文字列値である必要があります。

>[!IMPORTANT]
>
>プログラマーアプリケーションが `device_info` を渡すことにした場合、`AccessEnabler` Android/FireOS SDKで計算されたすべてのクライアント情報キーが上書きされます。 したがって、できるだけ多くのキーの値を計算して渡すことが非常に重要です。 実装について詳しくは、[ 概要 ](#pass-client-info-overview) テーブルおよび [Android](#android) と [FireOS](#fire-tv) のクックブックを参照してください。

## クックブック {#cookbooks}

この節では、デバイスタイプが異なる場合にクライアント情報 JSON オブジェクトを作成するためのクックブックを示します。

>[!IMPORTANT]
>
>**でマークされたキー！** を送信する必要があります。

### Android {#android}

デバイス情報は、次のように構成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------------------------|---------------|
| ! | モデル | Build.MODEL | GT-I9505 |
|   | ベンダー | Build.BRAND | samsung |
|   | 製造元 | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | ハードコード | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

接続情報は、次のように作成できます。

|   | キー | Source | 値（例） |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報は、同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード** する必要があります。 また、Adobe Pass認証 REST API の場合は、値を **URL エンコード** する必要があります。

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
* java 開発者向けドキュメントのパブリッククラス [build](https://developer.android.com/reference/android/os/Build.html){target=_blank}。

### FireTV {#fire-tv}

デバイス情報は、次のように構成できます。

|   | キー | Source | 値（例：） |
|---|---------------|-----------------------------|--------------|
| ! | モデル | Build.MODEL | AFTM |
|   | ベンダー | Build.BRAND | Amazon |
|   | 製造元 | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | ハードコード | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

接続情報は、次のように作成できます。

|   | キー | Source | 値（例） |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報は、同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード** する必要があります。 また、Adobe Pass認証 REST API の場合は、値を **URL エンコード** する必要があります。

>[!NOTE]
>
**リソース：**
* Android開発者向けドキュメントのパブリッククラス [ ビルド ](https://developer.android.com/reference/android/os/Build.html){target=_blank}。
* [FireTV デバイスの識別 ](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

デバイス情報は、次のように構成できます。

|   | キー | Source | 値（例） |
|---|---------------|------------------------|--------------|
| ! | モデル | uname.machine | iPhone |
|   | ベンダー | ハードコード | Apple |
|   | 製造元 | ハードコード | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次のように作成できます。

|   | キー | Source | 値（例） |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Reachability currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報は、同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは Base64 でエンコードする必要があります。 また、Adobe Pass認証 REST API の場合、値を URL エンコードする必要があります。

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
* [ 到達可能性について ](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

デバイス情報は、次のように構成できます。

| キー | Source | 値（例） |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | モデル | ハードコード | 「Roku」 |
|     | ベンダー | ifDeviceInfo.GetModelDetails().VendorName | 「シャープ」「Roku」 |
|     | 製造元 | ifDeviceInfo.GetModelDetails().VendorName | 「シャープ」「Roku」 |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | ハードコード | 「Roku」 |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

接続情報は、次のように作成できます。

|   | キー | Source | 値（例） |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | ハードコード | 接続がワイヤリングされている場合は true |

アプリケーション情報は、次の方法で作成できます。

|   | キー | Source | 値（例） |
|---|---------------|-----------|--------------|
|   | applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報は、同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード** する必要があります。 また、Adobe Pass認証 REST API の場合、値を URL エンコードする必要があります。

>[!NOTE]
>
詳しくは、「[ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)」を参照してください

### XBOX 1/360 {#xbox}

デバイス情報は、次のように構成できます。

|   | キー | Source | 値（例） |
|---|---|---|---|
| ! | モデル | EasClientDeviceInformation.SystemProductName |                 |
|   | ベンダー | ハードコード | Microsoft |
|   | 製造元 | ハードコード | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

接続情報は、次のように作成できます。

|   | キー | Source | 例 |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | 「なし」、「Wpa」など |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---|---|---|
| applicationId | ハードコード | CNN |

>[!IMPORTANT]
>
デバイス、接続、アプリケーションの情報は、同じ JSON オブジェクトに追加する必要があります。 その後、結果のオブジェクトは **Base64 エンコード** する必要があります。 また、Adobe Pass認証 REST API の場合は、値を **URL エンコード** する必要があります。

**リソース**

* [EasClientDeviceInformation クラス ](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation クラス ](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
