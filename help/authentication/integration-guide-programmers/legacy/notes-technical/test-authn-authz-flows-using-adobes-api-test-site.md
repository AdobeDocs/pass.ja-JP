---
title: Adobe API テストサイトを使用して認証フローと承認フローをテストする方法
description: Adobe API テストサイトを使用して認証フローと承認フローをテストする方法
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 65475d6da7a1b25cb2d8ebd6229a7cb360c7ab4a
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# （従来）Adobe API テストサイトを使用した認証フローと承認フローのテスト方法 {#How-to-test-auth-flows}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

AuthN および AuthZ フローをテストするために、**API テストサイトを準備しました。これは** 自由に使用できます。 アドビのサポートチームが喜んで資格情報を提供します。 お問い合わせは **tve-support@adobe.com** まで。


## 第 1 部 {#part-I}

リリース環境に対するテストについては、パート 2 に直接スキップします。  事前認定環境でテストするには、[&#x200B; 環境の設定と事前認定でのテスト &#x200B;](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md) を参照してください。

## 第 2 部

第 1 部を完了したら、次の手順を実行します。


1. Web ページ [&#x200B; ステージング API テスト &#x200B;](https://sp.auth-staging.adobe.com/apitest/api.html) を開きます。
1. 次の方法でアクセス イネーブラをロードします。
   * アクセスする場所（ステージングまたは実稼動）とデバッグモードにする必要があるかどうかのドロップダウンメニューからの選択
   * テストに使用するソフトウェア・ステートメントの入力
   * 次に、「**Access Enabler のロード**」ボタンをクリックします。
1. 次に、リクエスター ID の値を「**requestorID**」に設定し、「setRequestor」ボタンをクリックします。
1. その後、「getAuthentication」ボタンを押して、ディスプレイピッカーが表示されるのを待ちます。
1. ピッカーから「**MVPD**」を選択します。
1. 「**MVPD**」ログインページに資格情報を入力します。
1. リダイレクトされて戻された後、手順 1 ～ 3 をやり直します
1. 「setAuthenticationStatus」の手順 3 をやり直すと、値「1」が表示されます。 認証が機能しなかった場合は、MVPD ダイアログボックスが表示されます。
1. 認証をテストするには、「checkAuthorization」と「getAuthorization」のラベルが付いたボタンの右側の入力フィールドに、認証する **resource** を入力し、「getAuthorization」ボタンをクリックします。
1. その結果、「setToken」 – \> 「resource id」テキストボックスにリソースが表示され、「setToken」 – \> 「token」テキストボックスに shortAuthorizationToken が表示され、authZ が成功したことが示されます。
1. これで、「ログアウト」ボタンをクリックしてトークンを削除できます。
