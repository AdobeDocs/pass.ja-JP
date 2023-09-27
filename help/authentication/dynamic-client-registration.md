---
title: 動的クライアントの登録
description: 動的クライアントの登録
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# 動的クライアントの登録 {#dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## コンテキスト {#context}

最新のセキュリティプラクティス、改善された UX とプラットフォームの所有者要件に合わせて、Adobe Pass Authentication Android SDK とiOS SDK は、 [Android Chrome のカスタムタブ](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari view controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

現在の AdobePass の実装では、MVPD ログインページを表示する Web 環境を提供するために、プラットフォーム固有の Web ビューを使用します。 これらの Web ビューは、資格情報の管理を Platform ブラウザーと共有しないので、Adobe Pass Authentication アプリケーションを使用する際に、ブラウザーで保存したパスワードを使用することはできません。 また、セキュリティ上の理由から、一部のプラットフォームでは、認証タスク用の WebView コントローラを廃止する予定です。 GoogleとAppleの両方に、「Chrome カスタムタブ」や「Safari ビューコントローラー」などの代替オプションが用意されています。 これらは基本的に、それぞれのブラウザーの単一使用タブです。 Adobe Pass認証では、2018 年にこれらの新しいコンポーネントが採用されます。

## 詳細 {#details}

現在、Adobe Pass Authentication がアプリケーションを識別して登録する方法は 2 つあります。

* ブラウザーベースのクライアントは、許可されたドメインリストを介して登録されます。
* iOSや Android アプリケーションなどのネイティブアプリケーションクライアントは、署名された要求者メカニズムを通じて登録されます。

Chrome のカスタムタブと Safari View Controller の新しいフローに対処するため、Adobe Passは新しいアプリケーションを登録するための新しいクライアント登録メカニズムを提案しています。 このメカニズムにより、アプリケーションをより安全かつ詳細に制御でき、すべてのプラットフォームでアプリケーションを登録するために使用できます。

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
