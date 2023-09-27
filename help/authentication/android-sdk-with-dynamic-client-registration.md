---
title: 動的クライアント登録を使用する Android SDK
description: 動的クライアント登録を使用する Android SDK
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---

# 動的クライアント登録を使用する Android SDK {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#Intro}

Android 向け Android AccessEnabler SDK が、セッション cookie を使用せずに認証を有効にするように変更されました。 クッキーへのアクセスを制限するブラウザーが増えているので、認証を許可するには別の方法を使用する必要があります。

Android の場合、Chrome のカスタムタブを使用すると、他のアプリケーションからの cookie へのアクセスが制限されます。

>**Android SDK 3.0.0** を紹介します。

- 動的クライアント登録は、署名されたリクエスト元 ID とセッション cookie の認証に基づいて、現在のアプリ登録メカニズムに代わるものです
- 認証フロー用の Chrome カスタムタブ

>[!NOTE]
>
>Chrome カスタムタブをサポートしていない古い Android バージョンの場合、以前の AccessEnabler SDK バージョンと同様に、WebView 認証が使用されます。


## 動的クライアントの登録 {#DCR}

Android SDK v3.0 以降では、 [動的クライアントの登録](/help/authentication/dynamic-client-registration.md).


## 機能のデモ {#Demo}

ご覧ください [このウェビナー](https://my.adobeconnect.com/pzkp8ujrigg1/) これは、この機能の詳細なコンテキストを提供し、 TVE Dashboard を使用して software 文を管理する方法と、Android SDK の一部としてAdobeが提供するデモアプリケーションを使用して生成された文をテストする方法に関するデモを含みます。

## API の変更点 {#API}


### Factory.getInstance

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに 1 つの Access Enabler インスタンスが必要です。

| API 呼び出し：コンストラクター |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        AccessEnablerException をスロー |


**可用性：** v3.0 以降

**パラメーター：**

- *appContext*:Android アプリケーションコンテキスト
- softwareStatement: TVE Dashboard から取得した値、または *null* &quot;software\_statement&quot;が strings.xml に設定されている場合
- redirectUrl ：一意の url。TVE ダッシュボードに明示的に追加された逆順のドメインの 1 つ。 *null* &quot;redirect\_uri&quot;が strings.xml に設定されている場合

注意： softwareStatement または redirectUrl が無効な場合、アプリケーションは AccessEnabler を初期化しないか、Adobe Pass認証と承認のためにアプリケーションを登録します。
</br>
注意： strings.xml の redirectUrl パラメーターまたは redirect\_uri は、逆の順序でアプリケーションの TVE ダッシュボードに追加されるドメインの値である必要があります（例： TVE ダッシュボードに追加されるドメイン「adobe.com」の場合、redirectUrl は「com.adobe」である必要があります）。


### setRequestor

**説明：** チャネルの ID を確立します。 各チャネルには、Adobe Pass認証システムのAdobeへの登録時に、一意の ID が割り当てられます。 SSO とリモートトークンを扱う場合、アプリケーションがバックグラウンドになっているときに認証状態が変わる可能性があります。システム状態と同期するために、アプリケーションがフォアグラウンドに移行したときに setRequestor を再び呼び出すことができます。

サーバ応答には、MVPD のリストと、チャネルの ID に添付される設定情報が含まれます。 サーバの応答は、Access Enabler コードによって内部的に使用されます。 操作のステータス (SUCCESS/FAIL) のみが、setRequestorComplete() コールバックを介してアプリケーションに表示されます。

次の場合、 *url* パラメーターを使用しない場合、結果のネットワーク呼び出しでは、デフォルトのサービスプロバイダー URL(Adobeのリリース/実稼動環境 ) がターゲットになります。

値が *url* パラメーターを渡すと、結果のネットワーク呼び出しでは、 *url* パラメーター。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする際には、最初の応答が優先されます。 リスト内の各 MVPD に対して、Access Enabler は関連するサービスプロバイダの URL を記憶します。 以降のすべてのエンタイトルメントリクエストは、設定フェーズでターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスト元の設定 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**可用性：** v3.0 以降

| API 呼び出し：リクエスト元の設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**可用性：** v3.0 以降

**パラメーター：**

- *requestorID*：チャネルに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。
- *url*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で異なるインスタンスが使用される場合があります）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その際、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。

廃止：

- *signedRequestorID*：秘密鍵でデジタル署名された要求者 ID のコピー。 <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**コールバックがトリガーされました：** `setRequestorComplete()`

### ログアウト

**説明：** ログアウトフローを開始するには、このメソッドを使用します。 ログアウトは、一連の HTTP リダイレクト操作の結果です。これは、ユーザーがAdobe Pass認証サーバーと MVPD サーバーの両方からログアウトする必要があるためです。 その結果、このフローは ChromeCustomTab ウィンドウを開いてログアウトを実行します。

| API 呼び出し：ログアウトフローの開始 |
| --- |
| public void logout() |

**可用性：** v3.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`
</br></br>

## プログラマー実装フロー {#Progr}

### **1. アプリを登録**

a. software\_statement を取得し、Adobe Pass( TVE Dashboard ) から redirect\_uri を取得します。

b.これらの値をAdobe Pass SDK に渡す方法は 2 つあります。

strings.xml で、以下を追加します。

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

AccessEnabler.getInstance(appContext,softwareStatement, redirectUrl) を呼び出します


### 2.アプリケーションの設定

a. setRequestor(requestor\_id)

SDK は次の操作を実行します。

- 登録アプリケーション：使用 **software\_statement**&#x200B;を取得する場合、SDK は **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. この情報は、アプリケーションの内部ストレージに保存されます。

- 取得する **access\_token** client\_id、client\_secret および grant\_type=&quot;client\_credentials&quot;を使用します。 このアクセス\_token は、SDK がAdobe Passサーバーに対しておこなう呼び出しごとに使用されます

**トークンエラー応答：**

| エラー応答 | | |
| --- | --- | --- |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_request&quot;} | リクエストに必須のパラメーターがない、サポートされていないパラメーター値（付与タイプ以外）が含まれている、パラメーターを繰り返す、複数の資格情報を含む、クライアントを認証する複数のメカニズムを利用する、またはその他の形式が正しくありません。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK は、認証サーバーに再度登録する必要があります。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 認証済みのクライアントは、この認証付与タイプの使用を許可されていません。 |

- MVPD がパッシブ認証を必要とする場合、Chrome カスタムタブは、その MVPD でパッシブを実行するために開き、完了時に閉じます

b. checkAuthentication()

- true ：認証に移動
- false : MVPD を選択に移動します。

c. getAuthentication :SDK には次が含まれます。 **access_token** 呼び出しパラメーター内

- mvpd mored : setSelectedProvider(mvpd_id) に移動
- mvpd が選択されていません： displayProviderDialog
- mvpd selected : setSelectedProvider(mvpd_id) に移動します。

d. setSelectedProvider

- mvpd\_id 認証 URL が ChromeCustomTabs に読み込まれています
- ログイン成功： delegate.setAuthenticationStatus ( SUCCESS )
- ログインがキャンセルされました： MVPD の選択をリセット
- 認証が完了した時点を取り込むために、URL スキームは「adobepass://redirect_uri」として確立されます。

e. get/checkAuthorization :SDK には次が含まれます。 **access_token** Authorization としてのヘッダー： Bearer **access_token**

- 認証が成功した場合は、メディアトークンを取得するための呼び出しがおこなわれます。

f.ログアウト：

- SDK は現在の要求元に対して有効なトークンを削除します（SSO を介さずに他のアプリケーションで取得された認証は有効なままです）
- SDK は Chrome カスタムタブを開き、mvpd_id ログアウトエンドポイントに到達します。 完了すると、Chrome カスタムタブは閉じられます
- ログアウトが完了した瞬間を取り込むために、URL スキームは「adobepass://logout」として確立されます。
- logout は sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) および callback : setAuthenticationStatus(0,&quot;Logout&quot;) をトリガーします。

**注意：** 各呼び出しでは **access_token,** 以下の考えられるエラーコードは、SDK で処理されます。


| エラー応答 | | |
| --- | ---|--- |
| invalid_request | 400 | リクエストの形式が正しくありません。 SDK は、サーバーへの呼び出しの実行を停止する必要があります。 |
| invalid_client | 403 | クライアント ID は、要求を実行できなくなりました。 SDK は、クライアント登録を再度実行する必要があります。 |
| access_denied | 401 | アクセス\_token が無効です。 SDK は新しい access_token をリクエストする必要があります。 |
