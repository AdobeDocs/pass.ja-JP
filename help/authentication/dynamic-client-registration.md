---
title: 動的なクライアント登録
description: 動的なクライアント登録
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# 動的なクライアント登録 {#dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## コンテキスト {#context}

最新のセキュリティ対策に合わせて、UX とプラットフォームの所有者を改善しました
Adobe Pass認証Android SDK およびiOS SDK の要件は、[Apple Chromeのカスタムタブ ](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} および [Android Safari ビューコントローラー ](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank} の導入の方向に向かっています。

現在の AdobePass 実装では、MVPD ログインページを表示する web 環境を提供するために、プラットフォーム固有の web ビューを使用しています。 これらの web ビューは、Platform ブラウザーと資格情報管理を共有しないので、Adobe Pass Authentication アプリケーションを使用する際に、ブラウザーで保存されたパスワードを使用できません。 さらに、セキュリティ上の理由から、一部のプラットフォームでは、認証タスク用の WebView コントローラーが非推奨（廃止予定）になっています。 GoogleとAppleはどちらも、「Chromeのカスタムタブ」や「Safari ビューコントローラー」などの代替オプションを提供します。 これらは基本的に、それぞれのブラウザーの単一使用タブです。 Adobe Pass Authentication では、2018 年にこれらの新しいコンポーネントを採用する予定です。

## 詳細 {#details}

現在、Adobe Pass認証でアプリケーションを識別して登録する方法は 2 つあります。

* ブラウザーベースのクライアントは、許可されたドメインリストで登録されます
* iOSやAndroid アプリケーションなどのネイティブアプリケーションクライアントは、署名済みのリクエストメカニズムを使用して登録されます

Adobe Passは、Chromeのカスタムタブおよび Safari ビューコントローラーの新しいフローに対応するために、新しいアプリケーションを登録するための新しいクライアント登録メカニズムを提案します。 このメカニズムを使用すると、アプリケーションをより安全かつ詳細に制御でき、すべてのプラットフォームでアプリケーションを登録するために使用できます。

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
