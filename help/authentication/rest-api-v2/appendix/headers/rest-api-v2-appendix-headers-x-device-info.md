---
title: ヘッダー – X-Device-Info
description: REST API V2 - ヘッダー – X-Device-Info
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# ヘッダー – X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

<b>X-Device-Info</b> リクエストヘッダーには、実際のストリーミングデバイスに関連するクライアント情報（デバイス、接続、アプリケーション）が含まれます。

## 構文 {#syntax}

<table>
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

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">キー</th>
        <th style="background-color: #EFF2F7;">説明</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Presence</th>
        <th style="background-color: #EFF2F7;">可能な値</th>
    </tr>
    <tr>
        <td>primary ハードウェアタイプ</td>
        <td>デバイスの主要なハードウェアの種類。</td>
        <td></td>
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
        <td>モデル</td>
        <td>デバイスのモデル名。</td>
        <td><i>必須</i></td>
        <td>例：iPhone、SM-G930V、AppleTV など</td>
    </tr>
    <tr>
        <td>version</td>
        <td>デバイスのバージョン。</td>
        <td></td>
        <td>例：2.0.1 など</td>
    </tr>
    <tr>
        <td>製造元</td>
        <td>デバイスの製造会社/組織。</td>
        <td></td>
        <td>例：Samsung、LG、ZTE、Huawei、Motorola、Appleなど</td>
    </tr>
    <tr>
        <td>ベンダー</td>
        <td>デバイスの販売会社/組織。</td>
        <td></td>
        <td>例：Apple、Samsung、LG、Googleなど</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>デバイスのオペレーティングシステム（OS）名。</td>
        <td><i>必須</i></td>
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
        <td>osFamily</td>
        <td>デバイスのオペレーティングシステム（OS）グループ名。</td>
        <td></td>
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
        <td>osVendor</td>
        <td>デバイスのオペレーティングシステム（OS）のサプライヤ。</td>
        <td></td>
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
        <td>osVersion</td>
        <td>デバイスのオペレーティングシステム（OS）のバージョン。</td>
        <td></td>
        <td>例：10.2、9.0.1 など</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>ブラウザーの名前。</td>
        <td></td>
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
        <td>browserVendor</td>
        <td>ブラウザーの建物会社/組織。</td>
        <td></td>
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
        <td>browserVersion</td>
        <td>デバイスのブラウザーのバージョン。</td>
        <td></td>
        <td>例：60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>デバイスのユーザーエージェント。</td>
        <td></td>
        <td>例：Mozilla/5.0 （Macintosh、Intel Mac OS X 10_12_3） AppleWebKit/602.4.8 （KHTML、Gecko など） Version/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>デバイスの物理的な画面の幅。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>デバイスの物理的な画面の高さ。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>デバイスの物理的な画面のピクセル密度。</td>
        <td></td>
        <td>例：294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>デバイスの物理的な画面の斜めの寸法（インチ）。</td>
        <td></td>
        <td>例：5.5、10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>HTTP リクエストの送信に使用するデバイスの IP。</td>
        <td></td>
        <td>例 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>HTTP リクエストの送信に使用するデバイスのポート。</td>
        <td></td>
        <td>例：53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>ネットワーク接続タイプ。</td>
        <td></td>
        <td>例：WiFi、LAN、3G、4G、5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>ネットワーク接続のセキュリティの状態。</td>
        <td></td>
        <td>
            値は制限されています。
            <ul>
                <li>true – 安全なネットワークの場合</li>
                <li>false – 公共のホットスポットの場合</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>アプリケーションの一意の ID。</td>
        <td></td>
        <td>例：CNN</td>
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
