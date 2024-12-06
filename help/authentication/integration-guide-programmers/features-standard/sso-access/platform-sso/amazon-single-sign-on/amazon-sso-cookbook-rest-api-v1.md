---
title: Amazon SSO クックブック（REST API V1）
description: Amazon SSO クックブック（REST API V1）
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Amazon SSO クックブック（REST API V1） {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

Adobe Pass認証 REST API V1 は、FireOS で動作するクライアントアプリケーションのエンドユーザーに対して、Platform シングルサインオン（SSO）をサポートしています。

このドキュメントは、既存の [REST API V1 概要 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md) の拡張機能として機能し、概要を示します。

## platform id フローを使用したAmazonのシングルサインオン {#cookbook}

### 前提条件 {#prerequisites}

プラットフォーム ID フローを使用してAmazonのシングルサインオンに進む前に、次の前提条件が満たされていることを確認してください。

#### Amazon SSO SDK の統合 {#integrate-amazon-sso-sdk}

ストリーミングアプリケーションは、シングルサインオン（SSO）用の [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) ライブラリをビルドに組み込む必要があります。

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

#### Amazon SSO SDK の使用 {#use-amazon-sso-sdk}

ストリーミングアプリケーションは、Amazon SSO SDK を使用して SSO トークン（プラットフォーム ID）ペイロードを取得する必要があります。

Amazon SSO SDK は、SSO トークン（プラットフォーム ID）ペイロードを取得するための同期 API と非同期 API の両方を提供します。

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
   * ストリーミングアプリケーションは、AmazonおよびAdobeの担当者に連絡して調査する場合があります。

### ワークフロー {#workflow}

Amazon SSO トークン（プラットフォーム ID）ペイロードは、Adobe Pass認証エンドポイントに対して行われたすべての HTTP リクエストに存在する必要があります。

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> ストリーミングアプリケーションでは、`/authenticate` 呼び出し時に提供されたように、Amazon SSO トークン（プラットフォーム ID）ペイロードの送信をスキ `/regcode` プする場合があります。

Adobe Pass認証は、デバイススコープまたはプラットフォームスコープの識別子である SSO トークン（platform id）ペイロードを受け取るために次のメソッドをサポートしています。

* `Adobe-Subject-Token` という名前のヘッダーとして。
* `ast` という名前のクエリパラメーターとして
* 次の名前の post パラメーターとして、`ast`

>[!IMPORTANT]
>
> クエリパラメーターとして送信すると、URL 全体が非常に長くなり、拒否される場合があります。
>
> クエリ/POST パラメーターとして送信される場合は、リクエスト署名の生成時にそれを含める必要があります。

#### サンプル

**ヘッダーとしての送信**

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

**POST パラメーターとして送信**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> `Adobe-Subject-Token` ヘッダーまたは `ast` パラメーターの値が見つからないか無効な場合、Adobe Pass Authentication は、シングルサインオンを考慮せずにリクエストを処理します。
