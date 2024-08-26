---
title: Dynamic Client Registration を使用したAndroid SDK
description: Dynamic Client Registration を使用したAndroid SDK
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '1278'
ht-degree: 0%

---

# Dynamic Client Registration を使用したAndroid SDK {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#Intro}

Android用Android AccessEnabler SDK は、セッション Cookie を使用せずに認証を有効にするように変更されました。 cookie へのアクセスを制限するブラウザーが増えているので、認証を許可するには別の方法を使用する必要があります。

Androidの場合、Chromeのカスタムタブを使用すると、他のアプリケーションからの Cookie へのアクセスが制限されます。

>**Android SDK 3.0.0** には次が導入されています。

- dynamic client registration は、署名済みリクエスター ID とセッション cookie 認証に基づいて、現在のアプリ登録メカニズムを置き換えます
- Chrome認証フローのカスタムタブ

>[!NOTE]
>
>Chromeを使用していない古いバージョンのAndroidの場合、カスタムタブのサポートでは、古い AccessEnabler SDK バージョンと同様の WebView 認証を使用します。


## 動的なクライアント登録 {#DCR}

Android SDK v3.0 以降では、[Dynamic Client Registration Overview](./dcr-api/dynamic-client-registration-overview.md) で定義されている動的クライアント登録手順を使用します。


## 機能デモ {#Demo}

機能のコンテキストを詳しく説明する [ このウェビナー ](https://my.adobeconnect.com/pzkp8ujrigg1/) をご覧ください。これには、TVE ダッシュボードを使用してソフトウェアステートメントを管理する方法と、Android SDK の一部としてAdobeから提供されるデモアプリケーションを使用して生成されたステートメントをテストする方法のデモが含まれています。

## API の変更点 {#API}


### Factory.getInstance

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーション・インスタンスごとに 1 つの Access Enabler インスタンスが必要です。

| API 呼び出し：コンストラクター |
| --- |
| public static AccessEnabler getInstance （Context appContext, String softwareStatement, String redirectUrl） <br>        AccessEnablerException をスロー |


**提供：** v3.0 以降

**パラメーター：**

- *appContext*:Android アプリケーションコンテキスト
- softwareStatement: TVE ダッシュボードから取得された値。または string.xml で「software\_statement」が設定されている場合は *null*
- redirectUrl：一意の URL です。TVE ダッシュボードに明示的に追加された、逆順のドメインの 1 つです。string.xml で *redirect\_uri」が設定されている場合は null* になります。

注意：無効な softwareStatement または redirectUrl は、アプリケーションが AccessEnabler を初期化しないか、アプリケーションをAdobe Pass認証および認証用に登録します。
</br>
注意：strings.xml の redirectUrl パラメーターまたは redirect\_uri は、逆順でアプリケーションに対して TVE ダッシュボードで追加されたドメインの値にする必要があります（例：TVE ダッシュボードで追加されたドメイン「adobe.com」の場合、redirectUrl は「com.adobe」にする必要があります）。


### setRequestor

**説明：** チャネルの ID を確立します。 各チャネルには、Adobe Pass Authentication System のAdobeに登録すると、一意の ID が割り当てられます。 SSO およびリモート・トークンを処理する場合、アプリケーションがバックグラウンドにある場合に認証状態が変更される可能性があります。アプリケーションがフォアグラウンドになると、システム状態と同期するために setRequestor を再度呼び出すことができます（SSO が有効な場合はリモート・トークンを取得し、その間にログアウトが発生した場合はローカル・トークンを削除します）。

サーバー応答には、MVPD のリストと、チャネルの ID に添付されたいくつかの設定情報が含まれています。 サーバ応答は、アクセス イネーブラ コードによって内部的に使用されます。 setRequestorComplete （） コールバックを使用すると、操作のステータス（SUCCESS/FAIL）のみがアプリケーションに表示されます。

*urls* パラメーターを使用しない場合、生成されるネットワーク呼び出しは、デフォルトのサービスプロバイダー URL （Adobeリリース/実稼動環境）をターゲットにします。

*urls* パラメーターに値を指定すると、結果として得られるネットワーク呼び出しは、*urls* パラメーターで指定されるすべての URL をターゲットにします。 すべての設定要求が、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする場合は、最初のレスポンダーが優先されます。 Access Enabler は、リスト内の各 MVPD に対して、関連するサービス プロバイダの URL を記憶します。 以降のすべての使用権限リクエストは、設定段階でターゲット MVPD とペアになったサービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスター設定 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**提供：** v3.0 以降

| API 呼び出し：リクエスター設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**提供：** v3.0 以降

**パラメーター：**

- *requestorID*：チャネルに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。
- *urls*：オプションのパラメーターです。デフォルトでは、Adobe サービスプロバイダーが使用されます [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/)。 この配列を使用すると、Adobeが提供する認証サービスと承認サービスのエンドポイントを指定できます（デバッグ目的で別のインスタンスを使用することもできます）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その場合、MVPD リストはすべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダー、つまり、最初に応答し、その MVPD をサポートするプロバイダーに関連付けられます。

非推奨（廃止予定）:

- *signedRequestorID*：秘密鍵でデジタル署名されたリクエスター ID のコピー。<!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->。

**コールバックがトリガーされました：** `setRequestorComplete()`

### ログアウト

**説明：** このメソッドを使用して、ログアウトフローを開始します。 ログアウトは、ユーザーがAdobe Pass認証サーバーと MVPD の両方のサーバーからログアウトする必要があるために、一連の HTTP リダイレクト操作の結果です。 その結果、このフローでは、ログアウトを実行するための ChromeCustomTab ウィンドウが開きます。

| API 呼び出し：ログアウトフローの開始 |
| --- |
| public void logout （） |

**提供：** v3.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`
</br></br>

## プログラマ実装フロー {#Progr}

### **1.アプリケーションの登録**

a. Adobe Passから software\_statement と redirect\_uri を取得する（TVE Dashboard）

b.これらの値をAdobe Pass SDK に渡す方法は 2 つあります。

strings.xml に次を追加します。

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

AccessEnabler.getInstance （appContext,softwareStatement,
redirectUrl）


### 2. アプリケーションの設定

a. setRequestor （requestor\_id）

SDK は次の操作を実行します。

- アプリケーションの登録：**software\_statement** を使用して、SDK は **client\_id、client\_secret、client\_id\_issued\_at、redirect\_uris、grant\_types** を取得します。 この情報は、アプリケーションの内部ストレージに保存されます。

- client\_id、client\_secret および grant\_type=&quot;client\_credentials **を使用して、** access\_token&quot;を取得します。 この access\_token は、SDK がAdobe Pass サーバーに対して行う各呼び出しで使用されます

**トークンエラー応答：**

| エラーの応答 | | |
| --- | --- | --- |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_request&quot;} | 要求に必須パラメーターがないか、サポートされていないパラメーター値（許可タイプ以外）が含まれているか、パラメーターを繰り返しているか、複数の資格情報が含まれているか、クライアントの認証に複数のメカニズムが使用されているか、その他の形式が正しくありません。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;invalid\_client&quot;} | クライアントが不明なため、クライアント認証に失敗しました。 SDK は、再度認証サーバーに登録する必要があります。 |
| HTTP 400 （無効なリクエスト） | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | 認証済みクライアントには、この認証付与タイプの使用が許可されていません。 |

- mvpd がパッシブ認証を必要とする場合、Chromeのカスタムタブが開き、その MVPD でパッシブ認証を実行し、完了すると閉じます

b. checkAuthentication （）

- true：認証に移動します
- false : MVPD の選択に移動します。

c. getAuthentication :SDK は、呼び出しパラメーターに **access_token** を含めます

- mvpd remembered : setSelectedProvider （mvpd_id）に移動します
- mvpd が選択されていません：displayProviderDialog
- mvpd selected :setSelectedProvider （mvpd_id）に移動します

d. setSelectedProvider

- mvpd\_id 認証 URL は、ChromeCustomTabs に読み込まれる
- ログイン成功：delegate.setAuthenticationStatus （成功）
- ログインがキャンセルされました：MVPD 選択をリセット
- 認証が完了したときにキャプチャするための URL スキームが「adobepass://redirect_uri」として確立されています

e. get/checkAuthorization :SDK は、ヘッダーの **access_token** を Authorization:Bearer **access_token** として含めます。

- 認証に成功した場合、
メディアトークン

f. ログアウト :

- SDK は、現在の要求者の有効なトークンを削除します（SSO を介さずに他のアプリケーションによって取得された認証は有効なままです）
- SDK は、mvpd_id ログアウトエンドポイントに到達するために、Chromeのカスタムタブを開きます。 完了すると、Chromeのカスタムタブは閉じられます
- ログアウトが完了した瞬間をキャプチャするために、URL スキームは「adobepass://logout」として確立されます
- logout は、sendTrackingData （new Event （EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR）と callback : setAuthenticationStatus （0,&quot;Logout&quot;）をトリガーにします。

**注意：** 各呼び出しには **access_token が必要なため** 以下のエラーコードが SDK で処理されます。


| エラーの応答 | | |
| --- | ---|--- |
| invalid_request | 400 | リクエストの形式が正しくありません。 SDK は、サーバーへの呼び出しの実行を停止する必要があります。 |
| invalid_client | 403 | このクライアント ID は、要求の実行が許可されなくなりました。 SDK は、クライアント登録を再度実行する必要があります。 |
| access_denied | 401 | access\_token が無効です。 sdk は、新しい access_token をリクエストする必要があります。 |
