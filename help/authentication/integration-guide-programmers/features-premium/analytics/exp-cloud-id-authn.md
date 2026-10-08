---
title: Adobe Pass認証でのExperience Cloud IDの使用
description: Adobe Pass認証でのExperience Cloud IDの使用
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%
---
# Adobe Pass認証でのExperience Cloud IDの使用

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

## Experience Cloud IDとは何ですか？また、その取得方法は？ {#what-exp-cloud-id-obtain}

Experience Cloud ID （略してECID）は、アプリケーションまたはweb サイト内の個々のユーザーに対してAdobe Experience Cloudによって生成された一意のIDです。 ECIDは、複数のアプリケーションやweb サイトをまたいで特定のユーザーに関する情報をリンクするために使用されているすべてのExperience Cloud レポートで頻繁に使用されます。

訪問者IDを提供するシステムを既に導入している場合は、このドキュメントの範囲に同じIDを使用する必要があります。

ECIDを取得する方法の1つは、Experience Cloud ID サービスを使用することです。 TDM、JS ライブラリ、サーバーサイド、ダイレクト統合、モバイルプラットフォーム用のネイティブライブラリなどにもとづいて、好みの実装タイプを使用できます。 利用可能なサービス、ライブラリ、SDKの実装ガイドの包括的なビューについては、<https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>を参照してください。

## Adobe Pass認証でExperience Cloud IDを使用するメリットは何ですか？ {#benefit-ex-cloud-id}

ECIDを使用するようにSDKとクライアントレス REST APIを設定すると、後でAdobe Pass Authenticationによって収集されたデータを既存のExperience Cloud ソリューションにリンクできるようになります。 これにより、Adobeが提供するあらゆるソリューションをまたいで、カスタマージャーニーとエクスペリエンスをより詳細に把握できるようになります。

## Adobe Pass認証でExperience Cloud IDを使用する方法 {#how-to-ex-cloud-id-authn}

ECID （上記）を取得した後、この情報をSDKとクライアントレス REST APIに渡す必要があります。 この情報は、後でSDKが行う各ネットワーク呼び出しでサーバーに渡されます。 設定プロセスは、次のようにSDKごとに異なります。

### JS SDK {#js-sdk}

JavaScriptの場合は、マップ内のECIDを3番目のパラメーターとしてsetRequestor呼び出しに渡す必要があります。

**使用例：**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

IOS/tvOS SDKには、setOptionsという専用のメソッドがあります。

**使用例：**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Android/fireTV SDKの場合、この仕組みはiOSに似ています。 パラメーター名だけが異なります。 APIのドキュメントはここにあります。

**使用例：**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### クライアントレス API {#clientless-api}

REST API v1を介してAdobe Passを使用する場合、すべてのAPI **で** ECID **値を**&#39;ap_vi&#39;**という名前のパラメーターとして**&#x200B;送信する必要があります。

**使用例：**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST API V2 {#rest-api-v2}

REST API v2を介してAdobe Passを使用する場合、**ECID**&#x200B;の値は、すべてのAPI **で**&#x200B;という名前のヘッダーとして&#x200B;**送信する必要があります。&#39;AP-Visitor-Identifier&#39;**.

**使用例：**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
ヘッダー：\
`AP-Visitor-Identifier: THE_ECID_VALUE`

