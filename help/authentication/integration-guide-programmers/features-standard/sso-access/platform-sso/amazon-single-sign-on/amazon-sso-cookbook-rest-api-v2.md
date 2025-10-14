---
title: Amazon SSO クックブック（REST API V2）
description: Amazon SSO クックブック（REST API V2）
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Amazon SSO クックブック（REST API V2） {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

Adobe Pass認証 REST API V2 は、FireOS で動作するクライアントアプリケーションのエンドユーザーに対して、Platform シングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の [REST API V2 概要の拡張機能として機能し &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) 概要の概要と、[&#x200B; プラットフォーム ID フローを使用したシングルサインオン &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md) の実装方法を説明するドキュメントを提供します。

## platform id フローを使用したAmazonのシングルサインオン {#cookbook}

Adobe Pass Authentication はAmazonと協力して、ログインユーザーエクスペリエンスを向上し、TV サブスクライバーの TV Everywhere アプリケーション間でのシングルサインオン（SSO）を容易にします。

### 前提条件 {#prerequisites}

プラットフォーム ID フローを使用してAmazonのシングルサインオンに進む前に、次の前提条件が満たされていることを確認してください。

#### Amazon SSO SDKの統合 {#integrate-amazon-sso-sdk}

ストリーミングアプリケーションは、シングルサインオン（SSO）用の [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) ライブラリをビルドに統合する必要があります。

* 最新のAmazon SSO SDK ライブラリをダウンロードして、アプリケーションのディレクトリと並行して `/SSOEnabler` フォルダーにコピーします。

* マニフェストと Gradle ファイルを更新して、Amazon SSO SDK ライブラリを使用するようにします。

  **マニフェスト：**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  リポジトリで：

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  依存関係の下：

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Amazon SSO SDKの使用 {#use-amazon-sso-sdk}

ストリーミングアプリケーションは、Amazon SSO SDKを使用して、SSO トークン（プラットフォーム ID）ペイロードを取得する必要があります。

Amazon SSO SDKは、同期 API と非同期 API の両方を提供して、SSO トークン（プラットフォーム ID）ペイロードを取得します。

ストリーミングアプリケーションは、アーキテクチャに基づいて 2 つのオプションのいずれかを選択できます。

##### 非同期 API

* `SSOEnabler` インスタンスを取得し、`SSOEnablerCallback` を設定します。

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  これは、ストリーミングアプリケーションの初期化中に実行できます。

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  SSO トークンサクセス応答バンドルには、次の情報が含まれます。
   * キー「SSOToken」を持つ `string` としての SSO トークン。

  <br/>

  SSO トークン失敗応答バンドルには、次の内容が含まれます。
   * キー「ErrorCode」を含む `int` のエラーコード。
   * キー「ErrorDescription」を含む `string` のエラーの説明。

  <br/>

* SSO トークンを取得します。

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  この API は、初期化中に設定されたコールバックを介して応答を提供します。

##### 同期 API

* `SSOEnabler` インスタンスを取得します。

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* SSO トークンを取得します。

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  この API は呼び出し元スレッドをブロックし、結果のバンドルで応答します。 これは同期呼び出しなので、メインスレッドでは使用しないでください。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  この API は、同期呼び出しのタイムアウト値を設定します。 デフォルトタイムアウト値は 1 分です。

#### Amazon SSO のフォールバック {#fallback-amazon-sso}

ストリーミングアプリケーションは、Amazon SSO フローから通常の認証フローへのフォールバックシナリオを処理する必要があります。

ストリーミングアプリケーションで次の処理が行われていることを確認します。

* Amazon デバイス上で動作する必要のあるAmazon コンパニオンアプリケーションがない。
   * ストリーミングアプリケーションでは、次のクラス `com.amazon.ottssotokenlib.SSOEnabler` ードで実行時に `ClassNotFoundException` が発生する場合があります。

* 上記の API で返す必要のある SSO トークン（プラットフォーム ID）ペイロードがありません。
   * ストリーミングアプリケーションは、AmazonおよびAdobeの担当者に問い合わせて調査する場合があります。

### ワークフロー {#workflow}

Amazon SSO トークン（プラットフォーム ID）ペイロードは、Adobe Pass認証 REST API V2 エンドポイントに対して行われたすべての HTTP リクエストに存在する必要があります。

```
/api/v2/*
```

Adobe Pass認証 REST API V2 は、デバイススコープまたはプラットフォームスコープの識別子である SSO トークン（Platform ID）ペイロードを受け取るために次のメソッドをサポートしています。

* `Adobe-Subject-Token` という名前のヘッダーとして。

>[!IMPORTANT]
> 
> `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

#### サンプル

**ヘッダーとしての送信**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> `Adobe-Subject-Token` ヘッダー値がない場合や無効な場合、Adobe Pass Authentication はシングルサインオンを考慮せずにリクエストを処理します。
