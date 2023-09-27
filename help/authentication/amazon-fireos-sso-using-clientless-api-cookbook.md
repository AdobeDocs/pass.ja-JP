---
title: クライアントレス API クックブックを使用したAmazon FireOS SSO
description: クライアントレス API クックブックを使用したAmazon FireOS SSO
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# クライアントレス API クックブックを使用したAmazon FireOS SSO {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

## はじめに {#Introduction}

このドキュメントでは、クライアントレス API を使用してAmazonの SSO バージョンのAdobe Pass認証フローを実装する手順を説明します。 このドキュメントの最初の部分では、Amazonバージョンのアーキテクチャに対する特異性に焦点を当てます。これは、実装に既に慣れており、経験豊富な多くのパートナーが対象とします。

このドキュメントの第 2 部では、Adobe Pass Authentication クライアントレス API を実装する主な手順を説明します。

クライアントレスソリューションの動作に関する幅広い技術概要については、 [REST API の概要](/help/authentication/rest-api-overview.md). Adobeは、アーキテクチャ全体と最初の実装に関するサポートを受ける際に推奨される連絡先です。

## Amazon Clientless SSO {#AMZ-Clientless-SSO}

### アーキテクチャの概要 {#High-Level-Arch}

Amazonクライアントレス SSO の実装はシンプルで、主に通常のAdobe Primetime認証クライアントレス API と同じです。

Amazon SDK を使用してパーソナライズされたペイロードを取得し、Adobeクライアントレス API を呼び出す際に使用する必要があります。

ペイロードが認識され、認証済みセッションに対応している場合、クライアントレス API はセッションのトークンと共にすぐに返されます。

### Amazon SDK を使用するアプリケーションの構築方法 {#Build-entries}

* 最新のをダウンロードしてコピーする [Amazon Stub SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) を/SSOEnabler フォルダーにアプリディレクトリと並行して追加します。
* マニフェスト/gradle ファイルを更新して、ライブラリを使用します。

  **マニフェストファイルに次の行を追加します。**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Gradle ファイルのエントリ：**

  リポジトリ内：

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  dependencies の下に、以下を追加します。

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Amazonコンパニオンアプリがない場合の処理：

  コンパニオンがAmazonデバイス上に存在しない可能性は低いものの、アプリケーションが実行中の場合は、次のクラスで実行時に ClassNotFoundException が発生する必要があります。 `com.amazon.ottssotokenlib.SSOEnabler`.

  これが発生した場合、必要な操作は、ペイロードの手順をスキップし、通常の PrimeTime フローにフォールバックするだけです。 SSO は有効になりませんが、通常の認証フローは正常に実行されます。

</br>

### Amazon SDK を使用したAmazon SSO ペイロードの取得方法 {#UseAmazonSSO}

アプリケーションの初期化中に、SSOEnabler のインスタンスを取得します。 アプリケーションのアーキテクチャに基づいて、同期実装と非同期実装のどちらを使用するかを決定する必要があります。

何らかの理由で、API 呼び出しがペイロードを返さない場合は、通常の非 SSO フローを使用し、AmazonおよびAdobeパートナーに連絡して調査してください。

**非同期 API**

* SSO Enabler インスタンスを取得：

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* コールバックの設定

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * 成功応答バンドルには、次が含まれます。
      * キー「SSOToken」を持つ文字列としての SSO トークン
   * 失敗応答バンドルには次が含まれます。
      * キー「ErrorCode」を持つ整数としてのエラーコード
      * エラーの説明をキー「ErrorDescription」を持つ文字列として表示


* SSO トークンの取得

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* この API は、初期化時に設定されたコールバックを介して応答を提供します。

  **Ex**. init 中に作成された singleton インスタンスを使用してを呼び出します。

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**同期 API**

* SSO Enabler インスタンスを取得し、コールバックを設定します。

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* SSO トークンの取得

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * この API は呼び出し元のスレッドをブロックし、結果のバンドルで応答します。 これは同期呼び出しなので、必ずメインスレッドで使用しないでください。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * ミリ秒単位の値。 設定した場合、同期 API のデフォルトのタイムアウト値である 1 分を上書きします。


### 動的なクライアント登録を使用するAdobe Passクライアントレス API の更新 {#clientlessdcr}

これが初めての実装の場合は、 **クライアントレスの技術概要** サポートが必要な場合は、Adobeにお問い合わせください。

Adobeクライアントレス API では、アプリケーションサーバーを呼び出すために動的クライアント登録を使用するAdobeが必要です。

* アプリケーションで Dynamic Client 登録を使用するには、 [Dynamic Client 登録管理を使用してアプリケーションを登録](/help/authentication/dynamic-client-registration-management.md).

* Adobe Passサーバーに対して認証と承認のリクエストを実行するために Dynamic Client Registration API を実装するには、 [動的クライアント登録 API](/help/authentication/dynamic-client-registration-api.md) .

### Adobe Pass SSO を使用するためのAmazon Clientless API の更新 {#clientlesssso}

Amazon SDK から取得したAmazon SSO ペイロードは、 Adobe Pass Authentication エンドポイントに対するリクエストに存在する必要があります。

```
      /adobe-services/*
      /reggie/*
      /api/*
```


すべてのAdobe Pass認証エンドポイントは、デバイススコープ識別子またはプラットフォームスコープ識別子 (Amazon SSO ペイロードに存在 ) を受け取るために、次のメソッドをサポートしています。

* ヘッダーとして：&quot;Adobe — 件名 — トークン&quot;
* クエリパラメーターとして： &quot;ast&quot;
* POST パラメーターとして： &quot;ast&quot;


>[!NOTE]
>
>デバイス範囲の識別子またはプラットフォーム範囲の識別子がクエリ/POST パラメーターとして送信される場合は、要求の署名を生成する際にその識別子を含める必要があります。

>[!NOTE]
>
>クエリパラメーター「ast」を使用すると、URL 全体が非常に長くなり、拒否される場合があります。 /authenticate 呼び出し時に、このパラメーターは/regcode 呼び出しで指定されたので、スキップできます。

**例：**

**カスタムヘッダーとして送信**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**クエリパラメーターとして送信中**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**POST パラメーターとして送信中**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Amazon SSO が存在しないか無効な場合、Adobe Pass Authentication は属性を無視し、SSO が存在しない場合と同じように呼び出しが実行されます。
