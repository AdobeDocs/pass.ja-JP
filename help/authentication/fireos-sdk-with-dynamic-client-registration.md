---
title: 動的クライアント登録を使用したAmazon FireOS SDK
description: 動的クライアント登録を使用したAmazon FireOS SDK
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# 動的クライアント登録を使用したAmazon FireOS SDK {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

## <span id=""></span>はじめに {#Intro}

FireTV 用 FireOS AccessEnabler SDK が、セッション cookie を使用せずに認証を有効にするように変更されました。 クッキーへのアクセスを制限するブラウザーが増えているので、認証を許可する別の方法が必要になりました。

**FireOS SDK 3.0.4** は、署名されたリクエスト元 ID とセッション cookie の認証に基づいて、現在のアプリ登録メカニズムをに置き換えます。 [動的クライアントの登録](/help/authentication/dynamic-client-registration.md).


## API の変更点 {#API}

### Factory.getInstance

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに 1 つの Access Enabler インスタンスが必要です。

| API 呼び出し：コンストラクター |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        AccessEnablerException をスロー |

**可用性：** v3.0 以降

**パラメーター：**

- *appContext*:Android アプリケーションコンテキスト
- *softwareStatement*: TVE ダッシュボードから取得した値、または *null* &quot;software\_statement&quot;が strings.xml に設定されている場合
- *redirectUrl* :FireTV 実装の場合、このパラメーターは null にする必要があります。 この属性の設定は無視されます。

**メモ**

- 無効な softwareStatement により、アプリケーションが AccessEnabler を初期化しないか、またはAdobe Pass認証と承認のためのアプリケーションを登録します
- FireTV の redirectUrl パラメーターは、認証が一意の AccessEnabler インスタンスによって処理されるので、SDK がadobepass://android.appに設定されます。

### setRequestor

**説明：** チャネルの ID を確立します。 各チャネルには、Adobe Pass認証システムのAdobeへの登録時に、一意の ID が割り当てられます。 SSO とリモートトークンを扱う場合、アプリケーションがバックグラウンドになっているときに認証状態が変わる可能性があります。システム状態と同期するために、アプリケーションがフォアグラウンドに移行したときに setRequestor を再び呼び出すことができます。

サーバ応答には、MVPD のリストと、チャネルの ID に添付される設定情報が含まれます。 サーバの応答は、Access Enabler コードによって内部的に使用されます。 操作のステータス (SUCCESS/FAIL) のみが、setRequestorComplete() コールバックを介してアプリケーションに表示されます。

次の場合、 *url* パラメーターを使用しない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダー URL(Adobeリリースの実稼動環境 ) をターゲットにします。

値が *url* パラメーターを渡すと、結果のネットワーク呼び出しでは、 *url* パラメーター。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする際には、最初の応答が優先されます。 リスト内の各 MVPD に対して、Access Enabler は関連するサービスプロバイダの URL を記憶します。 以降のすべてのエンタイトルメントリクエストは、設定フェーズでターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスト元の設定 |
| --- |
| public void setRequestor(String requestorId) |

**可用性：** v3.0 以降

| API 呼び出し：リクエスト元の設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**可用性：** v3.0 以降

**パラメーター：**

- *requestorID*：チャネルに関連付けられた一意の ID。 最初にAdobe Pass Authentication サービスを登録する際に、Adobeによって割り当てられた一意の ID をサイトに渡します。
- *url*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます (http://sp.auth.adobe.com/)。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で異なるインスタンスが使用される場合があります）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その際、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。

廃止：

- *signedRequestorID*：秘密鍵でデジタル署名された要求者 ID のコピー。 <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**コールバックがトリガーされました：** `setRequestorComplete()`

</br>

### ログアウト

**説明：** ログアウトフローを開始するには、このメソッドを使用します。 ログアウトは、一連の HTTP リダイレクト操作の結果です。これは、ユーザーがAdobe Pass認証サーバーと MVPD サーバーの両方からログアウトする必要があるためです。 その結果、このフローは ChromeCustomTab ウィンドウを開いてログアウトを実行します。

| API 呼び出し：ログアウトフローの開始 |
| --- |
| public void logout() |

**可用性：** v3.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`

## プログラマー実装フロー {#Progr}

### **1. アプリを登録**

1. Adobe Pass( TVE Dashboard ) からソフトウェア\_statement を取得する
1. これらの値をAdobe Pass SDK に渡す方法は 2 つあります。
   - strings.xml で、以下を追加します。

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - AccessEnabler.getInstance(appContext,softwareStatement, null) を呼び出します



### **2. アプリケーションを設定**

- a. setRequestor(requestor\_id)

  SDK は次の操作を実行します。

   - 登録アプリケーション：使用 **software\_statement**&#x200B;に設定されていない場合、SDK は **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. この情報は、アプリケーションの内部ストレージに保存されます。
   - 取得する **access\_token** client\_id、client\_secret および grant\_type=&quot;client\_credentials&quot;を使用します。 このアクセス\_token は、SDK がAdobe Passサーバーに対しておこなう呼び出しごとに使用されます。

| トークンエラー応答： |  |  |
|--- | --- | --- |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_request&quot;} | リクエストに必須のパラメーターがない、サポートされていないパラメーター値（付与タイプ以外）が含まれている、パラメーターを繰り返す、複数の資格情報を含む、クライアントを認証する複数のメカニズムを利用する、またはその他の形式が正しくありません。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK *MUST* を認証サーバーに再度登録します。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 認証済みのクライアントは、この認証付与タイプの使用を許可されていません。 |

- MVPD がパッシブ認証を必要とする場合、WebView は、その MVPD でパッシブを実行するために開き、完了時に閉じます

- b. checkAuthentication()

   - *true* ：認証に移動します。
   - *false* :MVPD を選択に移動します。

- c. getAuthentication :SDK には次が含まれます。 **access_token** 呼び出しパラメーター内

   - mvpd mored : setSelectedProvider(mvpd\_id) に移動
   - mvpd が選択されていません： displayProviderDialog
   - mvpd selected : setSelectedProvider(mvpd\_id) に移動します。

- d. setSelectedProvider

   - mvpd\_id 認証 URL が ChromeCustomTabs に読み込まれています
   - ログイン成功： delegate.setAuthenticationStatus ( SUCCESS )
   - ログインがキャンセルされました： MVPD の選択をリセット
   - 認証が完了した時点を取り込むために、URL スキームは「adobepass://android.app」として確立されます。

- e. get/checkAuthorization :SDK はヘッダーに**access\_token **を Authorization: Bearer として含めます。 **access\_token**

- 認証が成功した場合は、メディアトークンを取得するための呼び出しがおこなわれます。

- f.ログアウト：

   - SDK は現在の要求元に対して有効なトークンを削除します（SSO を介さずに他のアプリケーションで取得された認証は有効なままです）
   - SDK は Chrome カスタムタブを開き、mvpd\_id ログアウトエンドポイントに到達します。 完了すると、Chrome カスタムタブは閉じられます
   - ログアウトが完了した瞬間を取り込むために、URL スキームは「adobepass://logout」として確立されます。
   - logout は sendTrackingData(new Event(EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR) および callback : setAuthenticationStatus(0,&quot;Logout&quot;) をトリガーします。



**注意：** 各呼び出しでは **access_token**&#x200B;の場合、以下の考えられるエラーコードは SDK で処理されます。

| エラー応答 |  |  |
|--- | --- | --- |
| invalid_request | 400 | リクエストの形式が正しくありません。 SDK は、サーバーへの呼び出しの実行を停止する必要があります。 |
| invalid_client | 403 | クライアント ID は、要求を実行できなくなりました。 SDK は、クライアント登録を再度実行する必要があります。 |
| access_denied | 401 | access_token が無効です。 SDK は新しい access_token をリクエストする必要があります。 |
