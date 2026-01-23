---
title: Adobe Pass認証でのExperience Cloud ID の使用
description: Adobe Pass認証でのExperience Cloud ID の使用
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---

# Adobe Pass認証でのExperience Cloud ID の使用

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## Experience Cloud ID とは何ですか？また、その取得方法を教えてください。 {#what-exp-cloud-id-obtain}

Experience Cloud ID （ECID）は、アプリケーションまたは web サイトの個々のユーザーに対してAdobe Experience Cloudで生成される一意の ID です。 ECID は、複数のアプリケーションや web サイトをまたいで特定のユーザーに関する情報をリンクするために使用されるすべてのExperience Cloud レポートで頻繁に使用されます。

訪問者 ID を提供するシステムが既に導入されている場合は、このドキュメントの範囲にも同じ ID を使用する必要があります。

ECID を取得する 1 つの方法は、Experience Cloud ID サービスを使用することです。 TDM、JS ライブラリ、サーバー側、直接統合、モバイルプラットフォーム用のネイティブライブラリのいずれかに基づいて、好みの実装タイプを使用できます。 利用可能なサービス、ライブラリ、SDKのガイドおよび実装ガイドの包括的なビューについては、以下を参照してください。<https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=ja>

## Adobe Pass認証でExperience Cloud ID を使用する利点は何ですか？ {#benefit-ex-cloud-id}

SDK およびクライアントレス REST API を ECID を使用するように設定すると、後でAdobe Pass Authentication によって収集されたデータを既存のExperience Cloud ソリューションにリンクできるようになります。 これにより、Adobeが提供するすべてのソリューションを通じて、カスタマージャーニーとエクスペリエンスをより深く理解できます。

## Adobe Pass認証でのExperience Cloud ID の使用方法は？ {#how-to-ex-cloud-id-authn}

（前述の） ECID を取得したら、この情報を SDK とクライアントレス REST API に渡す必要があります。 この情報は、後でSDKが行う各ネットワーク呼び出しでサーバーに渡されます。 設定プロセスは、次のようにSDKごとに異なります。

### JS SDK {#js-sdk}

JavaScriptの場合、setRequestor 呼び出しの 3 番目のパラメーターとしてマップで ECID を渡す必要があります。

**使用例：**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

iOS/tvOS SDKの場合は、setOptions という専用のメソッドがあります。

**使用例：**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Android/fireTV SDKの場合、この仕組みはiOSに似ています。 パラメーター名のみが異なります。 API については、こちらを参照してください。

**使用例：**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### クライアントレス API {#clientless-api}

REST API v1 経由でAdobe Passを使用する場合は、**ECID** 値を **&#39;ap_vi&#39;** という名前のパラメーターとして **すべての API で** 送信する必要があります。

**使用例：**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST API V2 {#rest-api-v2}

REST API v2 経由でAdobe Passを使用する場合は、**ECID** 値を **すべての API で** 「**AP-Visitor-Identifier&#39;** という名前のヘッダーとして送信する必要があります。

**使用例：**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
ヘッダー：\
`AP-Visitor-Identifier: THE_ECID_VALUE`

