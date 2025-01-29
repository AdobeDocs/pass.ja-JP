---
title: Android SDK API リファレンス
description: Android SDK API リファレンス
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '4560'
ht-degree: 0%

---

# （従来の）Android SDK API リファレンス {#android-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#intro}

このドキュメントでは、Adobe Pass Authentication バージョン 1.7 以降でサポートされる、Adobe Pass認証用のAndroid SDKで公開されたメソッドとコールバックについて詳しく説明します。 ここで説明するメソッドとコールバック関数は、AccessEnabler.h および EntitlementDelegate.h ヘッダーファイルで定義されます。

最新のAndroid AccessEnabler SDKについては、[https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) を参照してください。


**注意：** Adobe Pass Authentication チームでは、Adobe Pass Authentication *パブリック* API のみを使用することをお勧めします。

- パブリック API は、サポートされているすべてのクライアントタイプで利用可能 *かつ完全にテスト済み* です。 すべての公開機能で、各クライアントタイプに、関連するメソッドの対応するバージョンがあることを確認します。</span>
- 公開 API は、後方互換性をサポートし、パートナー統合が損なわれないようにするために、できるだけ安定している必要があります。 ただし、*非* 公開 API の場合、当社は将来の時点で署名を変更する権利を留保します。 現在のパブリック Adobe Pass認証 API 呼び出しの組み合わせではサポートできない特定のフローが発生した場合は、お知らせいただくのが最善の方法です。 お客様のニーズを考慮して、公開 API を変更し、今後も安定したソリューションを提供できます。

## ANDROID API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- 事前認証
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [ログアウト](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーション・インスタンスごとに 1 つの Access Enabler インスタンスが必要です。

| API 呼び出し：コンストラクター |
| --- |
| **public static** AccessEnabler **getInstance** （Context appContext, String softwareStatement, String redirectUrl） <br>        **throws** AccessEnablerException <br><br>**public static** AccessEnabler getInstance （Context appContext, String env_url, String softwareStatement, String redirectUrl） <br>**throws** AccessEnablerException |

**提供：** v3.1.2 以降

**パラメーター：**

- *appContext*:Android アプリケーションコンテキスト。
- env\_url: Adobeのステージング環境を使用してテストする場合は、env\_url を「sp.auth-staging.adobe.com」に設定できます

**非推奨：**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**説明：** プログラマーの ID を確立します。 各プログラマーには、Adobe Pass Authentication System のAdobeに登録すると、一意の ID が割り当てられます。 SSO およびリモート・トークンを処理する場合、アプリケーションがバックグラウンドにある場合に認証状態が変更される可能性があります。アプリケーションがフォアグラウンドになると、システム状態と同期するために setRequestor を再度呼び出すことができます（SSO が有効な場合はリモート・トークンを取得し、その間にログアウトが発生した場合はローカル・トークンを削除します）。

サーバー応答には、MVPD のリストと、プログラマーの ID に添付されたいくつかの設定情報が含まれています。 サーバ応答は、アクセス イネーブラ コードによって内部的に使用されます。 setRequestorComplete （） コールバックを使用すると、操作のステータス（SUCCESS/FAIL）のみがアプリケーションに表示されます。

*urls* パラメーターを使用しない場合、生成されるネットワーク呼び出しは、デフォルトのサービスプロバイダー URL （Adobeリリース/実稼動環境）をターゲットにします。

*urls* パラメーターに値を指定すると、結果として得られるネットワーク呼び出しは、*urls* パラメーターで指定されるすべての URL をターゲットにします。 すべての設定要求が、別々のスレッドで同時にトリガーされます。 MVPD のリストをコンパイルする場合は、最初のレスポンダーが優先されます。 Access Enabler は、リスト内の各MVPDについて、関連するサービス プロバイダの URL を記憶します。 以降のすべての使用権限リクエストは、設定段階でターゲット MVPDとペアになっていた、サービスプロバイダーに関連付けられた URL に送られます。

| API 呼び出し：リクエスター設定 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**提供：** v3.0 以降

| API 呼び出し：リクエスター設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**提供：** v3.0 以降


**パラメーター：**

- *requestorID*：プログラマーに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。

- *signedRequestorID*：秘密鍵でデジタル署名されたリクエスター ID のコピー。<!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->。

- *urls*：オプションのパラメーターです。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと承認サービスのエンドポイントを指定できます（デバッグ目的で別のインスタンスを使用することもできます）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その場合、MVPDのリストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー（最初に応答し、そのMVPDをサポートするプロバイダー）に関連付けられます。

**コールバックがトリガーされました：** `setRequestorComplete()`

非推奨（廃止予定）:

    public void setRequestor （String requestorId, String signedRequestorId） 
    
    public void setRequestor （String requestorId, String signedRequestorId, ArrayList&lt;String> url） 

[Android API に戻る…](#api)

### setRequestorComplete {#setRequestorComplete}

**説明：** 構成フェーズが完了したことをアプリケーションに通知する Access Enabler によってトリガーされるコールバック。 これは、アプリが使用権限リクエストの発行を開始できることを示すシグナルです。 設定フェーズが完了するまで、アプリケーションが使用権限リクエストを発行することはできません。

| コールバック：リクエスターの設定完了 |
| --- |
| java public void setRequestorComplete （int ステータス） |

**提供：** v1.0 以降

**パラメーター：**

- *ステータス*：次のいずれかの値を取ることができます。
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` – 構成フェーズが正常に完了しました
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` – 構成フェーズが失敗しました
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` – 構成フェーズが正常に完了しました
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` – 構成フェーズが失敗しました

**Trigger by:** `setRequestor()`

[Android API に戻る…](#api)

### setOptions {#setOptions}

**説明：** グローバル SDK オプションを設定します。 **Map\&lt;String, String\>** を引数として受け入れます。 マップからの値は、SDKが行うすべてのネットワーク呼び出しと共にサーバーに渡されます。

これらの値は、現在のフロー（認証/承認）とは無関係にサーバーに渡されます。 値を変更する場合は、このメソッドをいつでも呼び出すことができます。

| API 呼び出し：setOptions |
| --- |
| public void setOptions （HashMap&lt;String,String> options） |

**提供：** v1.9.2 以降

**パラメーター：**

- *options*：グローバル SDK オプションを含む Map&lt;String, String>。 現在、次のオプションを使用できます。
   - **applicationProfile** – この値に基づいてサーバーを設定するために使用できます。
   - **ap_vi** - Experience Cloud ID （visitorID）。 この値は、後で高度な分析レポートに使用できます。
   - **ap_ai** - Advertising ID
   - **device_info** – ここで説明するクライアント情報：[ クライアント情報デバイス接続とアプリケーションを渡す ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。

[先頭に戻る…](#apis)


### checkAuthentication {#checkAuthN}

**説明：** 認証ステータスを確認します。 これを行うには、ローカルトークンのストレージスペースで有効な認証トークンを検索します。 このメソッドはネットワーク呼び出しを実行しないので、メインスレッドで呼び出すことをお勧めします。 アプリケーションがユーザーの認証ステータスをクエリし、それに応じて UI を更新するために使用されます（例：ログイン/ログアウト UI を更新）。 認証状態は、[*setAuthenticationStatus （）*](#setAuthNStatus) コールバックを介してアプリケーションに伝えられます。

MVPDが「リクエスターごとの認証」機能をサポートしている場合、1 台のデバイスに複数の認証トークンを保存できます。  この機能について詳しくは、『Android技術概要』の [ キャッシュのガイドライン ](#$caching) 節を参照してください。

| API 呼び出し：認証ステータスの確認 |
| --- |
| public void checkAuthentication （） |

**提供：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`

[Android API に戻る…](#api)


### getAuthentication {#getAuthN}

**説明：** 完全認証ワークフローを開始します。 まず、認証ステータスを確認します。 まだ認証されていない場合は、認証フロー state-machine が起動します。

- 最後の認証が成功した場合、MVPD selection フェーズはスキップされ、[*navigateToUrl （）*](#navigagteToUrl) コールバックがトリガーされます。 アプリケーションは、このコールバックを使用して、MVPDのログインページを持つユーザーを表す WebView コントロールをインスタンス化します。
- 最後の認証の試行が失敗した場合、またはユーザーが明示的にログアウトした場合、[*displayProviderDialog （）*](#displayProviderDialog) コールバックがトリガーされます。 アプリケーションは、このコールバックを使用してMVPD選択 UI を表示します。 また、[setSelectedProvider （） ](#setSelectedProvider) メソッドを使用して Access Enabler ライブラリにユーザーのMVPD選択を通知することにより、認証フローを再開する必要があります。

ユーザーの資格情報はMVPDのログインページで確認されるため、ユーザーがMVPDのログインページで認証されている間に行われる複数のリダイレクト操作をモニタリングするには、アプリケーションが必要です。 正しい資格情報を入力すると、WebView コントロールは、*AccessEnabler.ADOBEPASS\_REDIRECT\_URL* 定数で定義されたカスタム URL にリダイレクトされます。 この URL は、WebView によって読み込まれることを意図していません。 アプリケーションはこの URL をインターセプトし、ログインフェーズが完了したことを示すシグナルとしてこのイベントを解釈する必要があります。 次に、認証フローを完了するために、アクセス イネーブラに制御を渡す必要があります（*getAuthenticationToken （）* メソッドを呼び出します）。

MVPDが「要求者ごとの認証」機能をサポートしている場合、1 台のデバイスに複数の認証トークンを格納できます（プログラマーごとに 1 つ）。  この機能について詳しくは、『Android技術概要』の [ キャッシュのガイドライン ](#$caching) 節を参照してください。

最後に、*setAuthenticationStatus （）* コールバックを介して認証ステータスがアプリケーションに伝えられます。



| API 呼び出し：認証フローを開始します |
| --- |
| public void getAuthentication （） |

**提供：** v1.0 以降

| API 呼び出し：認証フローを開始します |
| --- |
| public void getAuthentication （boolean forceAuthN, Map&lt;String, Object> genericData） |

**提供：** v1.8 以降

**パラメーター：**

- *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始する必要があるかどうかを指定するフラグ。
- *data*：有料テレビパスサービスに送信されるキーと値のペアで構成されるマップ。 Adobeはこのデータを使用して、SDKを変更せずに将来の機能を有効にできます。

**コールバックがトリガーされました：** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Android API に戻る…](#api)

### displayProviderDialog {#displayProviderDialog}

**説明** アクセスイネーブラによってトリガーされるコールバックで、適切な UI 要素をインスタンス化して、ユーザーが目的のMVPDを選択できるようにする必要があることをアプリケーションに通知します。 コールバックは、MVPD オブジェクトのリストに、選択 UI パネルを正しく作成するのに役立つ追加情報（MVPDのロゴを指す URL、わかりやすい表示名など）を提供します。

ユーザーが目的のMVPDを選択したら、*setSelectedProvider （）を呼び出し、ユーザーの選択に対応するMVPDの ID を渡すことにより* 上位レイヤーのアプリケーションが認証フローを再開する必要があります。

>[!NOTE]
>
> 認証フローの中止
> </br></br>
> これは、ユーザーが「戻る」ボタンを押すことができる点であることに注意してください。これは、認証フローの中止に相当します。 このようなシナリオでは、アプリケーションで `setSelectedProvider()` メソッドを呼び出し、*null* をパラメータとして渡して、アクセス イネーブラに認証状態マシンをリセットする機会を与える必要があります。

| コールバック：MVPD選択 UI の表示 |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**提供：** v1.0 以降

**パラメーター**:

- *mvpds*:MVPD selection UI 要素の構築にアプリケーションが使用できるMVPD関連の情報を保持しているMVPD オブジェクトのリスト。

**Trigger by:** `getAuthentication(), getAuthorization()`

[Android API に戻る…](#api)


### setSelectedProvider {#setSelectedProvider}

**説明：** このメソッドは、ユーザーが選択したMVPDをアクセス イネーブラに通知するためにアプリケーションによって呼び出されます。 アプリケーションはこの方法を使用して、認証に使用するサービスプロバイダーを選択または変更できます。

選択したMVPDが TempPass MVPDの場合、そのMVPDで自動的に認証が行われます。後で getAuthentication （）を呼び出す必要はありません。

getAuthentication （） メソッドに追加のパラメーターが指定されている場合、プロモーションの一時パスでは使用できないことに注意してください。

パラメータとして *null* を渡した場合、アクセス イネーブラは、ユーザーが認証フローをキャンセルした（[ 戻る ] ボタンを押した）ことを前提に、認証ステート マシンをリセットし、`AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` エラーコードの *setAuthenticationStatus （）* コールバックを呼び出して応答します。

| API 呼び出し：現在選択されているプロバイダーを設定します |
| --- |
| public void setSelectedProvider （String mvpdId） |

**提供：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Android API に戻る…](#api)


### navigateToUrl {#navigagteToUrl}

**非推奨：** Android SDK 3.0 以降では、Chromeのカスタムタブがデバイスに存在しない場合にのみ、navigateToUrl が使用されます

**説明：** アクセスイネーブラによってトリガーされるコールバック。資格情報を入力するためにMVPDのログインページを表示する必要があることをアプリケーションに知らせます。 アクセス イネーブラは、パラメータとしてMVPD ログイン ページの URL を渡します。 WebView コントロールをインスタンス化し、この URL にダイレクトするには、アプリケーションが必要です。 また、アプリケーションは、WebView コントロールによって読み込まれる URL を監視し、`AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` 定数によって定義されるカスタム URL をターゲットにするリダイレクト操作をインターセプトするために必要です。 このイベントが発生した場合、アプリケーションは *getAuthenticationToken （）* メソッドを呼び出して、WebView コントロールを閉じるか非表示にし、アクセス イネーブラ ライブラリにコントロールを戻す必要があります。 Access Enabler は、バックエンド・サーバから認証トークンを取得し、そのトークンをトークン・ストレージにローカルに保存することによって、認証フローを完了します。

>[!WARNING]
>
> **認証フローの中止** <br> ユーザーが「戻る」ボタンを押せる点が、認証フローの中止と同じであることに注意してください。 このようなシナリオでは、_null_ をパラメーターとして渡して _setSelectedProvider （）_ メソッドを呼び出し、アクセス イネーブラで認証状態マシンをリセットできるようにする必要があります。

| コールバック：MVPDのログインページを表示 |
| --- |
| public void navigateToUrl （String url） |

**提供：** v1.0 以降

**パラメーター：**

- *url*:MVPDのログインページを指す URL

**Trigger by:** `getAuthentication(), setSelectedProvider()`

[Android API に戻る…](#api)


### getAuthenticationToken {#getAuthNToken}

**非推奨：** Android SDK 3.0 以降、Chromeのカスタムタブは認証に使用されるので、この方式はアプリケーションから使用されなくなりました。

**説明：** バックエンドサーバーに認証トークンをリクエストして、認証フローを完了します。 このメソッドは、MVPD ログインページをホストする WebView コントロールが、`AccessEnabler.ADOBEPASS_REDIRECT_URL` 定数によって定義されるカスタム URL にリダイレクトされるイベントに対する応答としてのみ、アプリケーションで呼び出す必要があります。

| API 呼び出し：認証トークンの取得 |
| --- |
| public void getAuthenticationToken （） |

**提供：** v1.0 以降

**パラメーター：**

- *cookie*：ターゲットドメインに設定される cookie （参照実装については、SDKのデモアプリケーションを参照してください）。

**コールバックがトリガーされました：** `setAuthenticationStatus()`、`sendTrackingData()`

[Android API に戻る…](#api)


### setAuthenticationStatus {#setAuthNStatus}

**説明：** アクセス イネーブラによってトリガーされたコールバック。このコールバックは、
認証フローのステータスの適用。 多数あります
ユーザーの結果としてこの、このフローが失敗する可能性のある場所
インタラクションまたはその他の予期しないシナリオ（ネットワークなど）が原因
接続の問題など）。 このコールバックは、
認証フローの成功/失敗ステータス。
必要に応じて、エラー理由に関する追加情報を提供します。

| コールバック：認証フローのステータスをレポートします |
| --- |
| public void setAuthenticationStatus （int status, String errorCode） |

**提供：** v1.0 以降

**パラメーター：**

- *ステータス*：次のいずれかの値を取ることができます。
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` – 認証フローが正常に完了しました
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` – 認証フローが失敗しました
- *コード*：失敗の理由。 *status* が `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` の場合、*code* は空の文字列です（つまり、`AccessEnablerConstants.USER_AUTHENTICATED` 定数によって定義されます）。 エラーが発生した場合、このパラメーターには次のいずれかの値を指定できます。
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - ユーザーが認証されていません。 ローカルトークンキャッシュに有効な認証トークンがない場合の *checkAuthentication （）* メソッド呼び出しに応答して。
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler は、上位層アプリケーションが `setSelectedProvider()` に渡された *null* 後に認証ステート マシンをリセットして、認証フローを中止します。  ユーザーが認証フローをキャンセルした（つまり、「戻る」ボタンを押した）と考えられます。
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - ネットワークが利用できないか、ユーザーが認証フローを明示的に取り消したなどの理由により、認証フローが失敗しました。

**Trigger by:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Android API に戻る…](#api)


### checkPreauthorizedResources {#checkPreauth}

>**非推奨：** Android SDK 3.6 以降、事前認証 API は checkPreauthorizedResources を置き換え、拡張エラーコードを提供します。

**説明：** このメソッドは、保護された特定のリソースを表示する権限がユーザーに既にあるかどうかを判断するためにアプリケーションで使用されます。 このメソッドの主な目的は、UI の修飾に使用する情報を取得することです（例えば、ロックアイコンとロック解除アイコンでアクセスステータスを示すなど）。

| API 呼び出し：現在選択されているプロバイダーを設定します |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**提供：** v1.3 以降

**Parameters:** `resources` パラメーターは、認証をチェックする必要があるリソースの配列です。 リストの各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、`getAuthorization()` 呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマーとMVPDまたはメディア RSS フラグメントとの間で確立された同意値である必要があります。

**コールバックがトリガーされました：** `preauthorizedResources()`

[Android API に戻る…](#api)


### checkPreauthorizedResources {#checkPreauth2}

**非推奨：** Android SDK 3.6 以降、事前認証 API は checkPreauthorizedResources を置き換え、拡張エラーコードを提供します。

**説明：** このメソッドは、保護された特定のリソースを表示する権限がユーザーに既にあるかどうかを判断するためにアプリケーションで使用されます。 このメソッドの主な目的は、UI の修飾に使用する情報を取得することです（例えば、ロックアイコンとロック解除アイコンでアクセスステータスを示すなど）。

| API 呼び出し：現在選択されているプロバイダーを設定します |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**提供：** v3.1 以降

**Parameters:** `resources` パラメーターは、認証をチェックする必要があるリソースの配列です。 リストの各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、`getAuthorization()` 呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマーとMVPDまたはメディア RSS フラグメントとの間で確立された同意値である必要があります。

`cache` パラメーターは、キャッシュされた事前認証応答を使用できるかどうかを指定します。 デフォルトのキャッシュが true の場合、SDKは、可能であれば、以前にキャッシュされた応答を返します。

**コールバックがトリガーされました：** `preauthorizedResources()`

[Android API に戻る…](#api)

### preauthorizedResources {#preauthResources}

**非推奨：** Android SDK 3.6 以降、事前認証 API は checkPreauthorizedResources を置き換え、拡張エラーコードを提供します。 preauthorizedResources コールバックは、新しい API では呼び出されません。


**説明：** checkPreauthorizedResources （）によってトリガーされるコールバック。 ユーザーが既に表示を許可されているリソースのリストを提供します。

| API 呼び出し：現在選択されているプロバイダーを設定します |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**提供：** v1.3 以降

**Parameters:** `resources` パラメーターは、ユーザーが既に表示を許可されているリソースの配列です。

**Trigger by:** `checkPreauthorizedResources()`

[Android API に戻る…](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**説明：** このメソッドは、承認ステータスを確認するためにアプリケーションで使用されます。 まず、認証ステータスを最初に確認します。 認証されていない場合、*setTokenRequestFailed （）* コールバックがトリガーされ、メソッドが終了します。 認証されたユーザーは、認証フローもトリガーします。 *getAuthorization （）* メソッドの詳細を参照してください。

| API 呼び出し：認証ステータスの確認 |
| --- |
| public void checkAuthorization （String resourceId） |

**提供：** v1.0 以降

| API 呼び出し：認証ステータスの確認 |
| --- |
| public void checkAuthorization （String resourceId, Map&lt;String, Object> genericData） |

**提供：** v1.8 以降

**パラメーター：**

- *resourceId*：ユーザーが認証をリクエストするリソースの ID。
- *data*：有料テレビパスサービスに送信されるキーと値のペアで構成されるマップ。 Adobeはこのデータを使用して、SDKを変更せずに将来の機能を有効にできます。

**コールバックがトリガーされました：** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Android API に戻る…](#api)


### <span id="getAuthZ"></span>getAuthorization

**説明：** このメソッドは、承認フローを開始するためにアプリケーションによって使用されます。 ユーザーがまだ認証されていない場合は、認証フローも開始します。 ユーザーが認証されると、アクセス イネーブラは認証トークンの要求（有効な認証トークンがローカル トークン キャッシュに存在しない場合）と短期間有効なメディア トークンの要求を発行します。 短いメディアトークンが取得されると、認証フローは完了と見なされます。 *setToken （）* コールバックがトリガーされ、短いメディアトークンがパラメーターとしてアプリケーションに配信されます。 何らかの理由で認証が失敗した場合、*tokenRequestFailed （）* コールバックがトリガーされ、エラーコードと詳細が提供されます。

| API 呼び出し：認証フローの開始 |
| --- |
| public void getAuthorization （String resourceId） |

**提供：** v1.0 以降

| API 呼び出し：認証フローの開始 |
| --- |
| public void getAuthorization （String resourceId, Map&lt;String, Object> genericData） |

**提供：** v1.8 以降

**パラメーター：**

- *resourceId*：ユーザーが認証をリクエストするリソースの ID。
- *data*：有料テレビパスサービスに送信されるキーと値のペアで構成されるマップ。 Adobeはこのデータを使用して、SDKを変更せずに将来の機能を有効にできます。

**コールバックがトリガーされました：** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **追加のコールバックがトリガーされました** <br> このメソッドは、次のコールバックもトリガーできます（フローも開始される場合）。*setAuthenticationStatus （）*、*displayProviderDialog （）*、*navigateToUrl （）*

**注意：可能な限り、getAuthorization （）の代わりに checkAuthorization （）を使用してください。 getAuthorization （） メソッドは完全な認証フローを開始します（ユーザーが認証されていない場合）。これにより、プログラマ側の実装が複雑になる可能性があります**。

[Android API に戻る…](#api)


### setToken {#setToken}

**説明：** 認証フローが正常に完了したことをアプリケーションに通知する、アクセス イネーブラによってトリガーされたコールバック。 短時間のみ有効なメディアトークンもパラメーターとして配信されます。

| コールバック：認証フローが正常に完了しました |
| --- |
| public void setToken （String token, String resourceId） |

**提供：** v1.0 以降

**パラメーター：**

- *token*：短時間のみ有効なメディアトークン
- *resourceId*：認証が取得されたリソース

**Trigger by:** `checkAuthorization()`, `getAuthorization()`


[Android API に戻る…](#api)

### tokenRequestFailed {#tokenRequestFailed}

**説明：** 認証フローが失敗したことを上位層アプリケーションに通知するアクセス イネーブラによってトリガーされたコールバック。

| コールバック：認証フローが失敗しました |
| --- |
| public void tokenRequestFailed （String resourceId, <br>        String errorCode, String errorDescription） |

**提供：** v1.0 以降

**パラメーター：**

- *resourceId*：認証が取得されたリソース
- *errorCode*：失敗シナリオに関連付けられたエラーコード。 使用可能な値：
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` – 指定されたリソースをユーザーが承認できませんでした
- *errorDescription*：失敗シナリオに関する追加の詳細。 何らかの理由でこの記述文字列が使用できない場合、Adobe Pass認証は空の文字列 **（&quot;）** を送信します。

  この文字列は、MVPDでカスタムのエラーメッセージや営業関連のメッセージを渡すために使用できます。 例えば、サブスクライバーがリソースの認証を拒否された場合、MVPDから「現在、パッケージ内のこのチャネルへのアクセス権がありません。 パッケージをアップグレードする場合は、ここをクリックしてください。」です。 メッセージは、このコールバックを通じてAdobe Pass認証によってプログラマーに渡され、プログラマーは表示または無視のオプションを持ちます。 Adobe Pass認証では、このパラメーターを使用して、エラーを引き起こした可能性のある条件の通知を提供することもできます。 例えば、「プロバイダーの認証サービスと通信する際にネットワークエラーが発生しました」などです。

**Trigger by:** `checkAuthorization(), getAuthorization()`

[Android API に戻る…](#api)

### ログアウト {#logout}

**説明：** このメソッドを使用して、ログアウトフローを開始します。 ログアウトは、ユーザーがAdobe PassMVPDサーバーと認証サーバーの両方からログアウトする必要があるために、一連の HTTP リダイレクト操作の結果です。 その結果、このフローは、Access Enabler ライブラリから発行された単純な HTTP リクエストでは完了できません。 Chromeのカスタムタブは、SDKで HTTP リダイレクト操作を実行するために使用されます。 このフローは、ユーザーに表示され、完了すると閉じられます

| API 呼び出し：ログアウトフローの開始 |
| --- |
| public void logout （） |

**提供：** v1.0 以降

**パラメーター：** なし

**トリガーされたコールバック：**

- SDK 3.0 より前のバージョンの `navigateToUrl()`
- SDK バージョン > 3.0 の `setAuthenticationStatus()`


[Android API に戻る…](#api)


### getSelectedProvider {#getSelectedProvider}

**説明：** このメソッドを使用して、現在選択されているプロバイダを確認します。

| API 呼び出し：現在選択されているMVPDを特定します |
| --- |
| public void getSelectedProvider （） |

**提供：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `selectedProvider()`

[Android API に戻る…](#api)


### <span id="selectedProvider"></span>selectedProvider

**説明：** 現在選択されているMVPDに関する情報をアプリケーションに提供する Access Enabler によってトリガーされるコールバック。

| コールバック：現在選択されているMVPDに関する情報 |
| --- |
| public void selectedProvider （Mvpd mvpd） |


**提供：** v1.0 以降

**パラメーター：**

- *mvpd*：現在選択されているMVPDに関する情報を含むオブジェクト

**Trigger by:** `getSelectedProvider()`

[Android API に戻る…](#api)


### getMetadata {#getMetadata}

**説明：** このメソッドを使用して、Access Enabler ライブラリによってメタデータとして公開された情報を取得します。 アプリケーションは、複合 MetadataKey オブジェクトを提供することでこの情報にアクセスできます。

| API 呼び出し：AccessEnabler でメタデータをクエリーする |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**提供：** v1.0 以降

プログラマーが使用できるメタデータには次の 2 種類があります。

- 静的メタデータ（認証トークン TTL、認証トークン TTL およびデバイス ID）
- ユーザーメタデータ（認証フローまたは承認フローの実行中にMVPDからユーザーのデバイスに渡される、ユーザー ID や郵便番号などのユーザー固有の情報）

**パラメーター：**

- *metadataKey*：キーと args 変数をカプセル化したデータ構造。意味は次のとおりです。
   - キーが `METADATA_KEY_USER_META` で、引数に name = `METADATA_ARG_USER_META`、value = `[metadata_name]` の SerializableNameValuePair オブジェクトが含まれる場合、ユーザーメタデータに対するクエリが実行されます。 使用可能なユーザーメタデータタイプの現在のリスト：
      - `zip` – 郵便番号

      - `householdID` – 世帯の識別子。 MVPDがサブアカウントをサポートしていない場合、これは `userID` と同じです。

      - `maxRating` - ユーザーの保護者による制限の上限

      - `userID` - ユーザー ID。 MVPDがサブアカウントをサポートしていて、ユーザーがメインアカウントでない場合、`userID` は `householdID` とは異なります。

      - `channelID` - ユーザーが表示できるチャネルのリスト
   - キーが `METADATA_KEY_DEVICE_ID` の場合、現在のデバイス ID を取得するためにクエリが実行されます。 この機能はデフォルトで無効になっており、プログラマーはイネーブルメントと料金についてAdobeに問い合わせる必要があります。
   - キーが `METADATA_KEY_TTL_AUTHZ` で、引数に name = `METADATA_ARG_RESOURCE_ID`、value = `[resource_id]` の SerializableNameValuePair オブジェクトが含まれる場合、クエリは、指定されたリソースに関連付けられた認証トークンの有効期限を取得するために実行されます。
   - キーが `METADATA_KEY_TTL_AUTHN` の場合、クエリが実行されて認証トークンの有効期限が取得されます。



>[!NOTE]
>
>SDK 3.4.0 の場合、定数 `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN`com.adobe.adobepass.accessenabler.api.profile.UserProfileService から入手できます。



>[!NOTE]
>
>プログラマーが使用できる実際のユーザーメタデータは、MVPDが提供する内容によって異なります。  このリストは、新しいメタデータが利用可能になり、Adobe Pass認証システムに追加されると、さらに拡張されます。

**コールバックがトリガーされました：** [`setMetadataStatus()`](#setMetadaStatus)

**詳細情報：**[ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

[Android API に戻る…](#api)

### setMetadataStatus {#setMetadaStatus}

**説明：** *getMetadata （）* 呼び出しを介して要求されたメタデータを配信するアクセス イネーブラによってトリガーされたコールバック。

| コールバック：メタデータ取得リクエストの結果 |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**提供：** v1.0 以降

**パラメーター：**

- *key*：メタデータ値がリクエストされるキーと関連パラメーターを含む MetadataKey オブジェクト（参照実装についてはデモアプリケーションを参照してください）。
- *result*：リクエストされたメタデータを含む複合オブジェクト。 オブジェクトには次のフィールドがあります。
   - *simpleResult*：認証 TTL、認証 TTL、デバイス ID のいずれかに対してリクエストが行われたときのメタデータ値を表す文字列。 ユーザーメタデータに対してリクエストが行われた場合、この値は null です。

   - *userMetadataResult*:JSON ユーザーメタデータペイロードの Java 表現を含むオブジェクト。\
     例：

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

は、次のように Java に翻訳されます。

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**ユーザーメタデータオブジェクトの実際の構造は、次のようになります。**

```json
          {
              updated: 1334243471,
              encrypted: ["encryptedProp"],
              data: {
                  zip: ["12345", "34567"],
                  maxRating: { 
                      "MPAA": "PG-13",
                      "VCHIP": "TV-Y", 
                      "URL": "http://exam.pl/e/manage/ratings"
                  },
                  householdID: "3456",
                  userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                  channelID: ["channel-1", "channel-2"]
              }
          }
```

単純なメタデータ（認証 TTL、承認 TTL またはデバイス ID）に対してリクエストが行われた場合、この値は null になります。

- *encrypted*：取得したメタデータを暗号化するかどうかを指定するブール値。 このパラメーターは、ユーザーメタデータリクエストでのみ重要で、常に暗号化されずに受信される静的メタデータ（例：認証 TTL）には意味を持ちません。 このパラメーターを True に設定した場合、許可リスト登録済みの秘密鍵（[`setRequestor`](#setRequestor) 呼び出しのリクエスター ID の署名に使用されるのと同じ秘密鍵）を使用して RSA 復号化を実行し、暗号化されていないユーザーメタデータ値を取得するかどうかは、プログラマー次第です。

**Trigger by:** [`getMetadata()`](#getMetadata)

**詳細情報：**[ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)


[Android API に戻る…](#api)


### getVersion {#getVersion}

**説明：** このメソッドを使用すると、AccessEnabler ライブラリのバージョンを取得できます。

| API 呼び出し：AccessEnabler バージョンの取得 |
| --- |
| ```public static String getVersion()``` |


[Android API に戻る…](#api)

</br>

## 追跡イベント {#tracking}

Access Enabler は、エンタイトルメント フローに必ずしも関連しない追加のコールバックをトリガーします。 *sendTrackingData （）* という名前のイベントトラッキングコールバック関数の実装はオプションですが、アプリケーションは特定のイベントを追跡し、認証/承認の成功/失敗の試行回数などの統計をコンパイルすることができます。 *sendTrackingData （）* コールバックの仕様を以下に示します。


### sendTrackingData {#sendTrackingData}

**説明：** アクセス イネーブラによってトリガーされたコールバックは、認証/承認フローの完了/失敗など、さまざまなイベントの発生をアプリケーションに通知します。 デバイスの種類、Access Enabler クライアントの種類、およびオペレーティング システムも sendTrackingData （）によって報告されます。

>[!WARNING]
>
> デバイスの種類とオペレーティング システムは、パブリック Java ライブラリ （[http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)）とユーザーエージェント文字列を使用して取得されます。 この情報は、操作指標をデバイスカテゴリに分類する粗い方法としてのみ提供されますが、Adobeは誤った結果に対して責任を負うことはありません。 それに応じて新しい機能を使用してください。


- デバイスタイプに指定可能な値：
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Access Enabler クライアント・タイプに指定可能な値：
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| コールバック：トラッキングイベント |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**提供：** v1.0 以降

**パラメーター：**

- *event*：トラッキングされるイベント。 次の 3 つのトラッキングイベントタイプが考えられます。
   - **authorizationDetection:** 認証トークンリクエストが返されるたびに発生します（イベントタイプは `EVENT_AUTHZ_DETECTION`）。
   - **authenticationDetection:** 認証チェックが発生するたびに（イベントタイプが `EVENT_AUTHN_DETECTION`）
   - **mvpdSelection:** MVPD選択フォームでMVPDを選択したとき（イベントタイプ：`EVENT_MVPD_SELECTION`）
- *data*：レポートされたイベントに関連付けられている追加データ。 このデータは、値のリストの形式で表示されます。

*data* の値を解釈する手順を次に示します
配列：

- イベントタイプが *`EVENT_AUTHN_DETECTION`の場合：*
   - **0** - トークンリクエストが成功したかどうか（true/false）と、上記が true の場合：
   - **1** - MVPD ID 文字列
   - **2** - GUID （md5 ハッシュ化）
   - **3** - トークンは既にキャッシュにあります（true/false）
   - **4** - デバイスタイプ
   - **5**:Access Enabler クライアント・タイプ
   - **6** - オペレーティングシステムの種類

- イベントタイプ `EVENT_AUTHZ_DETECTION` の場合
   - **0** - トークンリクエストが成功したかどうか（true/false）と成功した場合は次の値：
   - **1** - MVPD ID
   - **2** - GUID （md5 ハッシュ化）
   - **3** - トークンは既にキャッシュにあります（true/false）
   - **4** - エラー
   - **5** – 詳細
   - **6** - デバイスタイプ
   - **7**:Access Enabler クライアント・タイプ
   - **8** - オペレーティングシステムの種類

- イベントタイプ `EVENT_MVPD_SELECTION` の場合
   - **0** – 現在選択されているMVPDの ID
   - **1** - デバイスタイプ
   - **2**:Access Enabler クライアント・タイプ
   - **3** - オペレーティングシステムの種類

**トリガー：** `checkAuthentication()`、`getAuthentication()`、`checkAuthorization()`、`getAuthorization()`、`setSelectedProvider()`

[Android API に戻る…](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
