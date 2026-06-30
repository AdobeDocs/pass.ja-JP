---
title: Android SDKとDynamic Client Registration
description: Android SDKとDynamic Client Registration
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: c2a5591cd8fea44f66fc25beb1fb40532e18d8a6
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 1%

---

# （レガシー）動的クライアント登録を使用したAndroid SDK {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#Intro}

Android AccessEnabler SDK Android版は、セッション Cookieを使用せずに認証を有効にするように変更されました。 ますます多くのブラウザがCookieへのアクセスを制限しているため、認証を許可するために別の方法を使用する必要があります。

Androidの場合、Chromeのカスタムタブを使用すると、他のアプリケーションからのCookieへのアクセスが制限されます。

>**Android SDK 3.0.0**&#x200B;の紹介：

- 動的クライアント登録は、署名済みの依頼者IDとセッション Cookie認証に基づいて、現在のアプリ登録メカニズムを置き換えます
- 認証フロー用のChrome カスタムタブ

>[!NOTE]
>
>Chrome Custom Tabs サポートのない古いAndroid バージョンの場合、以前のAccessEnabler SDK バージョンと同様にWebView認証が使用されます。


## 動的なクライアント登録 {#DCR}

Android SDK v3.0以降では、[Dynamic Client Registration Overview](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)で定義されているDynamic Client Registration プロシージャを使用します。


## 機能デモ {#Demo}

機能の詳細を説明し、TVE ダッシュボードを使用してソフトウェアステートメントを管理する方法と、Android SDKの一部としてAdobeが提供するデモアプリケーションを使用して生成されたステートメントをテストする方法に関するデモを含む[このウェビナー](https://my.adobeconnect.com/pzkp8ujrigg1/)をご覧ください。

## APIの変更 {#API}


### Factory.getInstance

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに1つのAccess Enabler インスタンスが必要です。

| API呼び出し：コンストラクター |
| --- |
| public static AccessEnabler getInstance （Context appContext, String softwareStatement, String redirectUrl） <br>がAccessEnablerExceptionをスローします |


**可用性：** v3.0以降

**パラメーター：**

- *appContext*: Android アプリケーションのコンテキスト
- softwareStatement: strings.xmlに「software\_statement」が設定されている場合にTVE ダッシュボードまたは&#x200B;*null*&#x200B;から取得した値
- redirectUrl：一意のURL。TVE ダッシュボードで明示的に追加された逆順のドメインの1つ。strings.xmlに「redirect\_uri」が設定されている場合は&#x200B;*null*

注意：無効なsoftwareStatementまたはredirectUrlが原因で、アプリケーションはAccessEnablerを初期化したり、Adobe Pass認証および認証にアプリケーションを登録したりしません</br>
注意：strings.xmlのredirectUrl パラメーターまたはredirect\_uriは、アプリケーション用TVE ダッシュボードに逆順で追加されたドメインの値である必要があります（例：TVE ダッシュボードに追加されたドメイン「adobe.com」の場合、redirectUrlは「com.adobe」である必要があります）。


### setRequestor

**説明：** チャネルのIDを確立します。 各チャネルには、Adobe Pass認証システム用のAdobeに登録する際に一意のIDが割り当てられます。 SSOおよびリモートトークンを扱う場合、アプリケーションがバックグラウンドにあるときに認証状態が変更される可能性があります。システム状態と同期するためにアプリケーションがフォアグラウンドに投入されたときにsetRequestorを再度呼び出すことができます（SSOが有効になっている場合はリモートトークンを取得し、その間にログアウトが発生した場合はローカルトークンを削除します）。

サーバー応答には、MVPDのリストと、チャネルのIDに添付されているいくつかの設定情報が含まれています。 サーバー応答は、Access Enabler コードによって内部で使用されます。 setRequestorComplete （） コールバックを使用して、操作のステータス（SUCCESS/FAILなど）のみがアプリケーションに表示されます。

*urls* パラメーターが使用されていない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダーURL （Adobe リリース/実稼動環境）をターゲットにします。

*urls* パラメーターに値が指定されている場合、結果のネットワーク呼び出しは、*urls* パラメーターで指定されたすべてのURLをターゲットにします。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPDのリストをコンパイルする場合は、最初のレスポンダーが優先されます。 リスト内の各MVPDについて、Access Enablerは関連するサービスプロバイダーのURLを記憶します。 その後のすべての使用権限リクエストは、設定フェーズでターゲット MVPDとペア設定されたサービスプロバイダーに関連付けられたURLに送信されます。

| API呼び出し：依頼者設定 |
| --- |
| `public void setRequestor(String requestorId)` |

**可用性：** v3.0以降

| API呼び出し：依頼者設定 |
| --- |
| `public void setRequestor(String requestorId, ArrayList<String> urls)` |

**可用性：** v3.0以降

**パラメーター：**

- *requestorID*: チャネルに関連付けられている一意のID。 Adobeで割り当てられた一意のIDを、Adobe Pass Authentication Serviceに初めて登録したときにサイトに渡します。
- *urls*: オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーは[http://sp.auth.adobe.com/](http://sp.auth.adobe.com/)で使用されます。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で様々なインスタンスを使用する場合があります）。 これを使用して、複数のAdobe Pass認証サービスプロバイダーインスタンスを指定できます。 この場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。

非推奨：

- *signedRequestorID*：秘密鍵でデジタル署名された依頼者IDのコピー。 <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**コールバックがトリガーされました：** `setRequestorComplete()`

### ログアウト

**説明：**&#x200B;このメソッドを使用して、ログアウトフローを開始します。 ログアウトは、ユーザーがAdobe Pass Authentication サーバーとMVPDのサーバーの両方からログアウトする必要があるため、一連のHTTP リダイレクト操作の結果です。 その結果、このフローはChromeCustomTab ウィンドウを開いてログアウトを実行します。

| API呼び出し：ログアウトフローを開始します |
| --- |
| public void logout （） |

**可用性：** v3.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** 


## プログラマ実装フロー {#Progr}

### **1. アプリケーション**&#x200B;を登録

a. Adobe Passからsoftware\_statementおよびredirect\_uriを取得します（TVE Dashboard）

b. これらの値をAdobe Pass SDKに渡すには、次の2つのオプションがあります。

strings.xmlに次を追加します。

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

AccessEnabler.getInstance （appContext,softwareStatementを呼び出します。
redirectUrl）


### &#x200B;2. アプリケーションの設定

a. setRequestor （requestor\_id）

SDKでは、次の操作を行います。

- アプリケーションを登録：**software\_statement**&#x200B;を使用すると、SDKは&#x200B;**client\_id、client\_secret、client\_id\_issued\_at、redirect\_uris、grant\_types**&#x200B;を取得します。 この情報は、アプリケーションの内部ストレージに保存されます。

- client\_id、client\_secret、grant\_type=&quot;client\_credentials&quot;を使用して&#x200B;**access\_token**&#x200B;を取得します。 このaccess\_tokenは、SDKからAdobe Pass サーバーへの呼び出しごとに使用されます

**トークンエラー応答：**

| エラー応答 | | |
| --- | --- | --- |
| HTTP 400 （不正なリクエスト） | {&quot;error&quot;: &quot;invalid\_request&quot;} | 要求に必要なパラメーターが含まれていない、サポートされていないパラメーター値（付与タイプ以外）が含まれている、パラメーターを繰り返す、複数の資格情報が含まれている、クライアントを認証するための複数のメカニズムを利用する、またはその他の方法で不正な形式である。 |
| HTTP 400 （不正なリクエスト） | {&quot;error&quot;: &quot;invalid\_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDKは、認証サーバーに再度登録する必要があります。 |
| HTTP 400 （不正なリクエスト） | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 認証されたクライアントは、この承認付与タイプを使用する権限がありません。 |

- MVPDでパッシブ認証が必要な場合は、「Chromeカスタム」タブが開き、そのMVPDでパッシブ認証が実行され、完了すると閉じます

b. checkAuthentication （）

- true：認証に移動
- false :MVPDを選択に移動します

c. getAuthentication :SDKでは、呼び出しパラメーターに&#x200B;**access_token**&#x200B;が含まれます

- mvpd記憶：setSelectedProvider （mvpd_id）に移動
- mvpdが選択されていません：displayProviderDialog
- mvpdを選択：setSelectedProvider （mvpd_id）に移動

d. setSelectedProvider

- mvpd\_id認証URLがChromeCustomTabsに読み込まれます
- ログインに成功しました：delegate.setAuthenticationStatus （SUCCESS）
- ログインがキャンセルされました：MVPDの選択範囲をリセット
- 認証が完了したときに取得するURL スキームは、「adobepass://redirect_uri」として確立されます

e. get/checkAuthorization :SDKには、Authorization: Bearer **access_token**&#x200B;として&#x200B;**access_token**&#x200B;がヘッダーに含まれます

- 認証が成功した場合、を取得するための呼び出しが行われます。
メディアトークン

f. ログアウト :

- SDKは、現在の依頼者の有効なトークンを削除します（SSOを介して取得した認証ではなく、他のアプリケーションで取得した認証は有効のままです）
- SDKはChrome カスタムタブを開いて、mvpd_id ログアウトエンドポイントにアクセスします。 完了すると、Chrome カスタムタブは閉じられます
- ログアウトが完了した瞬間をキャプチャするために、URL スキームは「adobepass://logout」として確立されます
- logoutはsendTrackingData （new Event （EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR）とコールバック :setAuthenticationStatus （0,&quot;Logout&quot;）をトリガーします

**注：**&#x200B;各呼び出しには&#x200B;**access_tokenが必要なため、以下のエラーコードは**&#x200B;個がSDKで処理されます。


| エラー応答 | | |
| --- | ---|--- |
| invalid_request | 400 | リクエストの形式が正しくありません。 SDKは、サーバーへの呼び出しの実行を停止する必要があります。 |
| invalid_client | 403 | クライアント IDは、リクエストの実行を許可されなくなりました。 Sdkは、クライアント登録を再度実行する必要があります。 |
| access_denied | 401 | access\_tokenが無効です。 sdkは新しいaccess_tokenをリクエストする必要があります。 |
