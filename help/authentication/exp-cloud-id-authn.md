---
title: Adobe Pass認証でのExperience CloudID の使用
description: Adobe Pass認証でのExperience CloudID の使用
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---

# Adobe Pass認証でのExperience CloudID の使用

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## Experience CloudID とは何ですか？その取得方法は何ですか？ {#what-exp-cloud-id-obtain}

Experience CloudID（短くは ECID）は、Adobe Experience Cloudがアプリケーション/Web サイトの個々のユーザーに対して生成する一意の ID です。 ECID は、複数のExperience Cloud/Web サイト間で特定のユーザーに関する情報をリンクするために使用される、すべてのアプリケーションレポートで頻繁に使用されます。

訪問者 ID を提供するシステムが既にある場合、このドキュメントの範囲で同じ ID を使用する必要があります。

ECID を取得する方法の 1 つは、Experience CloudID サービスを使用することです。 TDM、JS ライブラリ、サーバー側、直接統合、またはモバイルプラットフォーム用のネイティブライブラリに基づいて、好みの実装タイプを使用できます。 利用可能なサービス、ライブラリ、SDK および実装ガイドの包括的なビューについては、以下を参照してください。 <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## Adobe Pass認証でExperience CloudID を使用するメリットは何ですか。 {#benefit-ex-cloud-id}

ECID を使用するように SDK およびクライアントレス REST API を設定すると、後でAdobe Pass認証によって収集されたデータを既存のExperience Cloudソリューションにリンクできます。 これにより、Adobeが提供するすべてのソリューションにわたって、顧客の遍歴と経験をより深く理解できます。

## Adobe Pass認証でのExperience CloudID の使用方法は？ {#how-to-ex-cloud-id-authn}

（上記で説明した）ECID を取得したら、この情報をアドビの SDK およびアドビのクライアントレス REST API に渡す必要があります。 この情報は、後で SDK がおこなう各ネットワーク呼び出しでアドビのサーバーに渡されます。 設定プロセスは、次のように SDK ごとに異なります。

### JS SDK {#js-sdk}

JavaScript の場合、ECID を setRequestor 呼び出しの 3 番目のパラメーターとしてマップに渡す必要があります。

**使用例：**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

iOS/tvOS SDK の場合は、setOptions という専用のメソッドがあります。

**使用例：**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Android/fireTV SDK の場合、メカニズムはiOSに似ています。 パラメーター名だけが異なります。 API については、こちらを参照してください。

**使用例：**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### クライアントレス API {#clientless-api}

Adobe Passを REST API 経由で使用する場合、 **ECID** 値を送信する必要があります **（すべての API）** という名前のパラメーター **&#39;ap_vi&#39;**.

**使用例：**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
