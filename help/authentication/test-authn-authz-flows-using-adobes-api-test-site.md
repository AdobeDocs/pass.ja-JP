---
title: Adobeの API テストサイトを使用した認証フローと承認フローのテスト方法
description: Adobeの API テストサイトを使用した認証フローと承認フローのテスト方法
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Adobeの API テストサイトを使用して認証フローと認証フローをテストする方法 {#How-to-test-auth-flows}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

AuthN フローと AuthZ フローをテストするために、 **API テストサイト** それは君の自由に使える サポートチームがお客様に資格情報を提供します。 お問い合わせ先： **support@tve.zendesk.com**.


## 第 1 部 {#part-I}

RELEASE 環境に対するテストの場合は、第 II 部に直接進んでください。  事前認定環境でテストするには、 [事前に実行する環境とテストの設定](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## 第 2 部

第 1 部を完了したら、次の手順に従います。


1. ウェブページを開く： [ステージング API テスト](https://sp.auth-staging.adobe.com/apitest/api.html).
1. 次の URL からアクセスイネーブラを読み込みます。
   * [ステージング用のイネーブラ javascript にアクセス](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * または
   * [実稼動用のイネーブラ javascript へのアクセス](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * 次に、「**Load Access Enabler**」ボタンをクリックします。
1. 次に、要求元 ID の値を「 」に設定します。**requestorID**」をクリックし、「setRequestor」ボタンをクリックします。
1. その後、「getAuthentication」ボタンを押し、表示ピッカーが表示されるのを待ちます。
1. 「**MVPD**」と入力します。
1. 「**MVPD**」ログインページが表示されます。
1. リダイレクト後に、手順 1 ～ 3 をやり直します。
1. 「setAuthenticationStatus」の手順 3 をやり直すと、値「1」が表示されます。 認証が機能しなかった場合は、MVPD ダイアログボックスが表示されます。
1. 認証をテストするには、リソース入力フィールドに&quot;**requestorID**」をクリックし、「getAuthorization」ボタンをクリックします。
1. その結果、「setToken」 —\>「resource id」テキストボックスにはリソースが表示され、「setToken」 —\>「token」テキストボックスには shortAuthorizationToken が表示され、authZ が成功したことを意味します。
1. これで、「ログアウト」ボタンをクリックしてトークンを削除できます。
