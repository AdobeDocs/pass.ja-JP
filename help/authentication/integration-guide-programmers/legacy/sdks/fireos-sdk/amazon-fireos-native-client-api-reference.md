---
title: Amazon FireOS ネイティブクライアント API リファレンス
description: Amazon FireOS ネイティブクライアント API リファレンス
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '3498'
ht-degree: 0%

---

# （レガシー） Amazon FireOS Native Client API リファレンス {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

## 概要 {#intro}

このドキュメントでは、Adobe Pass AuthenticationでサポートされるAmazon FireOS SDK for Adobe Pass Authenticationで公開されるメソッドとコールバックについて詳しく説明します。 ここで説明するメソッドとコールバック関数は、AccessEnabler.hおよびEntitlementDelegate.h ヘッダーファイルで定義されます。

最新のAmazon FireOS AccessEnabler SDKについては、<https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library>を参照してください。

>[!NOTE]
>
>Adobe Pass Authentication チームでは、Adobe Pass Authentication *public* APIのみを使用することをお勧めします。

- パブリック APIは、サポートされているすべてのクライアントタイプで&#x200B;*利用でき、完全にテストされています*。 公開されている機能については、各クライアントタイプに対応するバージョンの関連メソッドが含まれていることを確認します。
- 下位互換性をサポートし、パートナー統合が壊れないようにするために、パブリック APIは可能な限り安定している必要があります。 ただし、*非*&#x200B;公開APIについては、今後いつでも署名を変更する権利を留保します。 現在のパブリック Adobe Pass Authentication API呼び出しの組み合わせでサポートできない特定のフローが発生した場合、最善のアプローチは私たちに知らせることです。 お客様のニーズを考慮して、公開APIを変更し、安定したソリューションを提供することができます。

## Amazon FireOS SDK API {#api}

- [getInstance](#getInstance)
- [setOptions](#fire_setOption)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- [checkPreauthorizedResources](#checkPreauth)
- [preauthorizedResources](#preauthResources)
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

</br>

### Factory.getInstance {#getInstance}

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに1つのAccess Enabler インスタンスが必要です。

| API呼び出し：コンストラクター |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br>  <code>件のパブリック静的AccessEnabler getInstance （Context appContext、String env_url、String softwareStatement、String redirectUrl）がAccessEnablerExceptionをスローします </code> |

**可用性：** v3.0以降


**パラメーター：**

- *appContext*: Amazon Fire OS アプリケーションのコンテキスト。
- softwareStatement
- redirectUrl :FireOSの場合、パラメーター値は無視され、デフォルトに設定されます。adobepass://android.app
- env_url: Adobe ステージング環境を使用してテストする場合、env\_urlは「sp.auth-staging.adobe.com」に設定できます

**非推奨：**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequestor {#setRequestor}

**説明：** プログラマーのIDを確立します。 各プログラマーには、Adobe Pass認証システムにAdobeに登録する際に一意のIDが割り当てられます。 この設定は、アプリケーションのライフサイクル中に1回だけ実行する必要があります。

サーバー応答には、MVPDのリストと、プログラマーのIDに添付されているいくつかの設定情報が含まれています。 サーバー応答は、Access Enabler コードによって内部で使用されます。 setRequestorComplete （） コールバックを使用して、操作のステータス（SUCCESS/FAILなど）のみがアプリケーションに表示されます。

*urls* パラメーターが使用されていない場合、結果のネットワーク呼び出しは、デフォルトのサービスプロバイダーURL （Adobe リリース/実稼動環境）をターゲットにします。

*urls* パラメーターに値が指定されている場合、結果のネットワーク呼び出しは、*urls* パラメーターで指定されたすべてのURLをターゲットにします。 すべての設定リクエストは、別々のスレッドで同時にトリガーされます。 MVPDのリストをコンパイルする場合は、最初のレスポンダーが優先されます。 リスト内の各MVPDについて、Access Enablerは関連するサービスプロバイダーのURLを記憶します。 その後のすべての使用権限リクエストは、設定フェーズでターゲット MVPDとペア設定されたサービスプロバイダーに関連付けられたURLに送信されます。

| API呼び出し：依頼者設定 |
| --- |
| ```public void setRequestor(String requestorId)``` |


**可用性：** v3.0以降


| API呼び出し：依頼者設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**可用性：** v3.0以降


**パラメーター：**

- *requestorID*: プログラマーに関連付けられている一意のID。 Adobeで割り当てられた一意のIDを、Adobe Pass Authentication Serviceに初めて登録したときにサイトに渡します。
- *urls*: オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます（http://sp.auth.adobe.com/）。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で様々なインスタンスを使用する場合があります）。 これを使用して、複数のAdobe Pass認証サービスプロバイダーインスタンスを指定できます。 この場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。

**コールバックがトリガーされました：** `setRequestorComplete()`



**非推奨：**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**説明：**&#x200B;設定フェーズが完了したことをアプリケーションに通知するAccess Enablerによってトリガーされるコールバック。 これは、アプリが使用権限リクエストの発行を開始できることを示すシグナルです。 設定フェーズが完了するまで、アプリケーションは使用権限の要求を発行できません。

| コールバック：依頼者設定が完了しました |
| --- |
| ```public void setRequestorComplete(int status)``` |

**可用性：** v1.0以降

**パラメーター：**

- *status*：次のいずれかの値を取ることができます：
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` – 設定
フェーズが正常に完了しました
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` – 設定
フェーズが失敗しました

**トリガー：** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**説明：** グローバル SDK オプションを設定します。 引数として&#x200B;**Map\&lt;String, String\>**&#x200B;を受け入れます。 マップの値は、SDKが行うすべてのネットワーク呼び出しと共にサーバーに渡されます。

値は、現在のフロー（認証/承認）に関係なく、サーバーに渡されます。 値を変更する場合は、任意の時点でこのメソッドを呼び出すことができます。



| API呼び出し：setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**可用性：** v3.0以降

**パラメーター：**

- *options*: グローバルなSDK オプションを含むMap\&lt;String, String\>です。 現在、次のオプションを使用できます。
   - **applicationProfile** – この値に基づいてサーバー設定を行うために使用できます。
   - **ap\_vi** - Experience Cloud ID サービス。 この値は、後で高度な分析レポートに使用できます。
   - **device\_info** - デバイス情報は、**デバイス情報のクックブックを渡す**&#x200B;に記載されています

</br>

### checkAuthentication {#checkAuthN}

**説明：**&#x200B;認証ステータスを確認します。 これは、ローカルトークンのストレージ領域で有効な認証トークンを検索することで実現します。 このメソッドを呼び出すと、ネットワーク呼び出しは実行されません。 ユーザーの認証ステータスをクエリし、それに応じてUIを更新するためにアプリケーションによって使用されます（つまり、ログイン/ログアウト UIを更新します）。 認証ステータスは、[*setAuthenticationStatus （）*](#setAuthNStatus) コールバックを介してアプリケーションに通知されます。

MVPDが「リクエスト者ごとの認証」機能をサポートしている場合、複数の認証トークンをデバイスに保存できます。

| API呼び出し：認証ステータスの確認 |
| --- |
| ```public void checkAuthentication()``` |

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**説明：**&#x200B;完全な認証ワークフローを開始します。 まず、認証ステータスを確認します。 まだ認証されていない場合は、認証フローのstate-machineが開始されます。

- 最後の認証が成功した場合、MVPDの選択フェーズはスキップされ、WebView コントロールがMVPDのログインページをユーザーに表示します。
- 前回の認証が失敗した場合、またはユーザーが明示的にログアウトした場合、[*displayProviderDialog （）*](#displayProviderDialog) コールバックがトリガーされます。 アプリケーションでは、このコールバックを使用してMVPDの選択UIを表示します。 また、[setSelectedProvider （） &#x200B;](#setSelectedProvider) メソッドを使用して、Access Enabler ライブラリにユーザーのMVPDの選択を通知することで、認証フローを再開する必要もあります。

MVPDが「Authentication per Requestor」機能をサポートしている場合、1つのデバイス（プログラマーごとに1つ）に複数の認証トークンを保存できます。

最後に、認証ステータスは&#x200B;*setAuthenticationStatus （）* コールバックを介してアプリケーションに通知されます。

| API呼び出し：認証フローを開始します |
| --- |
| ```public void getAuthentication()``` |

**可用性：** v1.0以降

| API呼び出し：認証フローを開始します |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**可用性：** v1.0以降

**パラメーター：**

- *forceAuthn*: ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
- *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるマップ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。

**コールバックがトリガーされました：** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**説明** Access Enablerによってトリガーされたコールバックは、ユーザーが目的のMVPDを選択できるようにするために、適切なUI要素をインスタンス化する必要があることをアプリケーションに通知します。 このコールバックは、MVPD オブジェクトのリストに、MVPDのロゴを指すURLやわかりやすい表示名など、選択UI パネルの正しい構築に役立つ情報を提供します。

ユーザーが目的のMVPDを選択したら、上位のアプリケーションで&#x200B;*setSelectedProvider （）*&#x200B;を呼び出して、ユーザーの選択に対応するMVPDのIDを渡すことで、認証フローを再開する必要があります。


| **コールバック：MVPDの選択範囲UIの表示** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**可用性：** v1.0以降

**パラメーター**:

- *mvpds*: アプリケーションがMVPD selection UI要素の構築に使用できる、MVPD関連の情報を保持するMVPD オブジェクトのリスト。

**トリガー：** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**説明：**&#x200B;このメソッドは、ユーザーのMVPDの選択内容をAccess Enablerに通知するためにアプリケーションによって呼び出されます。 パラメーターとして&#x200B;*null*&#x200B;を渡すと、Access Enablerは現在のMVPDをnull値にリセットします。

| **API呼び出し：現在選択されているプロバイダーを設定** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**&#x200B; 可用性：**&#x200B;v 1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**説明：** Android SDKのAccess Enablerによってトリガーされたコールバック。 Amazon FireOS SDKでは無視する必要があります。

| **コールバック：MVPD ログインページの表示** |
| --- |
| ```public void navigateToUrl(String url)``` |

**可用性：** v1.0以降

**パラメーター：**

- *url*: MVPDのログインページを指すURL

**トリガー：** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**説明：** バックエンドサーバーから認証トークンを要求して、認証フローを完了します。

| **API呼び出し：認証トークンを取得** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**可用性：** v1.0以降

**パラメーター：**

- *cookie*: ターゲットドメインに設定されているCookie （参照実装については、SDKのデモアプリケーションを参照）。

**コールバックがトリガーされました：** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**説明：**&#x200B;認証のステータスをアプリケーションに通知するAccess Enablerによってトリガーされるコールバック。 認証フローが失敗する場所は、ユーザーの操作の結果か、その他の予期せぬシナリオ（ネットワーク接続の問題など）によって多くあります。 このコールバックは、認証の成功/失敗ステータスをアプリケーションに通知し、必要に応じて失敗理由に関する追加情報も提供します。

このコールバックは、ログアウトフローが完了したときにも通知します。

| **コールバック：認証フローのステータスを報告** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**可用性：** v1.0以降

**パラメーター：**

- *status*：次のいずれかの値を取ることができます：
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` – 認証フローが正常に完了しました
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` – 認証フローに失敗しました
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - ログアウト
- *code*：表示されたステータスの理由。 *status*&#x200B;が`AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`の場合、*code*&#x200B;は空の文字列です（つまり、`AccessEnabler.USER_AUTHENTICATED`定数で定義されています）。 認証されていない場合、このパラメーターは次のいずれかの値を取ることができます。
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - ユーザーが認証されていません。 ローカル トークン キャッシュに有効な認証トークンがない場合の&#x200B;*checkAuthentication （）* メソッド呼び出しに対する応答。
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` – 上層アプリケーションが&#x200B;*null*&#x200B;を`setSelectedProvider()`に渡して認証フローを中止した後、AccessEnablerは認証状態マシンをリセットしました。  おそらく、ユーザーは認証フローをキャンセルしました（「戻る」ボタンを押しました）。
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - ネットワークが利用できないなどの理由で認証フローが失敗したか、ユーザーが認証フローを明示的にキャンセルしました。
   - `AccessEnabler.LOGOUT` - ログアウトアクションが原因でユーザーが認証されていません。

**トリガー：** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorizedResources {#checkPreauth}

**説明：**&#x200B;このメソッドは、ユーザーが特定の保護されたリソースを表示する権限を既に持っているかどうかを判断するために、アプリケーションで使用されます。 この方法の主な目的は、UIの装飾に使用する情報を取得することです（例えば、ロックアイコンとロック解除アイコンを使用してアクセスステータスを示す）。

| **API呼び出し：現在選択されているプロバイダーを設定** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**可用性：** v1.0以降

**&lt; パラメーター：** `resources` パラメーターは、認証を確認する必要があるリソースの配列です。 リスト内の各要素は、リソース IDを表す文字列である必要があります。 リソース IDは、`getAuthorization()`呼び出しのリソース IDと同じ制限の対象となります。つまり、プログラマーとMVPDまたはMedia RSS フラグメントの間で設定された合意値である必要があります。

**コールバックがトリガーされました：** `preauthorizedResources()`

</br>

### preauthorizedResources {#preauthResources}

**説明：** checkPreauthorizedResources （）によってトリガーされたコールバック。 ユーザーが既に表示を許可されているリソースのリストを提供します。

| **API呼び出し：現在選択されているプロバイダーを設定** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**&#x200B; 可用性：**&#x200B;v 1.0以降

**パラメーター：** `resources` パラメーターは、ユーザーが既に表示を許可されているリソースの配列です。

**トリガー：** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**説明：**&#x200B;このメソッドは、アプリケーションが認証ステータスを確認するために使用します。 まず、認証ステータスを確認します。 認証されていない場合、*setTokenRequestFailed （）* コールバックがトリガーされ、メソッドが終了します。 ユーザーが認証された場合は、認証フローもトリガーします。 *getAuthorization （）* メソッドの詳細を参照してください。

| **API呼び出し：認証ステータスを確認** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**可用性：** v1.0以降

| **API呼び出し：認証ステータスを確認** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**可用性：** v1.0以降

**パラメーター：**

- *resourceId*: ユーザーが認証を要求するリソースのID。
- *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるマップ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。

**コールバックがトリガーされました：** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**説明：**&#x200B;このメソッドは、アプリケーションが認証フローを開始するために使用します。 ユーザーがまだ認証されていない場合は、認証フローも開始します。 ユーザーが認証を受けた場合、Access Enablerは、認証トークン（ローカルトークンキャッシュに有効な認証トークンがない場合）および短期間有効なメディアトークンのリクエストを発行します。 ショートメディアトークンが取得されると、認証フローは完了したものとみなされます。 *setToken （）* コールバックがトリガーされ、ショートメディアトークンがパラメーターとしてアプリケーションに配信されます。 何らかの理由で認証が失敗した場合、*tokenRequestFailed （）* コールバックがトリガーされ、エラーコードと詳細が提供されます。

| **API呼び出し：認証フローを開始** |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**可用性：** v1.0以降

| **API呼び出し：認証フローを開始** |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**可用性：** v1.0以降

**パラメーター：**

- *resourceId*: ユーザーが認証を要求するリソースのID。
- *data*：有料テレビのパスサービスに送信するキーと値のペアで構成されるマップ。 Adobeでは、このデータを利用して、SDKに変更を加えることなく、将来の機能を有効にすることができます。

**コールバックがトリガーされました：** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **追加のコールバックがトリガーされました** <br>このメソッドは、次のコールバックもトリガーできます（フローも開始されている場合）: _setAuthenticationStatus （）_、_displayProviderDialog （）_ |

**注意：可能な限り、getAuthorization （）の代わりにcheckAuthorization （）を使用してください。 getAuthorization （） メソッドは、完全な認証フローを開始します（ユーザーが認証されていない場合）。これにより、プログラマー側で複雑な実装が行われる可能性があります。**

</br>

### setToken {#setToken}

**説明：**&#x200B;認証フローが正常に完了したことをアプリケーションに通知するAccess Enablerによってトリガーされるコールバック。 短期間有効なメディアトークンもパラメーターとして配信されます。

| **コールバック：承認フローが正常に完了しました** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**&#x200B; 可用性：**&#x200B;v 1.0以降

**パラメーター：**

- *トークン*：短期間有効なメディアトークン
- *resourceId*：認証を取得したリソース

**トリガー：** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**説明：** アクセス イネーブラによってトリガーされたコールバックは、認証フローが失敗したことを上位アプリケーションに通知します。

| **コールバック：認証フローが失敗しました** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**可用性：** v1.0以降

**パラメーター：**

- *resourceId*：認証を取得したリソース
- *errorCode*：失敗シナリオに関連付けられたエラーコード。 使用可能な値：
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - ユーザーは指定されたリソースを承認できませんでした
- *errorDescription*：失敗シナリオに関する追加の詳細。 この記述文字列が何らかの理由で使用できない場合、Adobe Pass Authenticationは空の文字列>**（&quot;）**&#x200B;を送信します。  この文字列は、MVPDでカスタムエラーメッセージまたはセールス関連メッセージを渡すために使用できます。 例えば、サブスクライバーがリソースの認証を拒否された場合、MVPDは次のようなメッセージを送信できます。「現在、パッケージ内のこのチャネルにアクセスできません。 パッケージをアップグレードする場合は、ここをクリックしてください。」 メッセージは、このコールバックを通じてAdobe Pass認証によってプログラマーに渡されます。プログラマーは、メッセージを表示または無視するオプションを持っています。 Adobe Pass認証では、このパラメーターを使用して、エラーの原因となった可能性のある条件を通知することもできます。 例えば、「プロバイダーの認証サービスと通信する際にネットワークエラーが発生しました。」

**トリガー：** `checkAuthorization(), getAuthorization()`

</br>

### ログアウト {#logout}

**説明：**&#x200B;このメソッドを使用して、ログアウトフローを開始します。 ログアウトは、ユーザーがAdobe Pass Authentication サーバーとMVPDのサーバーの両方からログアウトする必要があるため、一連のHTTP リダイレクト操作の結果です。

| **API呼び出し：ログアウトフローを開始** |
| --- |
| ```public void logout()``` |

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：**&#x200B;なし

</br>

### getSelectedProvider {#getSelectedProvider}

**説明：**&#x200B;このメソッドを使用して、現在選択されているプロバイダーを決定します。

| **API呼び出し：現在選択されているMVPDを決定** |
| --- |
| ```public void getSelectedProvider()``` |

**可用性：** v1.0以降

**パラメーター：**&#x200B;なし

**コールバックがトリガーされました：** `selectedProvider()`

</br>

### selectedProvider {#selectedProvider}

**説明：**&#x200B;現在選択されているMVPDに関する情報をアプリケーションに配信するAccess Enablerによってトリガーされるコールバック。

| **コールバック：現在選択されているMVPD**&#x200B;に関する情報 |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**可用性：** v1.0以降

**パラメーター：**

- *mvpd*：現在選択されているMVPDに関する情報を含むオブジェクト

**トリガー：** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**説明：**&#x200B;このメソッドを使用して、Access Enabler ライブラリによってメタデータとして公開された情報を取得します。 アプリケーションは、複合MetadataKey オブジェクトを指定することで、この情報にアクセスできます。

| **API呼び出し：メタデータのAccessEnablerのクエリ** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**可用性：** v1.0以降

プログラマーが利用できるメタデータには、次の2種類があります。

- 静的メタデータ（認証トークン TTL、認証トークン TTLおよびデバイス ID）
- ユーザーメタデータ（ユーザーIDや郵便番号など、ユーザー固有の情報。認証フローや認証フロー中にMVPDからユーザーのデバイスに渡されます）

**パラメーター：**

- *metadataKey*: キーと引数の変数をカプセル化するデータ構造で、次の意味を持ちます。
   - キーが`METADATA_KEY_TTL_AUTHN`の場合、認証トークンの有効期限を取得するためにクエリが実行されます。
   - キーが`METADATA_KEY_TTL_AUTHZ`で、引数に名前= `METADATA_ARG_RESOURCE_ID`および値= `[resource_id]`のSerializableNameValuePair オブジェクトが含まれている場合、クエリを実行して、指定したリソースに関連付けられている認証トークンの有効期限を取得します。
   - キーが`METADATA_KEY_DEVICE_ID`の場合、現在のデバイス IDを取得するためにクエリが実行されます。 この機能はデフォルトで無効になっており、プログラマーは有効化と料金についてAdobeに問い合わせる必要があります。
   - キーが`METADATA_KEY_USER_META`で、引数に名前= `METADATA_KEY_USER_META`および値= `[metadata_name]`のSerializableNameValuePair オブジェクトが含まれている場合、クエリはユーザーメタデータに対して実行されます。 使用可能なユーザーメタデータタイプの現在のリスト：
      - `zip` – 郵便番号
      - `householdID` – 世帯ID。 MVPDがサブアカウントをサポートしていない場合、これは`userID`と同じです。
      - `maxRating` - ユーザーの最大保護者の評価
      - `userID` - ユーザーID。 MVPDがサブアカウントをサポートしており、ユーザーがメインアカウントではない場合，
      - `channelID` - ユーザーが表示する権限を持つチャネルのリスト

プログラマが実際に使用できるユーザーメタデータは、MVPDで使用可能なメタデータによって異なります。  このリストは、新しいメタデータが利用可能になり、Adobe Pass認証システムに追加されるにつれて、さらに拡張されます。

**コールバックがトリガーされました：** [`setMetadataStatus()`](#setMetadaStatus)

**詳細情報：** [&#x200B; ユーザーメタデータ &#x200B;](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**説明：** *getMetadata （）*&#x200B;呼び出しを介して要求されたメタデータを配信するAccess Enablerによってトリガーされるコールバック。

| **コールバック：メタデータ取得要求の結果** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**可用性：** v1.0以降

**パラメーター：**

- *key*: メタデータ値が要求されるキーと関連するパラメーターを含むMetadataKey オブジェクト（参照実装については、デモアプリケーションを参照）。
- *結果*：要求されたメタデータを含む複合オブジェクト。 オブジェクトには次のフィールドがあります。
   - *simpleResult*：認証TTL、認証TTL、またはデバイス IDに対してリクエストが行われたときのメタデータ値を表す文字列。 この値は、ユーザーメタデータに対してリクエストが行われた場合はnullです。

   - *userMetadataResult*: JSON ユーザーメタデータペイロードのJava表現を含むオブジェクト。 例：

     ```json
     {
     "street": "Main Avenue",
     "buildings": ["150", "320"]
     }
     ```

     は次のようにJavaに翻訳されます。

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


この値は、単純なメタデータ（認証TTL、認証TTL、デバイス ID）に対してリクエストが行われた場合はnullです。

- *encrypted*：取得したメタデータが暗号化されているかどうかを指定するブール値。 このパラメーターは、ユーザーメタデータ要求に対してのみ有効であり、常に暗号化せずに受信される静的メタデータ（認証TTLなど）に対しては意味がありません。 このパラメーターがTrueに設定されている場合、ホワイトリストに登録されている秘密鍵（呼び出し[`setRequestor`](#setRequestor)で依頼者IDの署名に使用されるのと同じ秘密鍵）を使用してRSA復号化を実行して、暗号化されていないユーザーメタデータ値を取得するのはプログラマーの役割です。

**トリガー：** [`getMetadata()`](#getMetadata)

**詳細情報：** [&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

</br>

## getVersion {#getVersion}

**説明：**&#x200B;このメソッドを使用して、AccessEnablerの現在のバージョンを取得します

| **API呼び出し：AccessEnabler バージョンを取得** |
| --- |
| ```public static String getVersion()``` |

## トラッキングイベント {#tracking}

Access Enablerは、必ずしも使用権限フローに関連しない追加のコールバックをトリガーします。 *sendTrackingData （）*&#x200B;という名前のイベントトラッキングコールバック関数の実装はオプションですが、特定のイベントを追跡し、認証/承認の試行回数などの統計情報をコンパイルすることができます。 以下は、*sendTrackingData （）* コールバックの仕様です。

### sendTrackingData {#sendTrackingData}

**説明：**&#x200B;認証/認証フローの完了/失敗など、様々なイベントが発生したことをアプリケーションに通知するAccess Enablerによってトリガーされるコールバック。 デバイスの種類、Access Enabler クライアントの種類、およびオペレーティング システムもsendTrackingData （）によって報告されます。

>[!WARNING]
>
> デバイスタイプとオペレーティングシステムは、パブリック Java ライブラリ（http://java.net/projects/user-agent-utils）とユーザーエージェント文字列を使用して取得されます。 この情報は、操作指標をデバイスカテゴリーに分類する大まかな方法としてのみ提供されますが、Adobeは誤った結果に対して責任を負うことはできません。 それに応じて新しい機能を使用してください。

- デバイスタイプの可能な値：
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`

- Access Enabler クライアントの種類に指定できる値：
   - `flash`
   - `html5`
   - `ios`
   - `tvos`
   - `android`
   - `firetv`

| コールバック：トラッキングイベント |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**可用性：** v1.0以降

**パラメーター：**

- *event*：追跡されているイベント。 トラッキングイベントには、次の3つのタイプがあります。
   - **authorizationDetection:**&#x200B;認証トークン要求が返されるたびに（イベントタイプは`EVENT_AUTHZ_DETECTION`）
   - 認証チェックが発生するたびに&#x200B;**authenticationDetection:** （イベントタイプは`EVENT_AUTHN_DETECTION`）です
   - **mvpdSelection:**：ユーザーがMVPDの選択フォームでMVPDを選択した場合（イベントタイプは`EVENT_MVPD_SELECTION`）
- *data*：報告されたイベントに関連付けられている追加データ。 このデータは、値のリストの形式で表示されます。

*data*&#x200B;配列の値を解釈する手順を次に示します。

- イベントタイプ *`EVENT_AUTHN_DETECTION`:*&#x200B;の場合
   - **0** - トークン要求が成功したかどうか（true/false）、上記がtrueの場合：
   - **1** - MVPD ID文字列
   - **2** - GUID （md5 ハッシュ）
   - **3** - トークンは既にキャッシュ内にあります（true/false）
   - **4** - デバイスの種類
   - **5** - Access Enabler クライアント タイプ
   - **6** - オペレーティング システムの種類

- イベントタイプ `EVENT_AUTHZ_DETECTION`の
   - **0** - トークン要求が成功したかどうか（true/false）、成功した場合：
   - **1** - MVPD ID
   - **2** - GUID （md5 ハッシュ）
   - **3** - トークンは既にキャッシュ内にあります（true/false）
   - **4** - エラー
   - **5** – 詳細
   - **6** - デバイスの種類
   - **7** - Access Enabler クライアント タイプ
   - **8** - オペレーティング システムの種類

- イベントタイプ `EVENT_MVPD_SELECTION`の
   - **0** – 現在選択されているMVPDのID
   - **1** - デバイスの種類
   - **2** - Access Enabler クライアント タイプ
   - **3** - オペレーティング システムの種類

**トリガー：** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
