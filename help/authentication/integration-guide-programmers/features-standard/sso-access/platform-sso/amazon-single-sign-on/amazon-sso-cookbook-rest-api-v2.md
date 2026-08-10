---
title: Amazon SSO クックブック （REST API V2）
description: Amazon SSO クックブック （REST API V2）
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Amazon SSO クックブック （REST API V2） {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

Adobe Pass Authentication REST API V2は、FireOSで動作するクライアントアプリケーションのエンドユーザー向けに、Platform Single Sign-On （SSO）をサポートしています。

このドキュメントは、プラットフォーム ID フロー[&#128279;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)を使用して シングルサインオンを実装する方法を説明するドキュメントと、概要レベルのビューを提供する既存の[REST API V2概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)の拡張機能として機能します。

## Platform ID フローを使用したAmazon シングルサインオン {#cookbook}

Adobe Pass Authenticationは、Amazonと連携して、TV Everywhere アプリケーション全体でTV加入者のログインユーザーエクスペリエンスを向上させ、シングルサインオン（SSO）を促進します。

### 前提条件 {#prerequisites}

Platform ID フローを使用してAmazon シングルサインオンを続行する前に、次の前提条件を満たしていることを確認してください。

#### Amazon SSO SDKの統合 {#integrate-amazon-sso-sdk}

ストリーミングアプリケーションは、[Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) library for Single Sign-On （SSO）をビルドに統合する必要があります。

* 最新のAmazon SSO SDK ライブラリをダウンロードし、アプリケーションのディレクトリと並行して`/SSOEnabler` フォルダーにコピーします。

* Amazon SSO SDK ライブラリを使用するように、マニフェストファイルとGradle ファイルを更新します。

  **マニフェスト：**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  リポジトリの下：

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  依存関係で：

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Amazon SSO SDKの使用 {#use-amazon-sso-sdk}

ストリーミングアプリケーションは、SSO トークン（Platform ID）ペイロードを取得するために、Amazon SSO SDKを使用する必要があります。

Amazon SSO SDKは、SSO トークン（Platform ID）ペイロードを取得するための同期APIと非同期APIの両方を提供します。

ストリーミングアプリケーションは、アーキテクチャに基づいて2つのオプションのいずれかを選択できます。

##### 非同期API

* `SSOEnabler` インスタンスを取得し、`SSOEnablerCallback`を設定します。

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

  SSO トークン成功応答バンドルには、次のものが含まれます。
  * キー「SSOToken」を持つ`string`としてのSSO トークン。

  <br/>

  SSO トークン失敗応答バンドルには、次のものが含まれます。
  * エラーコードはキー「ErrorCode」を持つ`int`です。
  * キー「ErrorDescription」を持つ`string`としてのエラー説明。

  <br/>

* SSO トークンを取得します。

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  このAPIは、初期化中に設定されたコールバックを介して応答を提供します。

##### 同期API

* `SSOEnabler` インスタンスを取得：

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* SSO トークンを取得します。

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  このAPIは、呼び出し元のスレッドをブロックし、結果バンドルで応答します。 これは同期呼び出しなので、必ずメインスレッドで使用しないでください。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  このAPIは、同期呼び出しのタイムアウト値を設定します。 デフォルトのタイムアウト値は1分です。

#### Amazon SSOのフォールバック {#fallback-amazon-sso}

ストリーミングアプリケーションは、Amazon SSO フローから通常の認証フローへのフォールバックシナリオを処理する必要があります。

ストリーミングアプリケーションが処理していることを確認します。

* Amazon デバイスで実行する必要があるAmazon コンパニオンアプリケーションがありません。
  * ストリーミングアプリケーションで、次のクラス `com.amazon.ottssotokenlib.SSOEnabler`の実行時に`ClassNotFoundException`が発生する可能性があります。

* 上記のAPIから返されるSSO トークン（プラットフォーム ID）ペイロードがありません。
  * ストリーミングアプリケーションは、AmazonおよびAdobeの担当者に連絡して調査する場合があります。

### ワークフロー {#workflow}

Amazon SSO トークン（Platform ID）ペイロードは、Adobe Pass Authentication REST API V2 エンドポイントに対して行われたすべてのHTTP リクエストに存在する必要があります。

```
/api/v2/*
```

Adobe Pass Authentication REST API V2は、デバイス範囲またはプラットフォーム範囲のIDであるSSO トークン（プラットフォーム ID）ペイロードを受信するために、次のメソッドをサポートしています。

* ヘッダー名：`Adobe-Subject-Token`

>[!IMPORTANT]
> 
> `Adobe-Subject-Token` ヘッダーについて詳しくは、[Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) ドキュメントを参照してください。

#### サンプル

**ヘッダーとして送信**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> 「`Adobe-Subject-Token`」ヘッダー値が見つからないか無効な場合、Adobe Pass認証は、シングルサインオンを考慮せずにリクエストを処理します。
