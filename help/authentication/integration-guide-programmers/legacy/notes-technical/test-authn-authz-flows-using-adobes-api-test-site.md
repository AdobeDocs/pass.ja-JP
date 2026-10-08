---
title: AdobeのAPI テストサイトを使用して認証および認証フローをテストする方法
description: AdobeのAPI テストサイトを使用して認証および認証フローをテストする方法
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%
---
# （レガシー） AdobeのAPI テストサイトを使用して認証フローと認証フローをテストする方法 {#How-to-test-auth-flows}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

AuthNおよびAuthZ フローをテストするために、自由に使用できる&#x200B;**API テストサイト**&#x200B;を用意しました。 サポートチームが喜んで資格情報を提供します。 **tve-support@adobe.com**&#x200B;までご連絡ください。


## パート I {#part-I}

RELEASE環境に対するテストについては、パート IIに直接スキップしてください。  事前認定環境でテストを行うには、[事前認定環境の設定とテスト ](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)を参照してください。

## パート II

パート Iを完了したら、次の手順を実行します。


1. Web ページを開きます：[ ステージング API テスト ](https://sp.auth-staging.adobe.com/apitest/api.html)。
1. 次の方法でアクセス イネーブラを読み込む：
   * アクセスする場所（ステージングまたは実稼動）とデバッグモードである必要がある場合は、ドロップダウンメニューから選択します
   * テストするソフトウェア文の入力
   * 次に、「**Load Access Enabler**」ボタンをクリックします。
1. 次に、依頼者IDの値を「**requestorID**」に設定し、「setRequestor」ボタンをクリックします。
1. その後、「getAuthentication」ボタンを押して、ディスプレイピッカーが表示されるのを待ちます。
1. ピッカーから「**MVPD**」を選択します。
1. 「**MVPD**」ログインページに資格情報を入力します。
1. リダイレクトされた後、手順1 ～ 3をやり直します
1. 「setAuthenticationStatus」で手順3をもう一度実行すると、値「1」が表示されます。 認証が機能しなかった場合は、MVPD ダイアログボックスが表示されます。
1. 認証をテストするには、「checkAuthorization」と「getAuthorization」のラベルが付いたボタンの右側にある入力フィールドに、認証する&#x200B;**リソース**&#x200B;を入力し、「getAuthorization」ボタンをクリックします。
1. その結果、「setToken」 – \> 「resource id」テキストボックスにリソースが表示され、「setToken」 – \> 「token」テキストボックスにshortAuthorizationTokenが表示され、authZが成功したことを示します。
1. これで、「ログアウト」ボタンをクリックしてトークンを削除できます。
