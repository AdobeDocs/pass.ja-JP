---
title: Charles プロキシの使用
description: Charles プロキシの使用
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Charles プロキシの使用 {#using-charles-proxy}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


**Charles:** <http://charlesproxy.com>


## Charles Proxy のダウンロード、インストール、使用の手引き {#download-install-and-get-stared-with-charles-proxy}

- **ダウンロード** - <http://www.charlesproxy.com/download/>
- **インストール** - <http://www.charlesproxy.com/documentation/installation/>
- **はじめに** - <http://www.charlesproxy.com/documentation/getting-started/>


## 「構造」タブと「シーケンス」タブ {#structure-vs-sequence-tabs}

トラフィックを表示する方法は 2 つあります。

1. **構造**  — リクエストはホスト別にグループ化されます
1. **シーケンス** ：リクエストは、呼び出された順序で表示されます


## SSL および証明書 {#ssl-and-certificates}

SSL プロキシの有効化 `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

「SSL プロキシを有効にする」チェックボックスをオンにして、すべての HTTPS の場所を追加します。


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)



- SSL プロキシ — <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL 証明書 — <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- モバイルデバイスからの SSL プロキシ — 以下を参照してください。


## ホストを無視/除外 {#ignore-/-exclude-hosts}

出力が乱雑になりすぎた場合は、場所を無視または除外するように選択できます。次のいずれかの操作を行うことで、場所を無視または除外できます。

- 無視するリクエストを右クリックし、「無視」を選択します。
- 除外する場所を手動で追加します `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## DNS スプーフィング {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



DNS スプーフィングは、特にモバイルデバイスで作業する際に、別の IP に要求をリダイレクトする場合に非常に役立ちます。

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## リモートをマッピング {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



map remote を使用すると、「受信」リクエストを別のエンドポイントにリダイレクトできます。 この機能の最も一般的な使用例は、「マッピング」です。 `AccessEnabler.swf` から `AccessEnablerDebug.swf:`

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## リバースプロキシ {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## モバイル {#mobile}

### iOSデバイスでの Charles の使用 (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### iPhoneからの SSL 接続 {#ssl-connection-from-iphone}

参照先 <http://charlesproxy.com/charles.crt> をiOSデバイスから削除します。  これにより、証明書のインストールダイアログが開始します。

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)

</br>

クリック `\[ *Install*... *Install*... *Done* \]` 証明書のインストールを完了するには、をクリックします。

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### iOSデバイスからの Charles の使用 {#using-charles-from-an-ios-device}

iOSデバイスで、 `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. ネットワークの横にある小さな青い矢印をクリックし、HTTP プロキシに移動して、「手動」を選択します。


</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)


</br>
ここで、Charles を実行しているマシンの IP とポートを指定する必要があります。 <span style="line-height: 1.6em;">iOSデバイスで Safari を開いて Web ページを開こうとすると、Charles を実行しているマシンで次のポップアップが表示されます。

</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)

</br>
「許可」をクリックして、デバイスが Charles を使用してすべてのリクエストをプロキシできるようにします。

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS — 任意の証明書を信頼する {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS認証エラー — adobepass.ios.app が見つかりません

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Android での Charles の使用

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


参照先 [Charles プロキシ](http://charlesproxy.com/charles.crt) を Android デバイスから削除します。
