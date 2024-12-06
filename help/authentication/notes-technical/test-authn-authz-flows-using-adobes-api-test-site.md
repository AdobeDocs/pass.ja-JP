---
title: Adobeの API テストサイトを使用して認証および承認フローをテストする方法
description: Adobeの API テストサイトを使用して認証および承認フローをテストする方法
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# Adobeの API テストサイトを使用して認証および承認フローをテストする方法 {#How-to-test-auth-flows}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

AuthN および AuthZ フローをテストするために、**API テストサイトを準備しました。これは** 自由に使用できます。 アドビのサポートチームが喜んで資格情報を提供します。 お問い合わせは **support@tve.zendesk.com** まで。


## 第 1 部 {#part-I}

リリース環境に対するテストについては、パート 2 に直接スキップします。  事前認定環境でテストするには、[ 環境の設定と事前認定でのテスト ](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md) を参照してください。

## 第 2 部

第 1 部を完了したら、次の手順を実行します。


1. Web ページ [ ステージング API テスト ](https://sp.auth-staging.adobe.com/apitest/api.html) を開きます。
1. 次の URL からアクセス イネーブラをロードします。
   * [ ステージング用の Access Enabler JavaScript](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js)。
   * または
   * [ 本番用の Access Enabler Javascript](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js)。
   * 次に、「**Access Enabler のロード**」ボタンをクリックします。
1. 次に、リクエスター ID の値を「**requestorID**」に設定し、「setRequestor」ボタンをクリックします。
1. その後、「getAuthentication」ボタンを押して、ディスプレイピッカーが表示されるのを待ちます。
1. ピッカーから「**MVPD**」を選択します。
1. 「**MVPD**」ログインページに資格情報を入力します。
1. リダイレクトされて戻された後、手順 1 ～ 3 をやり直します
1. 「setAuthenticationStatus」の手順 3 をやり直すと、値「1」が表示されます。 認証が機能しなかった場合は、MVPD ダイアログボックスが表示されます。
1. 認証をテストするには、リソース入力フィールドに「**requestorID**」と入力し、「getAuthorization」ボタンをクリックします。
1. その結果、「setToken」 – \> 「resource id」テキストボックスにリソースが表示され、「setToken」 – \> 「token」テキストボックスに shortAuthorizationToken が表示され、authZ が成功したことが示されます。
1. これで、「ログアウト」ボタンをクリックしてトークンを削除できます。
