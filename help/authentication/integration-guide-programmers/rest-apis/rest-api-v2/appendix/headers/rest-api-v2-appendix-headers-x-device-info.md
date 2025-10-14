---
title: ヘッダー – X-Device-Info
description: REST API V2 - ヘッダー – X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 42df16e34783807e1b5eb1a12ca9db92f4e4c161
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 2%

---

# ヘッダー – X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>X-Device-Info</b> 要求ヘッダーは、実際のストリーミング デバイスに関連するクライアント情報（デバイス、接続、およびアプリケーション）を格納し、MVPD が適用する可能性のあるプラットフォーム固有のルールを決定するために使用されます。

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
      <td>標準</td>
      <td>不可</td>
   </tr>
</table>

## ディレクティブ {#directives}

<b>&lt;device_information></b>

次の表で必要とマークされた属性を少なくとも含む JSON 要素の `Base64-encoded` 値。

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Presence</th>
        <th style="background-color: #EFF2F7; width: 15%;">キー</th>
        <th style="background-color: #EFF2F7;">説明</th>    
        <th style="background-color: #EFF2F7; width: 15%;">制限付き</th>
        <th style="background-color: #EFF2F7;">可能な値</th>
    </tr>
    <tr>
        <td></td>
        <td>primary ハードウェアタイプ</td>
        <td>デバイスの主要なハードウェアの種類。</td>
        <td>&check;</td>
        <td>
            値は制限されています。
            <ul>
                <li>カメラ</li>
                <li>DataCollectionTerminal</li>
                <li>デスクトップ</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>ゲーム コンソール</li>
                <li>GeolocationTracker</li>
                <li>眼鏡</li>
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
        <td>例：iPhone、SM-G930V、AppleTV など</td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>version</td>
        <td>デバイスのバージョン。</td>
        <td></td>
        <td>例：2.0.1 など</td>
    </tr>
    <tr>
        <td></td>
        <td>製造元</td>
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
        <td>&check;</td>
        <td>
            値は制限されています。
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
        <td>デバイスのオペレーティングシステム（OS）グループ名。</td>
        <td>&check;</td>
        <td>
            値は制限されています。
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
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
        <td>デバイスのオペレーティングシステム（OS）のサプライヤ。</td>
        <td>&check;</td>
        <td>
            値は制限されています。
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>任天堂</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>ソニー</li>
                <li>Tizen プロジェクト</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必須</i></td>
        <td>osVersion</td>
        <td>デバイスのオペレーティングシステム（OS）のバージョン。</td>
        <td></td>
        <td>例：10.2、9.0.1 など</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>ブラウザーの名前。</td>
        <td>&check;</td>
        <td>
            値は制限されています。
            <ul>
                <li>Androidブラウザー</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>海猿</li>
                <li>Symbian ブラウザ</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>ブラウザーの建物会社/組織。</td>
        <td>&check;</td>
        <td>
            値は制限されています。
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>モトローラ</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>任天堂</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>ソニーエリクソン</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>デバイスのブラウザーのバージョン。</td>
        <td></td>
        <td>例：60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>デバイスのユーザーエージェント。</td>
        <td></td>
        <td>例：Mozilla/5.0 （Macintosh、Intel Mac OS X 10_12_3） AppleWebKit/602.4.8 （KHTML、Gecko など） Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>デバイスの物理的な画面の幅。</td>
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
        <td>デバイスの物理的な画面のピクセル密度。</td>
        <td></td>
        <td>例：294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>デバイスの物理的な画面の斜めの寸法（インチ）。</td>
        <td></td>
        <td>例：5.5、10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>HTTP リクエストの送信に使用するデバイスの IP。</td>
        <td></td>
        <td>例 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>HTTP リクエストの送信に使用するデバイスのポート。</td>
        <td></td>
        <td>例：53124</td>
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
        <td>ネットワーク接続のセキュリティの状態。</td>
        <td>&check;</td>
        <td>
            値は制限されています。
            <ul>
                <li>true – 安全なネットワークの場合</li>
                <li>false – 公共のホットスポットの場合</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>アプリケーションの一意の ID。</td>
        <td></td>
        <td>例：REF30</td>
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

## クックブック {#cookbooks}

>[!IMPORTANT]
> 
> コードスニペットとドキュメントリソースは、参照用に提供されています。
> 
> コードスニペットは完全なものではないので、プロジェクトで機能させるには追加の変更が必要になる場合があります。
>
> 実際の実装に関係なく、`X-Device-Info` ヘッダーには、「[&#x200B; ディレクティブ &#x200B;](#directives) セクションで説明されている形式の値が含まれている必要があります。

### ブラウザー {#browsers}

ブラウザーで実行されているクライアントアプリケーションの場合、ブラウザーは `X-Device-Info` ヘッダーに必要な最小限の情報のセットを自動的に送信するので、`User-Agent` ヘッダーは省略できます。

引き続き `X-Device-Info` ヘッダーを使用して、デバイス、接続、およびアプリケーションに関する追加情報を提供できます。これは、デバイス識別メカニズムを提供するライブラリまたはサービスをクライアントアプリケーションが統合する場合に可能です。

### モバイルデバイス {#mobile-devices}

#### iOSと iPadOS {#ios-ipados}

[iOSまたは iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes) を実行するデバイスの `X-Device-Info` ヘッダーを作成するには、次のドキュメントと以下のコードスニペットを参照してください。

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice) のApple開発者向けドキュメント。
* Apple開発者向けドキュメント [&#x200B; 到達可能性 &#x200B;](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)。
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html) に関する Linux マニュアルのドキュメント。

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

デバイス情報は、次のように構成できます。

| キー | Source | 値（例） |
|---------------|------------------------|-----------------|
| モデル | uname.machine | iPhone |
| ベンダー | ハードコード | Apple |
| 製造元 | ハードコード | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次のように作成できます。

| キー | Source | 値（例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコード | REF30 |

#### Android {#android}

[Android](https://developer.android.com/about/versions) を実行するデバイスの `X-Device-Info` ヘッダーを作成するには、次のドキュメントと以下のコードスニペットを参照します。

* [&#x200B; ビルド &#x200B;](https://developer.android.com/reference/android/os/Build.html) クラスのAndroid開発者向けドキュメント。

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

デバイス情報は、次のように構成できます。

| キー | Source | 値（例） |
|---------------|-----------------------------|-----------------|
| モデル | Build.MODEL | GT-I9505 |
| ベンダー | Build.BRAND | samsung |
| 製造元 | Build.MANUFACTURER | samsung |
| version | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | ハードコード | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

接続情報は、次のように作成できます。

| キー | Source | 値（例） |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコード | REF30 |

### テレビ接続デバイス {#tv-connected-devices}

#### tvOS {#tvos}

[tvOS](https://developer.apple.com/documentation/tvos-release-notes) を実行するデバイスの `X-Device-Info` ヘッダーを作成するには、次のドキュメントと以下のコードスニペットを参照してください。

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice) のApple開発者向けドキュメント。
* Apple開発者向けドキュメント [&#x200B; 到達可能性 &#x200B;](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)。
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html) に関する Linux マニュアルのドキュメント。

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

デバイス情報は、次のように構成できます。

| キー | Source | 値（例） |
|---------------|------------------------|-----------------|
| モデル | uname.machine | AppleTV |
| ベンダー | ハードコード | Apple |
| 製造元 | ハードコード | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10.2 |

接続情報は、次のように作成できます。

| キー | Source | 値（例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコード | REF30 |

#### Fire OS {#fireos}

[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html) を実行するデバイスの `X-Device-Info` ヘッダーを作成するには、次のドキュメントを参照してください。

* [&#x200B; ビルド &#x200B;](https://developer.android.com/reference/android/os/Build.html) クラスのAndroid開発者向けドキュメント。
* Amazon開発者向けドキュメント [Fire TV デバイスの識別 &#x200B;](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html)。

デバイス情報は、次のように構成できます。

| キー | Source | 値（例） |
|---------------|-----------------------------|-----------------|
| モデル | Build.MODEL | AFTM |
| ベンダー | Build.BRAND | Amazon |
| 製造元 | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | ハードコード | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

接続情報は、次のように作成できます。

| キー | Source | 値（例） |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコード | REF30 |

#### Roku OS {#rokuos}

[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md) を実行するデバイスの `X-Device-Info` ヘッダーを作成するには、次のドキュメントを参照してください。

* [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md) 用の Roku 開発者向けドキュメント。

デバイス情報は、次のように構成できます。

| キー | Source | 値（例） |
|---------------|--------------------------------------------|-----------------|
| モデル | ハードコード | 「Roku」 |
| ベンダー | ifDeviceInfo.GetModelDetails().VendorName | 「シャープ」「Roku」 |
| 製造元 | ifDeviceInfo.GetModelDetails().VendorName | 「シャープ」「Roku」 |
| version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | ハードコード | 「Roku」 |
| osVersion | ifDeviceInfo.getVersion() |                 |

接続情報は、次のように作成できます。

| キー | Source | 値（例） |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | ハードコード | 接続がワイヤリングされている場合は true |

アプリケーション情報は、次の方法で作成できます。

| キー | Source | 値（例） |
|---------------|-----------|-----------------|
| applicationId | ハードコード | REF30 |

### その他 {#others}

ドキュメントに記載されていないデバイスプラットフォームの場合、クライアント情報（デバイス、接続、アプリケーション）は、通常、デバイスのハードウェアおよび OS のマニュアルに記載されている利用可能なハードウェアおよび OS の属性にリンクする必要があります。
