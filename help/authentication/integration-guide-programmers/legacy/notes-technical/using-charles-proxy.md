---
title: Charles プロキシの使用
description: Charles プロキシの使用
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# （レガシー） Charles プロキシの使用 {#using-charles-proxy}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

**Charles:** <http://charlesproxy.com>


## Charles プロキシをダウンロード、インストール、使用の手引き {#download-install-and-get-stared-with-charles-proxy}

- **ダウンロード** - <http://www.charlesproxy.com/download/>
- **インストール** - <http://www.charlesproxy.com/documentation/installation/>
- **はじめに** - <http://www.charlesproxy.com/documentation/getting-started/>


## 構造タブとシーケンスタブ {#structure-vs-sequence-tabs}

トラフィックを表示する方法は 2 つあります。

1. **構造** - リクエストはホスト別にグループ化されます
1. **シーケンス** - リクエストは、呼び出された順序でリストされます


## SSL と証明書 {#ssl-and-certificates}

SSL プロキシ `\[ *Proxy -\> Proxy Settings... -\> SSL* \]` を有効にする

「SSL プロキシを有効にする」チェックボックスをオンにして、すべての HTTPS の場所を追加します。

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL プロキシ - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL 証明書 – <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- モバイルデバイスからの SSL プロキシ – 以下を参照してください。


## ホストを無視/除外 {#ignore-/-exclude-hosts}

出力が散乱しすぎた場合は、場所を無視または除外することを選択できます。次のいずれかの方法で、場所を無視または除外できます。

- 無視したいリクエストを右クリックし、「無視」を選択します。
- `\[ *Proxy -\> Recording Settings... -\> Exclude* \]` から除外する場所を手動で追加する


## DNS スプーフィング {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS スプーフィングは、要求を別の IP にリダイレクトしようとした場合、特にモバイルデバイスを操作している場合に非常に便利です。

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## リモートのマップ {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Map remote を使用すると、「受信」リクエストを別のエンドポイントにリダイレクトできます。 この機能の最も一般的なユースケースは、`AccessEnabler.swf` を `AccessEnablerDebug.swf:` に「マッピング」することです

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## リバースプロキシ {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## モバイル {#mobile}

### iOS デバイスでの Charles の使用（iPhone/iPad） {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### iPhoneからの SSL 接続 {#ssl-connection-from-iphone}

iOS デバイスから <http://charlesproxy.com/charles.crt> を参照します。  証明書のインストールダイアログが開きます。

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

「`\[ *Install*... *Install*... *Done* \]`」をクリックして、証明書のインストールを完了します。

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### iOS デバイスからの Charles の使用 {#using-charles-from-an-ios-device}

iOS デバイスで、「`\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`」を選択します。 ネットワークの横にある小さな青い矢印をクリックし、HTTP プロキシに移動して「手動」を選択します。


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
ここでは、Charles を実行しているマシンの IP とポートを指定する必要があります。 <span style="line-height: 1.6em;">iOS デバイスで Safari を開いて web ページを開こうとすると、Charles が稼働しているマシンで次のポップアップが表示されます。

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
「許可」をクリックして、デバイスが Charles を使用してそのすべてをプロキシ化できるようにします
リクエスト。

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS – 証明書を信頼する {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS認証エラー – adobepass.ios.app が見つかりません

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Charles for Androidの使用

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Android デバイスから [Charles プロキシ &#x200B;](http://charlesproxy.com/charles.crt) を参照します。
