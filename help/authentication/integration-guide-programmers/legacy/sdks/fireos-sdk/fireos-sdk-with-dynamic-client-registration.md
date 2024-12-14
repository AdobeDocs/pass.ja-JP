---
title: Dynamic Client Registration を使用したAmazon FireOS SDK
description: Dynamic Client Registration を使用したAmazon FireOS SDK
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---


# （従来の） Dynamic Client Registration を使用したAmazon FireOS SDK {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

## <span id=""></span> はじめに {#Intro}

FireOS AccessEnabler SDK for FireTV は、セッション Cookie を使用せずに Authentication を有効にするように変更されました。 Cookie へのアクセスを制限するブラウザーが増えているので、認証を許可するには別の方法が必要でした。

**FireOS SDK 3.0.4** は、署名済みリクエスター ID とセッション cookie 認証に基づく現在のアプリ登録メカニズムを [Dynamic Client Registration Overview](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) に置き換えます。


## API の変更点 {#API}

### Factory.getInstance

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーション・インスタンスごとに 1 つの Access Enabler インスタンスが必要です。

| API 呼び出し：コンストラクター |
| --- |
| public static AccessEnabler getInstance （Context appContext, String softwareStatement, String redirectUrl） <br>        AccessEnablerException をスロー |

**提供：** v3.0 以降

**パラメーター：**

- *appContext*:Android アプリケーションコンテキスト
- *softwareStatement*: TVE ダッシュボードから取得された値。または string.xml で「software\_statement」が設定されている場合は *null*
- *redirectUrl* :FireTV 実装の場合、このパラメーターは null にする必要があります。 この属性に関する設定は無視されます。

**備考**

- 無効な softwareStatement は、アプリケーションが AccessEnabler を初期化しないか、アプリケーションをAdobe Passの認証および認証用に登録します
- 認証は一意の AccessEnabler インスタンスによって処理されるため、FireTV の redirectUrl パラメータはSDKによってadobepass://android.appに設定されます。

### setRequestor

**説明：** チャネルの ID を確立します。 各チャネルには、Adobe Pass Authentication System のAdobeに登録すると、一意の ID が割り当てられます。 SSO およびリモート・トークンを処理する場合、アプリケーションがバックグラウンドにある場合に認証状態が変更される可能性があります。アプリケーションがフォアグラウンドになると、システム状態と同期するために setRequestor を再度呼び出すことができます（SSO が有効な場合はリモート・トークンを取得し、その間にログアウトが発生した場合はローカル・トークンを削除します）。

サーバー応答には、MVPD のリストと、チャネルの ID に添付されたいくつかの設定情報が含まれています。 サーバ応答は、アクセス イネーブラ コードによって内部的に使用されます。 setRequestorComplete （） コールバックを使用すると、操作のステータス（SUCCESS/FAIL）のみがアプリケーションに表示されます。

*urls* パラメーターを使用しない場合、生成されるネットワーク呼び出しは、デフォルトのサービスプロバイダー URL （Adobeリリースの実稼動環境）をターゲットにします。

*urls* パラメーターに値を指定すると、結果として得られるネットワーク呼び出しは、*urls* パラメーターで指定されるすべての URL をターゲットにします。 すべての設定要求が、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする場合は、最初のレスポンダーが優先されます。 Access Enabler は、リスト内の各MVPDについて、関連するサービス プロバイダの URL を記憶します。 以降のすべての使用権限リクエストは、設定段階でターゲット MVPDとペアになっていた、サービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスター設定 |
| --- |
| public void setRequestor （String requestorId） |

**提供：** v3.0 以降

| API 呼び出し：リクエスター設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**提供：** v3.0 以降

**パラメーター：**

- *requestorID*：チャネルに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録する際に、Adobeによって割り当てられた一意の ID をサイトに渡します。
- *urls*：オプションのパラメーターです。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと承認サービスのエンドポイントを指定できます（デバッグ目的で別のインスタンスを使用することもできます）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その場合、MVPDのリストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー（最初に応答し、そのMVPDをサポートするプロバイダー）に関連付けられます。

非推奨（廃止予定）:

- *signedRequestorID*：秘密鍵でデジタル署名されたリクエスター ID のコピー。<!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->。

**コールバックがトリガーされました：** `setRequestorComplete()`

</br>

### ログアウト

**説明：** このメソッドを使用して、ログアウトフローを開始します。 ログアウトは、ユーザーがAdobe PassMVPDサーバーと認証サーバーの両方からログアウトする必要があるために、一連の HTTP リダイレクト操作の結果です。 その結果、このフローでは、ログアウトを実行するための ChromeCustomTab ウィンドウが開きます。

| API 呼び出し：ログアウトフローの開始 |
| --- |
| public void logout （） |

**提供：** v3.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`

## プログラマ実装フロー {#Progr}

### **1.アプリケーションの登録**

1. Adobe Passから software\_statement を取得する（TVE Dashboard）
1. これらの値をAdobe Pass SDKに渡す方法は 2 つあります。
   - strings.xml に次を追加します。

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - AccessEnabler.getInstance （appContext,softwareStatement, null）を呼び出す



### **2。 アプリケーションの設定**

- a. setRequestor （requestor\_id）

  SDKは、次の操作を実行します。

   - アプリケーションを登録：**software\_statement** を使用して、SDKは **client\_id、client\_secret、client\_id\_issued\_at、redirect\_uris、grant\_types** を取得します。 この情報は、アプリケーションの内部ストレージに保存されます。
   - client\_id、client\_secret および grant\_type=&quot;client\_credentials **を使用して、** access\_token&quot;を取得します。 この access\_token は、SDKがAdobe Pass サーバーに対して行う各呼び出しで使用されます。

| トークンエラー応答： |  |  |
|--- | --- | --- |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_request&quot;} | 要求に必須パラメーターがないか、サポートされていないパラメーター値（許可タイプ以外）が含まれているか、パラメーターを繰り返しているか、複数の資格情報が含まれているか、クライアントの認証に複数のメカニズムが使用されているか、その他の形式が正しくありません。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK *必ず* 認証サーバーに再登録します。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 認証済みクライアントには、この認証付与タイプの使用が許可されていません。 |

- MVPDでパッシブ認証が必要な場合、WebView は開いてMVPDでパッシブに実行され、完了すると閉じられます

- b. checkAuthentication （）

   - *true*：認証に移動します
   - *false* :「MVPDを選択」に移動します

- c. getAuthentication :SDKの呼び出しパラメーターに **access_token** が含まれます

   - mvpd remembered : setSelectedProvider （mvpd\_id）に移動します
   - mvpd が選択されていません：displayProviderDialog
   - mvpd selected :setSelectedProvider （mvpd\_id）に移動します

- d. setSelectedProvider

   - mvpd\_id 認証 URL は、ChromeCustomTabs に読み込まれる
   - ログイン成功：delegate.setAuthenticationStatus （成功）
   - ログインがキャンセルされました：MVPDの選択をリセット
   - 認証が完了したときにキャプチャするための URL スキームが「adobepass://android.app」として確立されています

- e. get/checkAuthorization :SDKは、Authorization: Bearer **access\_token **として、ヘッダーに **access\_token** を含めます。

- 認証に成功すると、メディアトークンを取得するための呼び出しが行われます

- f. ログアウト :

   - SDKによって、現在の要求者の有効なトークンが削除されます（SSO を介さずに他のアプリケーションによって取得された認証は有効なままです）
   - SDKは、mvpd\_id ログアウトエンドポイントに到達するために、Chromeのカスタムタブを開きます。 完了すると、Chromeのカスタムタブは閉じられます
   - ログアウトが完了した瞬間をキャプチャするために、URL スキームは「adobepass://logout」として確立されます
   - ログアウトは、sendTrackingData （new Event （EVENT\_LOGOUT,USER\_NOT\_AUTHENTICATED\_ERROR）と callback : setAuthenticationStatus （0,&quot;Logout&quot;）をトリガーにします。



**注意：** 各呼び出しには **access_token** が必要なため、以下のエラーコードの可能性がSDKで処理されます。

| エラーの応答 |  |  |
|--- | --- | --- |
| invalid_request | 400 | リクエストの形式が正しくありません。 SDKは、サーバーへの呼び出しを実行しなくなるはずです。 |
| invalid_client | 403 | このクライアント ID は、要求の実行が許可されなくなりました。 SDK は、クライアント登録を再度実行する必要があります。 |
| access_denied | 401 | access_token が無効です。 sdk は、新しい access_token をリクエストする必要があります。 |
