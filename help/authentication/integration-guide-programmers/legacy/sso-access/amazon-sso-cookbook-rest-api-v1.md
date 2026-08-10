---
title: Amazon SSO クックブック （REST API V1）
description: Amazon SSO クックブック （REST API V1）
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 0%

---

# （レガシー） Amazon SSO クックブック （REST API V1） {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

Adobe Pass Authentication REST API V1は、FireOSで動作するクライアントアプリケーションのエンドユーザー向けに、Platform Single Sign-On （SSO）をサポートしています。

このドキュメントは、高レベルのビューを提供する既存の[REST API V1概要](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)の拡張機能として機能します。

## Platform ID フローを使用したAmazon シングルサインオン {#cookbook}

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

Amazon SSO トークン（Platform ID）ペイロードは、Adobe Pass認証エンドポイントに対して行われたすべてのHTTP リクエストに存在する必要があります。

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> ストリーミングアプリケーションは、`/regcode`呼び出しで提供されたように、`/authenticate`呼び出しのAmazon SSO トークン（Platform ID）ペイロードの送信をスキップする可能性があります。

Adobe Pass Authenticationは、デバイス範囲またはプラットフォーム範囲のIDであるSSO トークン（プラットフォーム ID）ペイロードを受信するために、次のメソッドをサポートしています。

* ヘッダー名：`Adobe-Subject-Token`
* 次の名前のクエリパラメーターとして：`ast`
* 次の名前の投稿パラメーターとして：`ast`

>[!IMPORTANT]
>
> クエリパラメーターとして送信された場合、URL全体が非常に長くなり、拒否される可能性があります。
>
> クエリ/ポストパラメーターとして送信される場合は、リクエスト署名の生成時に含める必要があります。

#### サンプル

**ヘッダーとして送信**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**クエリパラメーターとして送信**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**投稿パラメーターとして送信**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> `Adobe-Subject-Token` ヘッダーまたは`ast` パラメーター値が見つからないか無効な場合、Adobe Pass Authenticationはシングルサインオンを考慮せずにリクエストを処理します。
