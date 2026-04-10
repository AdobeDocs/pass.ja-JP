---
title: Charles Proxyの使用
description: Charles Proxyの使用
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# （レガシー） Charles プロキシの使用 {#using-charles-proxy}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

**チャールズ：** <http://charlesproxy.com>


## Charles Proxyのダウンロード、インストール、および使用開始 {#download-install-and-get-stared-with-charles-proxy}

- **ダウンロード** - <http://www.charlesproxy.com/download/>
- **インストール** - <http://www.charlesproxy.com/documentation/installation/>
- **はじめに** - <http://www.charlesproxy.com/documentation/getting-started/>


## 構造タブとシーケンスタブ {#structure-vs-sequence-tabs}

トラフィックを表示するには、次の2つの方法があります。

1. **構造** – 要求はホスト別にグループ化されます
1. **シーケンス** - リクエストは、呼び出される順序で一覧表示されます


## SSLと証明書 {#ssl-and-certificates}

SSL プロキシを有効にする`\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

「SSL プロキシを有効にする」チェックボックスをオンにして、すべてのHTTPSの場所を追加します。

<!-- 
NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL プロキシ - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL証明書 – <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- モバイルデバイスからのSSL プロキシ – 以下を参照してください。


## ホストの無視/除外 {#ignore-/-exclude-hosts}

出力が乱雑になりすぎた場合は、場所を無視するか除外するかを選択できます。次のいずれかの操作を行うことで、場所を無視または除外できます。

- 無視するリクエストを右クリックし、「無視」を選択します
- `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`から除外する場所を手動で追加


## DNS スプーフィング {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS スプーフィングは、リクエストを別のIPにリダイレクトしようとする場合、特にモバイルデバイスを操作する場合に非常に便利です。

<!-- 
NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Map Remote {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



map remoteを使用すると、「受信」リクエストを別のエンドポイントにリダイレクトできます。 この機能の最も一般的な使用例は、`AccessEnabler.swf`を`AccessEnablerDebug.swf:`に「マッピング」することです

<!-- 
NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## リバースプロキシ {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## モバイル {#mobile}

### IOS デバイスでのCharlesの使用（iPhone / iPad） {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### IPHONEからのSSL接続 {#ssl-connection-from-iphone}

お使いのiOS デバイスから<http://charlesproxy.com/charles.crt>を参照します。  これにより、証明書のインストールダイアログが開始されます。

<!-- 
NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

`\[ *Install*... *Install*... *Done* \]`をクリックして、証明書のインストールを完了します。

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### IOS デバイスからのCharlesの使用 {#using-charles-from-an-ios-device}

IOS デバイスで「`\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`」を選択します。 ネットワークの横にある青い小さな矢印をクリックし、「HTTP Proxy」に移動して「Manual」を選択します。


</br>

<!-- 
NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
ここでは、Charlesを実行しているマシンのIPとポートを指定する必要があります。 <span style="line-height: 1.6em;">iOS デバイスでSafariを開いてweb ページを開こうとすると、Charlesを実行しているコンピューターに次のポップアップが表示されます。

</br>

<!-- 
NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
「許可」をクリックして、デバイスがCharlesを使用してすべての属性をプロキシできるようにします
リクエスト：

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS – 任意の証明書を信頼する {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS認証エラー – adobepass.ios.appが見つかりません

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## AndroidでのCharlesの使用

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Android デバイスから[Charles proxy](http://charlesproxy.com/charles.crt)を参照します。
