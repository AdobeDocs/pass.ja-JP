---
title: Header - X-Device-Info
description: REST API V2 - Header - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1234'
ht-degree: 3%
---
# Header - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## 概要 {#overview}

<b>X-Device-Info</b> リクエストヘッダーには、実際のストリーミングデバイスに関連するクライアント情報（デバイス、接続、アプリケーション）が含まれており、MVPDが適用する可能性のあるプラットフォーム固有のルールを決定するために使用されます。

## 構文 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>ヘッダータイプ</td>
      <td>リクエストヘッダー</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>いいえ</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;device_information></b>

次の表で必須とマークされた属性を少なくとも含むJSON要素の`Base64-encoded`値。

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">プレゼンス</th>
        <th style="background-color: #EFF2F7; width: 15%;">キー</th>
        <th style="background-color: #EFF2F7;">説明</th>    
        <th style="background-color: #EFF2F7; width: 15%;">制限付き</th>
        <th style="background-color: #EFF2F7;">使用可能な値</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>デバイスの主なハードウェアタイプ。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>カメラ</li>
                <li>DataCollectionTerminal</li>
                <li>デスクトップ</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>メガネ</li>
                <li>MediaPlayer</li>
                <li>MobilePhone</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>タブレット</li>
                <li>WirelessHotspot</li>
                <li>腕時計</li>
                <li>不明</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>モデル</td>
        <td>デバイスのモデル名。</td>
        <td></td>
        <td>iPhone、SM-G930V、AppleTVなど</td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>バージョン</td>
        <td>デバイスのバージョン。</td>
        <td></td>
        <td>e.g. 2.0.1など</td>
    </tr>
    <tr>
        <td></td>
        <td>メーカー</td>
        <td>デバイスの製造会社/組織。</td>
        <td></td>
        <td>例：Samsung、LG、ZTE、Huawei、Motorola、Appleなど</td>
    </tr>
    <tr>
        <td></td>
        <td>ベンダー</td>
        <td>デバイスの販売会社/組織。</td>
        <td></td>
        <td>例：Apple、Samsung、LG、Googleなど</td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>osName</td>
        <td>デバイスのオペレーティングシステム（OS）名。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>Android</li>
                <li>CHROME OS</li>
                <li>Linux</li>
                <li>MAC OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>デバイスのOS グループ名。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>シンビアン</li>
                <li>タイゼン</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>デバイスのオペレーティングシステム（OS）サプライヤー。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>任天堂</li>
                <li>ノキア</li>
                <li>六区</li>
                <li>Samsung</li>
                <li>ソニー</li>
                <li>Tizen プロジェクト</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>osVersion</td>
        <td>デバイスのオペレーティングシステム（OS）バージョン。</td>
        <td></td>
        <td>例：10.2、9.0.1など</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>ブラウザーの名前。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>Android Browser</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>オペラ</li>
                <li>Safari</li>
                <li>シーモンキー</li>
                <li>Symbian ブラウザー</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>ブラウザーのビルド会社/組織。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>モトローラ</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>任天堂</li>
                <li>ノキア</li>
                <li>Samsung</li>
                <li>ソニー・エリクソン</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>デバイスのブラウザーバージョン。</td>
        <td></td>
        <td>e.g. 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>デバイスのユーザーエージェント。</td>
        <td></td>
        <td>e.g. Mozilla/5.0 （Macintosh; Intel Mac OS X 10_12_3） AppleWebKit/602.4.8 （KHTML, like Gecko） Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>デバイスの物理スクリーン幅。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>デバイスの物理的な画面の高さ。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>デバイスの物理的な画面ピクセル密度。</td>
        <td></td>
        <td>e.g. 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>デバイスの物理的な画面の対角寸法（インチ）。</td>
        <td></td>
        <td>e.g. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>HTTP リクエストの送信に使用されるデバイスのIP。</td>
        <td></td>
        <td>e.g. 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>HTTP リクエストの送信に使用されるデバイスのポート。</td>
        <td></td>
        <td>e.g. 53124</td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>connectionType</td>
        <td>ネットワーク接続タイプ。</td>
        <td></td>
        <td>例：WiFi、LAN、3G、4G、5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>ネットワーク接続のセキュリティ状態。</td>
        <td>&amp;check;</td>
        <td>
            値は次のように制限されています。
            <ul>
                <li>true – 安全なネットワークの場合</li>
                <li>false – 公共のホットスポットの場合</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>アプリケーションの一意のID。</td>
        <td></td>
        <td>e.g. REF30</td>
    </tr>
</table>


## 例 {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
> 
> コードスニペットとドキュメントリソースは、参照目的で提供されます。
> 
> コードスニペットは完全なものではなく、プロジェクトで作業するには、追加の変更が必要になる場合があります。
>
> 実際の実装に関係なく、`X-Device-Info` ヘッダーには、[ ディレクティブ ](#directives) セクションで説明されているようにフォーマットされた値を含める必要があります。

### ブラウザー {#browsers}

ブラウザーで実行されているクライアントアプリケーションの場合、`X-Device-Info` ヘッダーは省略できます。ブラウザーは、`User-Agent` ヘッダーに必要な最小限の情報セットを自動的に送信します。

クライアントアプリケーションがデバイス識別メカニズムを提供するライブラリまたはサービスを統合する場合は、`X-Device-Info` ヘッダーを使用して、デバイス、接続、およびアプリケーションに関する追加情報を提供できます。

### モバイルデバイス {#mobile-devices}

#### iOSとiPadOS {#ios-ipados}

[iOSまたはiPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)を実行しているデバイスの`X-Device-Info` ヘッダーを作成するには、次のドキュメントとコードスニペットの下を参照してください。

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)のApple開発者向けドキュメント。
* [Reachability](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)のApple開発者向けドキュメント。
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html)のLinux マニュアル ドキュメント。

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

デバイス情報は、次のように構築できます。

| キー | Source | 値（例） |
|---------------|------------------------|-----------------|
| モデル | uname.machine | iPhone |
| ベンダー | ハードコードされた | Apple |
| メーカー | ハードコードされた | Apple |
| バージョン | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコードされた | REF30 |

#### Android {#android}

[Android](https://developer.android.com/about/versions)を実行しているデバイスの`X-Device-Info` ヘッダーを作成するには、次のドキュメントとコードスニペットの下を参照してください。

* [ ビルド ](https://developer.android.com/reference/android/os/Build.html) クラスのAndroid開発者向けドキュメント。

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
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

デバイス情報は、次のように構築できます。

| キー | Source | 値（例） |
|---------------|-----------------------------|-----------------|
| モデル | Build.MODEL | GT-I9505 |
| ベンダー | Build.BRAND | samsung |
| メーカー | Build.MANUFACTURER | samsung |
| バージョン | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | ハードコードされた | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

接続情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコードされた | REF30 |

### TV接続デバイス {#tv-connected-devices}

#### tvOS {#tvos}

[tvOS](https://developer.apple.com/documentation/tvos-release-notes)を実行しているデバイスの`X-Device-Info` ヘッダーを作成するには、次のドキュメントとコードスニペットの下を参照してください。

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)のApple開発者向けドキュメント。
* [Reachability](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)のApple開発者向けドキュメント。
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html)のLinux マニュアル ドキュメント。

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

デバイス情報は、次のように構築できます。

| キー | Source | 値（例） |
|---------------|------------------------|-----------------|
| モデル | uname.machine | AppleTV |
| ベンダー | ハードコードされた | Apple |
| メーカー | ハードコードされた | Apple |
| バージョン | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコードされた | REF30 |

#### Fire OS {#fireos}

[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)を実行しているデバイスの`X-Device-Info` ヘッダーを作成するには、次のドキュメントを参照してください。

* [ ビルド ](https://developer.android.com/reference/android/os/Build.html) クラスのAndroid開発者向けドキュメント。
* Fire TV デバイスの特定[に関するAmazon開発者用ドキュメント ](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html)。

デバイス情報は、次のように構築できます。

| キー | Source | 値（例） |
|---------------|-----------------------------|-----------------|
| モデル | Build.MODEL | AFTM |
| ベンダー | Build.BRAND | Amazon |
| メーカー | Build.MANUFACTURER | Amazon |
| バージョン | Build.DEVICE | モントーヤ |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | ハードコードされた | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

接続情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコードされた | REF30 |

#### Roku OS {#rokuos}

[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)を実行しているデバイスの`X-Device-Info` ヘッダーを作成するには、次のドキュメントを参照してください。

* [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)のRoku開発者ドキュメント。

デバイス情報は、次のように構築できます。

| キー | Source | 値（例） |
|---------------|--------------------------------------------|-----------------|
| モデル | ハードコードされた | 「六」 |
| ベンダー | ifDeviceInfo.GetModelDetails （）.VendorName | 「シャープ」「ロク」 |
| メーカー | ifDeviceInfo.GetModelDetails （）.VendorName | 「シャープ」「ロク」 |
| バージョン | ifDeviceInfo.GetModelDetails （）.ModelNumber | 「5303X」 |
| displayWidth | ifDeviceInfo.GetDisplaySize （）.w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize （）.h | 1080 |
| osName | ハードコードされた | 「六」 |
| osVersion | ifDeviceInfo.getVersion （） |                 |

接続情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType （） | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | ハードコードされた | 接続が有線の場合はtrue |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコードされた | REF30 |

### その他 {#others}

ドキュメントに記載されていないデバイスプラットフォームの場合、クライアント情報（デバイス、接続、アプリケーション）は、使用可能なハードウェアおよびオペレーティングシステム（OS）属性（通常はデバイスのハードウェアおよびOS マニュアルで指定）にリンクする必要があります。
