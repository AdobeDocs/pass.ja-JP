---
title: Amazon FireOS SDKとDynamic Client Registration
description: Amazon FireOS SDKとDynamic Client Registration
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: c2a5591cd8fea44f66fc25beb1fb40532e18d8a6
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---


# （レガシー） Amazon FireOS SDKとDynamic Client Registration {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

## <span id=""></span>はじめに {#Intro}

FireOS AccessEnabler SDK for FireTVは、セッション Cookieを使用せずに認証を有効にするように変更されました。 ますます多くのブラウザがCookieへのアクセスを制限しているため、認証を許可するための別の方法が必要でした。

**FireOS SDK 3.0.4**&#x200B;は、署名済み依頼者IDとセッション Cookie認証に基づく現在のアプリ登録メカニズムを[動的クライアント登録の概要](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)に置き換えます。


## APIの変更 {#API}

### Factory.getInstance

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに1つのAccess Enabler インスタンスが必要です。

| API呼び出し：コンストラクター |
| --- |
| public static AccessEnabler getInstance （Context appContext, String softwareStatement, String redirectUrl） <br>がAccessEnablerExceptionをスローします |

**可用性：** v3.0以降

**パラメーター：**

- *appContext*: Android アプリケーションのコンテキスト
- *softwareStatement*: strings.xmlに「software\_statement」が設定されている場合にTVE ダッシュボードまたは&#x200B;*null*&#x200B;から取得した値
- *redirectUrl* :FireTV実装の場合、このパラメーターはnullにする必要があります。 この属性の設定は無視されます。

**メモ**

- 無効なsoftwareStatementにより、アプリケーションがAccessEnablerを初期化しないか、Adobe Pass認証および認証にアプリケーションを登録しません
- firetvのredirectUrl パラメーターは、一意のAccessEnabler インスタンスによって認証が処理されるため、SDKによってadobepass://android.appに設定されます。

### setRequestor

**説明：** チャネルのIDを確立します。 各チャネルには、Adobe Pass認証システム用のAdobeに登録する際に一意のIDが割り当てられます。 SSOおよびリモートトークンを扱う場合、アプリケーションがバックグラウンドにあるときに認証状態が変更される可能性があります。システム状態と同期するためにアプリケーションがフォアグラウンドに投入されたときにsetRequestorを再度呼び出すことができます（SSOが有効になっている場合はリモートトークンを取得し、その間にログアウトが発生した場合はローカルトークンを削除します）。

サーバー応答には、MVPDのリストと、チャネルのIDに添付されているいくつかの設定情報が含まれています。 サーバー応答は、Access Enabler コードによって内部で使用されます。 setRequestorComplete （） コールバックを使用して、操作のステータス（SUCCESS/FAILなど）のみがアプリケーションに表示されます。

*urls* パラメーターが使用されていない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダーURL （Adobe リリース実稼働環境）をターゲットにします。

*urls* パラメーターに値が指定されている場合、結果のネットワーク呼び出しは、*urls* パラメーターで指定されたすべてのURLをターゲットにします。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPDのリストをコンパイルする場合は、最初のレスポンダーが優先されます。 リスト内の各MVPDについて、Access Enablerは関連するサービスプロバイダーのURLを記憶します。 その後のすべての使用権限リクエストは、設定フェーズでターゲット MVPDとペア設定されたサービスプロバイダーに関連付けられたURLに送信されます。

| API呼び出し：依頼者設定 |
| --- |
| public void setRequestor （String requestorId） |

**可用性：** v3.0以降

| API呼び出し：依頼者設定 |
| --- |
| `public void setRequestor(String requestorId, ArrayList<String> urls)` |

**可用性：** v3.0以降

**パラメーター：**

- *requestorID*: チャネルに関連付けられている一意のID。 Adobeで割り当てられた一意のIDを、最初にAdobe Pass Authentication Serviceに登録するときにサイトに渡します。
- *urls*: オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で様々なインスタンスを使用する場合があります）。 これを使用して、複数のAdobe Pass認証サービスプロバイダーインスタンスを指定できます。 この場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。

非推奨：

- *signedRequestorID*：秘密鍵でデジタル署名された依頼者IDのコピー。 <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**コールバックがトリガーされました：** `setRequestorComplete()`

</br>

### ログアウト

**説明：**&#x200B;このメソッドを使用して、ログアウトフローを開始します。 ログアウトは、ユーザーがAdobe Pass Authentication サーバーとMVPDのサーバーの両方からログアウトする必要があるため、一連のHTTP リダイレクト操作の結果です。 その結果、このフローはChromeCustomTab ウィンドウを開いてログアウトを実行します。

| API呼び出し：ログアウトフローを開始します |
| --- |
| public void logout （） |

**可用性：** v3.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`

## プログラマ実装フロー {#Progr}

### **1. アプリケーション**&#x200B;を登録

1. Adobe Passからsoftware\_statementを取得します（TVE Dashboard）
1. これらの値をAdobe Pass SDKに渡すには、次の2つのオプションがあります。
   - strings.xmlに次を追加します。

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - AccessEnabler.getInstance （appContext,softwareStatement,null）を呼び出します。



### **2. アプリケーション**&#x200B;の設定

- a.  setRequestor （requestor\_id）

  SDKは、次の操作を実行します。

   - アプリケーションの登録：**software\_statement**&#x200B;を使用すると、SDKは&#x200B;**client\_id、client\_secret、client\_id\_issued\_at、redirect\_uris、grant\_types**&#x200B;を取得します。 この情報は、アプリケーションの内部ストレージに保存されます。
   - client\_id、client\_secret、grant\_type=&quot;client\_credentials&quot;を使用して&#x200B;**access\_token**&#x200B;を取得します。 このaccess\_tokenは、SDKからAdobe Pass サーバーへの呼び出しごとに使用されます。

| トークンエラー応答： |  |  |
|--- | --- | --- |
| HTTP 400 （不正なリクエスト） | {&quot;error&quot;: &quot;invalid\_request&quot;} | 要求に必要なパラメーターが含まれていない、サポートされていないパラメーター値（付与タイプ以外）が含まれている、パラメーターを繰り返す、複数の資格情報が含まれている、クライアントを認証するための複数のメカニズムを利用する、またはその他の方法で不正な形式である。 |
| HTTP 400 （不正なリクエスト） | {&quot;error&quot;: &quot;invalid\_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK *MUST*&#x200B;は、認証サーバーに再度登録する必要があります。 |
| HTTP 400 （不正なリクエスト） | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 認証されたクライアントは、この承認付与タイプを使用する権限がありません。 |

- MVPDでパッシブ認証が必要な場合、WebViewはそのMVPDでパッシブ認証を実行するために開き、完了すると閉じます

- b. checkAuthentication （）

   - *true*：認証に移動
   - *false* :MVPDを選択に移動

- c. getAuthentication :SDKでは、呼び出しパラメーターに&#x200B;**access_token**&#x200B;が含まれます

   - mvpd記憶：setSelectedProvider （mvpd\_id）に移動
   - mvpdが選択されていません：displayProviderDialog
   - mvpd selected :setSelectedProvider （mvpd\_id）に移動

- d. setSelectedProvider

   - mvpd\_id認証URLがChromeCustomTabsに読み込まれます
   - ログインに成功しました：delegate.setAuthenticationStatus （SUCCESS）
   - ログインがキャンセルされました：MVPDの選択範囲をリセット
   - 認証が完了したときに取得するURL スキームは、「adobepass://android.app」として確立されます

- e. get/checkAuthorization :SDKには、Authorization: Bearer **access\_token** 1}として**access\_tokenがヘッダーに含まれます**

- 認証に成功すると、メディアトークンを取得するための呼び出しが行われます

- f. ログアウト :

   - SDKは、現在の依頼者の有効なトークンを削除します（SSOを介して取得した認証ではなく、他のアプリケーションで取得した認証は有効のままです）
   - SDKはChrome カスタムタブを開いて、mvpd\_id ログアウトエンドポイントにアクセスします。 完了すると、Chrome カスタムタブは閉じられます
   - ログアウトが完了した瞬間をキャプチャするために、URL スキームは「adobepass://logout」として確立されます
   - logoutはsendTrackingData （new Event （EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR）とコールバック :setAuthenticationStatus （0,&quot;Logout&quot;）をトリガーします。



**注：**&#x200B;各呼び出しには&#x200B;**access_token**&#x200B;が必要なため、以下のエラーコードはSDKで処理されます。

| エラー応答 |  |  |
|--- | --- | --- |
| invalid_request | 400 | リクエストの形式が正しくありません。 SDKは、サーバーへの呼び出しの実行を停止する必要があります。 |
| invalid_client | 403 | クライアント IDは、リクエストの実行を許可されなくなりました。 Sdkは、クライアント登録を再度実行する必要があります。 |
| access_denied | 401 | access_tokenが無効です。 sdkは新しいaccess_tokenをリクエストする必要があります。 |
