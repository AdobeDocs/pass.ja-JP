---
title: クライアントレス API クックブックを使用したAmazon FireOS SSO
description: クライアントレス API クックブックを使用したAmazon FireOS SSO
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 0%

---

# クライアントレス API クックブックを使用したAmazon FireOS SSO {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

## 概要 {#Introduction}

このドキュメントでは、クライアントレス API を使用してAmazonの SSO バージョンのAdobe Pass認証フローを実装する手順を説明します。 このドキュメントの最初の部分では、すでにAmazonの実装に精通し、経験を積んでいる多くのパートナー向けに、アーキテクチャの Aem Architecture バージョンの特異性について説明します。

このドキュメントの第 2 部では、Adobe Pass認証クライアントレス API を実装するための主な手順について説明します。

クライアントレスソリューションの仕組みに関する技術的な概要については、[REST API の概要 ](/help/authentication/rest-api-overview.md) を参照してください。 アーキテクチャ全体と最初の実装に関するサポートについては、Adobeにお問い合わせください。

## Amazon クライアントレス SSO {#AMZ-Clientless-SSO}

### アーキテクチャの概要 {#High-Level-Arch}

Amazon クライアントレス SSO 実装はシンプルで、通常のAdobe Primetime認証クライアントレス API とほとんど同じです。

パーソナライズされたペイロードを取得し、Adobeのクライアントレス API を呼び出す際に使用するには、Amazon SDK を使用する必要があります。

ペイロードが認識され、認証済みセッションに対応している場合、クライアントレス API はすぐにセッションのトークンを返します。

### Amazon SDK を使用するアプリケーションの作成方法 {#Build-entries}

* 最新の [Amazon スタブ SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) をダウンロードして、アプリ ディレクトリと並行して/SSOEnabler フォルダーにコピーします
* manifest/gradle ファイルを更新してライブラリを使用します。

  **マニフェストファイルに次の行を追加します。**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Gradle ファイルエントリ：**

  リポジトリで：

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  「dependencies」で、次のコマンドを追加します。

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Amazon コンパニオンアプリがない場合の処理：

  該当する可能性は低いですが、アプリケーションを実行しているAmazon デバイスにコンパニオンが存在しない場合、次のクラス `com.amazon.ottssotokenlib.SSOEnabler` で、実行時に ClassNotFoundException が発生する可能性があります。

  この場合は、ペイロードステップをスキップして通常の PrimeTime フローにフォールバックするだけで済みます。 SSO は有効になりませんが、通常の認証フローは正常に行われます。

</br>

### Amazon SDK を使用してAmazon SSO ペイロードを取得する方法 {#UseAmazonSSO}

アプリケーションの初期化中に、SSOEnabler のインスタンスを取得します。 アプリケーションのアーキテクチャに応じて、同期実装と非同期実装のどちらを選択するかを決定する必要があります。

何らかの理由で API 呼び出しがペイロードを返さない場合は、通常の非 SSO フローを使用し、AmazonおよびAdobeパートナーに問い合わせて調査してください。

**非同期 API**

* SSO イネーブラ インスタンスの取得：

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

   * 成功応答バンドルには、次の内容が含まれます。
      * キー「SSOoken」を含む文字列としての SSO トークン
   * 失敗の応答バンドルには、次の内容が含まれます。
      * キー「ErrorCode」を持つ整数としてのエラーコード
      * 「ErrorDescription」キーを持つ文字列としてのエラーの説明


* SSO トークンの取得

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* この API は、初期化時に設定されたコールバックを介して応答を提供します。

  **例**。 初期化中に作成されたシングルトンインスタンスを使用した呼び出し：

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**同期 API**

* SSO イネーブラ・インスタンスの取得とコールバックの設定

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* SSO トークンの取得

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * この API は呼び出し元スレッドをブロックし、結果のバンドルで応答します。 これは同期呼び出しなので、メインスレッドでは使用しないでください。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * ミリ秒単位の値。 設定した場合、同期 API のデフォルトのタイムアウト値 1 分を上書きします。


### Dynamic Client Registration を使用するためのAdobe Pass クライアントレス API アップデート {#clientlessdcr}

初めて実装する場合は、**クライアントレス技術概要** を参照し、サポートが必要な場合はAdobeにお問い合わせください。

Adobeクライアントレス API では、Adobeサーバーを呼び出すために、アプリケーションで Dynamic Client Registration を使用する必要があります。

* アプリケーションで動的クライアント登録を使用するには、[ 動的クライアント登録管理 ](/help/authentication/dynamic-client-registration-management.md) の手順に従ってアプリケーションを登録します。

* Dynamic Client Registration API を実装してAdobe Pass サーバーに対する認証および承認リクエストを実行するには、[Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md) の手順に従います。

### Amazon SSO を使用するためのAdobe Pass クライアントレス API アップデート {#clientlesssso}

Amazon SDK から取得されたAmazon SSO ペイロードは、Adobe Pass認証エンドポイントへのリクエストに存在する必要があります。

```
      /adobe-services/*
      /reggie/*
      /api/*
```


すべてのAdobe Pass認証エンドポイントは、デバイススコープの ID またはプラットフォームスコープの ID （Amazon SSO ペイロードに存在）を受け取るために次のメソッドをサポートしています。

* As a ヘッダー：「Adobe – 件名 – トークン」
* クエリパラメーターとして：「ast」
* Post パラメーターとして：&quot;ast&quot;


>[!NOTE]
>
>デバイススコープの識別子または Platform スコープの識別子が query/post パラメーターとして送信される場合、リクエスト署名の生成時にそれが含まれる必要があります。

>[!NOTE]
>
>クエリパラメーター「ast」を使用すると、url 全体が非常に長くなり、拒否される場合があります。 /authenticate 呼び出し時に、/regcode 呼び出しで指定されたので、このパラメーターをスキップできます

**例：**

**カスタムヘッダーとして送信**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**クエリパラメーターとして送信**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**POST パラメーターとして送信**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Amazon SSO が存在しない場合または無効な場合、Adobe Pass認証は属性を無視し、呼び出しは SSO が存在しないかのように実行されます。
