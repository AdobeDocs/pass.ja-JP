---
title: JavaScript SDK クックブック
description: JavaScript SDK クックブック
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# （従来の）JavaScript SDK クックブック {#javascript-sdk-cookbook}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#intro}

このドキュメントでは、Adobe Pass Authentication Service とJavaScriptを統合するためにプログラマーの上位レベルのアプリケーションで実装される使用権限ワークフローについて説明します。 JavaScript API リファレンスへのリンクは、全体に含まれています。

また、「関連情報 [&#x200B; の節には次の内容も含まれます &#x200B;](#related)
JavaScript コードサンプルのセットへのリンク。

## 使用権限フロー {#entitlement}

1. [前提条件](#prereq)
2. [起動フロー](#startup)
3. [認証フロー](#authn)
4. [認証フロー](#authz)
5. [メディアフローを表示](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## 前提条件 {#prereq}

**依存関係：**

- Adobe Pass Authentication Library （AccessEnabler）をインストールする場合は、Adobe Pass Authentication 担当営業または販売店に依頼してください。
- 有効なAdobe Pass認証リクエスター ID がある場合は、Adobe Pass認証アカウントマネージャーと協力して申請してください。

コールバック関数を作成します。

- `entitlementLoaded`
</br>

**トリガー:** AccessEnabler が読み込まれ、初期化が完了しました。

- `displayProviderDialog(mvpds)`

  **トリガー:** ユーザーがプロバイダー（MVPD）を選択しておらず、まだ認証されていない場合にのみ `getAuthentication(),` されます
mvpds パラメーターは、ユーザーが使用できるプロバイダーの配列です。

- `setAuthenticationStatus(status, errorcode)`

  **トリガー:**
   - `checkAuthentication()` 毎回。
   - ユーザーが既に認証済みで、プロバイダーを選択している場合にのみ `getAuthentication()` 用します。

  返されるステータスは成功または失敗です。エラーコードは失敗のタイプを示します。

- `createIFrame(width, height)`

  **トリガー:** `setSelectedProvider(providerID)`。選択したプロバイダーが IFrame で表示するように構成されている場合のみ。

  >[!NOTE]
  >
  >プロバイダーは、認証画面をリダイレクトまたは iFrame のいずれかでレンダリングするように設定されており、プログラマーはその両方を考慮する必要があります。

- `sendTrackingData(event, data)`

  **トリガー:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`。  `event` パラメーターは、どの使用権限イベントが発生したかを示します。`data` パラメーターは、イベントに関連する値のリストです。
- `setToken(token, resource)`
  リソースの表示が正常に承認された後の **トリガー:** `checkAuthorization()` および `getAuthorization()`。   `token` パラメーターは短時間のみ有効なメディアトークンです。`resource` パラメーターは、ユーザーが表示を許可されているコンテンツです。

- `tokenRequestFailed(resource, code, description)`
  認証に失敗した後の **トリガー:**`checkAuthorization()` および `getAuthorization()`。\
  `resource` パラメーターは、ユーザーが表示しようとしたコンテンツです。`code` パラメーターは、エラーのタイプを示すエラーコードです。`description` パラメーターは、エラーコードに関連付けられたエラーの説明です。

- `selectedProvider(mvpd)`

  **トリガー:** [`getSelectedProvider()`] （#$getSelProv `mvpd` パラメーターは、次のユーザーが選択したプロバイダーに関する情報を提供します
ユーザー。

- `setMetadataStatus(metadata, key, arguments)`

  **トリガー:** `getMetadata().`\
  `metadata` パラメーターは、要求された特定のデータを提供します。key パラメーターは、`getMetadata()`request で使用されるキーで、`arguments` パラメーターは、`getMetadata()` に渡されたディクショナリと同じです。


## 2.起動フロー

**I. AccessEnabler JavaScriptをロードします。**

**ステージングプロファイル用**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

または…

**実稼動プロファイルの場合**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**トリガー:** 初期化が完了すると、Adobe Pass
認証は `entitlementLoaded()` コールバック関数を呼び出します。 これは、アプリケーションが AccessEnabler と通信するためのエントリー・ポイントです。


**二** を呼び出して `setRequestor()` を確立します
プログラマーの ID。プログラマーの `requestorID` を渡し、
（オプション）Adobe Pass認証エンドポイントの配列。

**トリガー:** なし。ただし、必要に応じて `displayProviderDialog()` を呼び出すことができます。


**三** 完全な `checkAuthentication()` 認証フロー [ を開始せずに既存の認証を確認するには、] を呼び出します。  この呼び出しが成功した場合は、`authorization flow` に直接進むことができます。  そうでない場合は、`authentication flow` に進みます。

**依存関係：** `setRequestor()` の呼び出しが成功しました（この依存関係は、後続のすべての呼び出しにも適用されます）。

**トリガー:** `setAuthenticationStatus()` コールバック

</br>

## 3.認証フロー </span>


**依存関係：** `setRequestor()` の呼び出しが成功しました（この依存関係は、後続のすべての呼び出しにも適用されます）。


`getAuthentication()` を呼び出して認証ステータスを取得するか、またはプロバイダー認証フローをトリガーします。

**トライガー：**

- `displayProviderDialog()` ユーザーがまだ認証されていない場合
- 認証が既に実行されたかどうかを `setAuthenticationStatus()` します

認証フローの完了は、AccessEnabler が `setAuthenticationStatus()` を使用して `isAuthenticated == 1` を呼び出したときに到達します。

## 4.認証フロー {#authz}

**依存関係：**

- `setRequestor()` の呼び出しが成功した場合（この依存関係は、以降のすべての呼び出しにも適用されます）。
- 有効な ResourceID がMVPDと合意されました。 ResourceID は、他のデバイスやプラットフォームで使用されるものと同じにする必要があり、MVPD 間でも同じであることに注意してください。

`getAuthorization()` を呼び出し、リクエストされたメディアの ResourceID を渡します。 呼び出しが成功すると、短いメディアトークンが返され、ユーザーが要求されたメディアの表示を許可されていることを確認します。

- 呼び出しが成功した場合：ユーザーに有効な AuthN トークンがあり、ユーザーが要求されたメディアを監視する権限を持っている。
- 呼び出しが失敗した場合：スローされた例外を調べて、そのタイプ（AuthN、AuthZ など）を特定します。
- 呼び出しが AuthN エラーの場合は、AuthN フローを再起動します。
- 呼び出しが AuthZ エラーの場合、ユーザーは要求されたメディアを監視する権限がないため、何らかのエラーメッセージがユーザーに表示されます。
- その他のエラー（接続エラー、ネットワークエラーなど）が発生した場合は、適切なエラーメッセージをユーザーに表示します。

メディアトークン検証子を使用して、成功した `getAuthorization()` 呼び出しから返された shortMediaToken を検証します。


**依存関係：** ショートメディアトークン検証子（と共に含まれる）
AccessEnabler ライブラリ）

- 検証に合格した場合：ユーザーに要求されたメディアを表示/再生します。
- 失敗した場合：AuthZ トークンが無効で、メディアリクエストが拒否され、エラーメッセージがユーザーに表示されます。

## &#x200B;5. メディアフローの表示 {#logout}

- ユーザーが表示するメディアを選択します。
   - メディアは保護されていますか？
      - アプリは、メディアが保護されているかどうかを確認します。
         - メディアが保護されている場合、アプリは上記の認証（AuthZ）フローを開始します。
         - メディアが保護されていない場合は、メディアの表示フローに進みます。
         - 再生メディア

## 訪問者 ID の設定 {#visitorID}

[Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) の値の設定は、分析の観点から非常に重要です。 EC visitorID の値が設定されると、SDKはネットワーク呼び出しごとにこの情報を送信し、Adobe Pass Authentication サービスはこの情報を収集します。 これにより、Adobe Pass Authentication Service からの分析データを、他のアプリケーションや web サイトからの他の分析レポートと関連付けることができます。 EC 訪問者 ID の設定方法については、[&#x200B; こちら &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) を参照してください。


>[!NOTE]
>
>この機能のサポートは、JS SDK バージョン 3.1.0 以降で利用可能です。

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
