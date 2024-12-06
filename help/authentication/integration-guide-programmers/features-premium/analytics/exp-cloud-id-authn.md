---
title: Adobe Pass認証でのExperience CloudID の使用
description: Adobe Pass認証でのExperience CloudID の使用
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Adobe Pass認証でのExperience CloudID の使用

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## Experience CloudID とは何ですか？また、その取得方法を教えてください。 {#what-exp-cloud-id-obtain}

Experience CloudID （ECID）は、アプリケーションまたは web サイトの個々のユーザーに対してAdobe Experience Cloudで生成される一意の ID です。 ECID は、複数のアプリケーションや web サイトをまたいで特定のExperience Cloudに関する情報をリンクするために使用されるすべてのユーザーレポートで頻繁に使用されます。

訪問者 ID を提供するシステムが既に導入されている場合は、このドキュメントの範囲にも同じ ID を使用する必要があります。

ECID を取得する 1 つの方法は、Experience CloudID サービスを使用することです。 TDM、JS ライブラリ、サーバー側、直接統合、モバイルプラットフォーム用のネイティブライブラリのいずれかに基づいて、好みの実装タイプを使用できます。 利用可能なサービス、ライブラリ、SDK および実装ガイドの包括的なビューについては、以下を参照してください。<https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## Adobe Pass認証でExperience CloudID を使用する利点は何ですか？ {#benefit-ex-cloud-id}

SDK およびクライアントレス REST API を ECID を使用するように設定すると、後でAdobe PassExperience Cloudによって収集されたデータを既存の認証ソリューションにリンクできるようになります。 これにより、Adobeが提供するすべてのソリューションを通じて、カスタマージャーニーとエクスペリエンスをより深く理解できます。

## Adobe Pass認証でのExperience CloudID の使用方法は？ {#how-to-ex-cloud-id-authn}

（前述の） ECID を取得したら、この情報を SDK とクライアントレス REST API に渡す必要があります。 この情報は、後で SDK が行う各ネットワーク呼び出しでサーバーに渡されます。 次のように、設定プロセスは SDK ごとに異なります。

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

iOS/tvOS SDK の場合、setOptions という専用のメソッドがあります。

**使用例：**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Android/fireTV SDK の場合、仕組みはiOSに似ています。 パラメーター名のみが異なります。 API については、こちらを参照してください。

**使用例：**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### クライアントレス API {#clientless-api}

REST API 経由でAdobe Passを使用する場合は、**ECID** 値を **&#39;ap_vi&#39;** という名前のパラメーターとして **すべての API で** 送信する必要があります。

**使用例：**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
