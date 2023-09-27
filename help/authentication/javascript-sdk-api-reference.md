---
title: JavaScript SDK API リファレンス
description: JavaScript SDK API リファレンス
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 4eb5fc1eb1eea4c5e27bbee298db4b9b4ba2daef
workflow-type: tm+mt
source-wordcount: '2836'
ht-degree: 0%

---

# JavaScript SDK API リファレンス {#javascript-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## API リファレンス {#api-reference}

これらの関数は、MVPD とのやり取りのリクエストを開始します。 すべての呼び出しは非同期です。実装する必要があります。 [コールバック](#callbacks) 応答を処理するには、次の手順に従います。

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, endpoints, options){#setrequestor(inRequestorID,endpoints,options)}

**説明：** リクエストの送信元のサイトを識別します。  通信セッション内の他の API 呼び出しの前に、この呼び出しをおこなう必要があります。

**パラメーター：**

- *inRequestorID*  — 登録時に元のサイトにAdobeが割り当てた一意の識別子。

- *endpoints*  — このパラメーターはオプションです。 次のいずれかの値を指定できます。

   - Adobeが提供する認証および認証サービスのエンドポイントを指定できる配列（デバッグ目的で異なるインスタンスが使用される場合があります）。 複数の URL が提供される場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。 デフォルト（値が指定されていない場合）では、Adobe サービスプロバイダーが使用されます (<http://sp.auth.adobe.com/>) をクリックします。

  例：
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options*  — アプリケーション ID 値、訪問者 ID 値の更新レス設定（バックグラウンドログアウト）および MVPD 設定 (iFrame) を含む JSON オブジェクト。 すべての値はオプションです。
   1. 指定した場合、Experience CloudvisitorID は、ライブラリが実行するすべてのネットワーク呼び出しでレポートされます。 この値は、後で高度な分析レポートに使用できます。
   2. アプリケーションの一意の ID が指定されている場合 —`applicationId`  — 値は、X-Device-Info HTTP ヘッダーの一部として、アプリケーションによっておこなわれる後続のすべての呼び出しに追加されます。 この値は、後でから取得できます。 [ESM](/help/authentication/entitlement-service-monitoring-overview.md) 適切なクエリを使用してレポートを作成する。

  **注意：** すべての JSON キーでは大文字と小文字が区別されます。

  例：

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- プログラマーは、iFrame がログインに必要かどうかを指定する (*iFrameRequired* キー ) と iFrame のサイズ (*iFrameWidth* および *iFrameHeight* キー )。 JSON オブジェクトには次のテンプレートがあります。

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


上記のテンプレートの最上位キーはすべてオプションで、デフォルト値 (*backgroundLogin*, *backgroundLogut*&#x200B;はデフォルトでは false で、mvpdConfig は null です。これは、MVPD 設定が上書きされないことを意味します )。


- **注意**：上記のパラメーターに無効な値またはタイプを指定すると、未定義の動作が発生します。



次のシナリオの設定例を示します。リフレッシュレスログインとログアウトを有効にし、MVPD1 をフルページリダイレクトログイン（非 iFrame）に、MVPD2 を width=500、height=300 の iFrame ログインに変更します。

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**コールバックがトリガーされました：** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[トップに戻る](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**説明：** 指定したリソースの認証を要求します。 お客様が認証可能なリソースにアクセスしようとするたびに、この関数を呼び出して、Access Enabler から短時間有効な認証トークンを取得します。 リソース ID は、認証を提供する MVPD と合意されています。

現在の顧客に対して、キャッシュされた認証トークンを使用します。 そのようなトークンが見つからない場合、は最初に認証プロセスを開始し、認証を続行します。

**パラメーター：**

- `inResourceID`  — ユーザーが認証をリクエストするリソースの ID。
- `redirect_url`  — オプションでリダイレクト URL を指定し、MVPD の認証プロセスが、認証が開始されたページではなく、そのページにユーザーを返すようにします。


**コールバックがトリガーされました：** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) 成功して [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) 失敗時

>[!CAUTION]
>
>可能な限り、getAuthorization() の代わりに checkAuthorization() を使用してください。 getAuthorization() メソッドは、完全な認証フローを開始し（ユーザが認証されていない場合）、プログラマ側で複雑な実装を引き起こす可能性があります。

</b>

[トップに戻る](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**説明：** 現在の顧客の認証をリクエストします。 通常、「ログイン」ボタンのクリックに応じて呼び出されます。 現在の顧客のキャッシュされた認証トークンを確認します。 そのようなトークンが見つからない場合、は認証プロセスを開始します。 これにより、既定またはカスタムのプロバイダ選択ダイアログが呼び出され、選択したプロバイダを使用して MVPD のログインインターフェイスにリダイレクトされます。

が成功すると、はユーザーの認証トークンを作成して保存します。 認証に失敗した場合、プロバイダーは適切なエラーメッセージを [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode) コールバック。

**パラメーター：**

- redirect_url — オプションでリダイレクト URL を指定します。これにより、MVPD の認証プロセスは、認証が開始されたページではなく、そのページにユーザーを返します。

**コールバックがトリガーされました：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[トップに戻る](#top)

</br>

## checkAuthN {#checkauthn}

**説明：** 現在の顧客の現在の認証状態を確認します。  どの UI にも関連付けられていません。

**コールバックがトリガーされました：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[トップに戻る](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**説明：** このメソッドは、現在の顧客と特定のリソースの認証ステータスを確認するために、アプリケーションで使用されます。 まず、認証状態を最初に確認します。 認証されない場合、tokenRequestFailed() コールバックがトリガーされ、メソッドが終了します。 ユーザーが認証されると、認証フローもトリガーされます。 詳細は、 [getAuthorization()](#getAuthZメソッド )。

>[!TIP]
>
> **check-status 関数の使用**  認証をリクエストする前に、認証または認証のステータスを確認する必要はありません。 これらの関数を呼び出して、例えば、独自のステータス表示を更新できます。 ユーザーの操作が必要な場合は、これらを使用しないでください。

**パラメーター：**

- `inResourceID`  — ユーザーが認証をリクエストするリソースの ID。


**コールバックがトリガーされました：**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**説明：** リソースのリストに対して「プリフライト」認証ステータスをリクエストします。

**パラメーター：**

- *リソース*: resources パラメーターは、承認が確認される必要があるリソースの配列です。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、 `getAuthorization()` 呼び出し、すなわち、プログラマと MVPD の間に確立された値、またはメディア RSS フラグメントに基づく合意済みの値です。

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

この API バリアントは、JS SDK バージョン 4.0 以降で使用できます。


**パラメーター：**

- *リソース*: resources パラメーターは、承認が確認される必要があるリソースの配列です。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、 `getAuthorization()` 呼び出し、すなわち、プログラマと MVPD の間に確立された値、またはメディア RSS フラグメントに基づく合意済みの値です。

- *キャッシュ*：事前に認証されたリソースを確認する際に内部キャッシュを使用するかどうか。 これはオプションのパラメーターで、デフォルト値はです。 **true**. true の場合、動作は上記の API と同じです。つまり、この関数の後続の呼び出しでは、内部キャッシュを使用して、事前に認証されたリソースを解決します。 渡す **false** のパラメーターは内部キャッシュを無効にし、その結果、 **checkPreauthorizedResources** API が呼び出されます。

**コールバックがトリガーされました：** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[トップに戻る](#top)
</br>

## getMetadata(Key) {#getMetadata}

**説明：** Access Enabler ライブラリによってメタデータとして公開された情報を取得します。

メタデータには次の 2 つのタイプがあります。

- **静的** （認証トークン TTL、認証トークン TTL、およびデバイス ID）
- **ユーザーメタデータ** （これには、認証フローまたは認証フローの間に MVPD からユーザーのデバイスに渡されるユーザー固有の情報が含まれます）。

**詳細情報：** [ユーザーメタデータ](#UserMetadata)

**パラメーター：**

- *key*：要求されたメタデータを指定する ID。
   - キーが `"TTL_AUTHN",` 次に、認証トークンの有効期限を取得するためにクエリが実行されます。

   - キーが `"TTL_AUTHZ"` params は、リソース id を文字列として含む配列で、クエリが実行され、指定したリソースに関連付けられた認証トークンの有効期限が取得されます。

   - キーが `"DEVICEID"` 次に、現在のデバイス id を取得するためのクエリが実行されます。 この機能はデフォルトで無効になっています。有効化と料金については、Adobeにお問い合わせください。

   - キーが次のユーザーメタデータタイプのリストに含まれている場合、対応するユーザーメタデータを含む JSON オブジェクトがに送信されます。 [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) コールバック関数：

   - `"zip"`  — 郵便番号

   - `"encryptedZip"`  — 暗号化された郵便番号

   - `"householdID"`  — 世帯識別子。 MVPD がサブアカウントをサポートしない場合、これは userID と同じになります。

   - `"maxRating"`  — ユーザーの親の最大評価

   - `"userID"`  — ユーザー識別子。 MVPD がサブアカウントをサポートし、ユーザーがメインアカウントではない場合、userID は householdID とは異なります。

   - `"channelID"`  — ユーザーが表示する権限のあるチャネルのリスト

   - `"is_hoh"`  — ユーザーが世帯のトップかどうかを識別するフラグ。

   - `"encryptedZip"`  — 暗号化された郵便番号

   - `"typeID"`  — ユーザーアカウントがプライマリ/セカンダリアカウントであるかどうかを識別するフラグ。

   - `"primaryOID"`  — 世帯識別子

   - `"postalCode"`  — 郵便番号と類似

   - `"acctID"`  — アカウント ID

   - `"acctParentID"`  — アカウントの親 ID

  **注意**：プログラマーが使用できる実際のユーザーメタデータは、MVPD が使用可能にするものによって異なります。  詳しくは、 [ユーザーメタデータ](#UserMetadata) （使用可能なユーザメタデータの現在のリスト）


例：

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**コールバックがトリガーされました：** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[トップに戻る](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**説明：** プロバイダ選択 UI から MVPD を選択し、プロバイダ選択 UI にプロバイダ選択を送信した場合、またはユーザがプロバイダ選択 UI を選択せずに閉じた場合は null パラメータでこの関数を呼び出す場合は、この関数を呼び出します。

**コールバックがトリガーされました：**[ setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[トップに戻る](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**説明：** プロバイダーの選択ダイアログで顧客の選択の結果を取得します。 これは、最初の認証チェックの後、いつでも使用できます。

この関数は非同期的で、結果を `selectedProvider()` コールバック関数。

- **MVPD** 現在選択されている MVPD。MVPD が選択されていない場合は null。
- **AE_State** 現在の顧客の認証の結果（「新規ユーザー」、「未認証ユーザー」または「認証ユーザー」）

**コールバックがトリガーされました：** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[トップに戻る](#top)

</br>

## ログアウト {#logout}

**説明：** 現在の顧客をログアウトし、そのユーザーのすべての認証および認証情報を消去します。 顧客のシステムからすべての authN および authZ トークンを削除します。

**コールバックがトリガーされました：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[トップに戻る](#top)

</br>

## コールバックの定義 {#calllback-definitions}

非同期リクエスト呼び出しへの応答を処理するには、次のコールバックを実装する必要があります。

- [entitlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded() {#entitlementLoaded}

**説明：** Access Enabler の初期化が完了し、要求を受け取る準備が整ったときにトリガーされます。 このコールバックを実装して、Access Enabler API との通信をいつ開始できるかを知る。
</br>

[トップに戻る](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**説明：** このコールバックを実装して、設定情報と MVPD リストを受け取ります。

**パラメーター：**

- *configXML*:MVPD リストを含む、現在の REQUESTOR の設定を保持している xml オブジェクト。


**トリガー元：** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[トップに戻る](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**説明：** このコールバックを実装して、独自のカスタムプロバイダー選択 UI を呼び出します。 お客様が選択できるダイアログでは、表示名（およびオプションのロゴ）を使用する必要があります。 顧客が選択を行い、ダイアログを閉じたら、選択したプロバイダーに関連する ID を、への呼び出しで送信します。 *setSelectedProvider()*.

**パラメーター：**

- *プロバイダー*  — 要求された MVPD を表すオブジェクトの配列。

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**トリガー元：** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[トップに戻る](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**説明：** ユーザーが認証ログインページ UI を表示する iFrame を必要とする MVPD を選択した場合に、このコールバックを実装します。

**トリガー元：**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [トップに戻る](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**説明：** このコールバックを実装して、認証状態（1=認証済みまたは 0=未認証）と、認証状態の判断中にエラーが発生した場合に説明的なエラーメッセージを受け取ります（チェックが正常に完了した場合は空の文字列）。

>[!NOTE]
> 
>現在の [エラー報告の進行](/help/authentication/error-reporting.md) システムでは、この関数に送信される errorCode パラメーターを無視できます。  ただし、 isAuthenticated フラグは、エンタイトルメントフローでユーザーの認証状態を追跡するために引き続き使用できます


**パラメーター：**

- *isAuthenticated*  — 認証ステータスが 1（認証済み）または 0（未認証）になります。
- *errorCode*  — 認証ステータスの判別時に発生したエラー。 ない場合は空の文字列。


**トリガー元：** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[トップに戻る](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>デバイスタイプとオペレーティングシステムは、パブリック Java ライブラリ (<http://java.net/projects/user-agent-utils>) およびユーザーエージェント文字列。 この情報は、運用指標をAdobeのカテゴリに分類する簡単な方法としてのみ提供されますが、誤った結果に対する責任はに与えられません。 適宜、新しい機能を使用してください。

**説明：** 特定のイベントが発生したときにトラッキングデータを受け取るには、このコールバックを実装します。 例えば、この機能を使用して、同じ資格情報でログインしたユーザー数を追跡できます。 トラッキングは現在設定できません。 Adobe Pass Authentication 1.6 の場合、 `sendTrackingData()` また、デバイス、Access Enabler クライアント、およびオペレーティング・システム・タイプに関する情報も報告します。 The `sendTrackingData()` コールバックは後方互換性を維持します。

- デバイスタイプに指定できる値：
   - コンピュータ
   - タブレット
   - モバイル
   - ガメコンソール
   - 不明

- Access Enabler のクライアント・タイプに指定可能な値：
   - html5
   - ios
   - android


イベントタイプと関連する情報の配列を渡します。 イベントタイプは次のとおりです。

| mvpdSelection | ユーザは、プロバイダ選択ダイアログで MVPD を選択しました。 |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | 認証チェックが完了しました。 |
| authorizationDetection | 認証リクエストが完了しました。 |

</br>
データは、各イベントタイプに固有です。
</br>

| イベントタイプ（文字列） | データ（配列） |
|:--- | :--- |
| mvpdSelection | 0：選択した MVPD |
|  | 1：デバイスタイプ |
|  | 2:Access Enabler クライアント・タイプ |
|  | 3:OS |
| authenticationDetection | 0：トークンリクエストが成功したかどうか (true/false) |
|  | 1: MVPD ID |
|  | 2:GUID |
|  | 3：トークンは既にキャッシュに存在します (true/false) |
|  | 4：デバイスタイプ |
|  | 5:Access Enabler のクライアント・タイプ |
|  | 6:OS |
| authorizationDetection | 0：トークンリクエストが成功したかどうか (true/false) |
|  | 1: MVPD ID |
|  | 2:GUID |
|  | 3：トークンは既にキャッシュに存在します (true/false) |
|  | 4：エラー |
|  | 5：詳細 |
|  | 6：デバイスタイプ |
|  | 7:Access Enabler クライアント・タイプ |
|  | 8:OS |


**トリガー元：** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[トップに戻る](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**説明：** このコールバックを実装して、短時間のみ有効なメディアトークン (inToken) と、承認リクエストまたは確認認証リクエストが行われ、正常に完了したリソースの ID(inRequestedResourceID) を受け取ります。

**トリガー元：** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[トップに戻る](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**説明：** 承認または確認認証要求が失敗した場合に通知するこのコールバックを実装します。 オプションで、MVPD がプログラマーが表示するカスタムメッセージを提供するために使用できます。

>[!IMPORTANT]
>
>このコールバック関数は、元のAdobe Pass Authentication エラーレポートシステムに含まれています。 後方互換性のために保持されますが、現在の高度なエラーレポートシステムを使用して独自のコールバックを実装した場合は、この関数をまったく使用する必要はありません。 新しいエラー報告システムでは、承認（または他の操作）が失敗した理由に関する詳細情報と、エラーや警告のタイプ別に推奨されるアクションコースが提供されます。

**パラメーター：**

- *inRequestedResourceID*  — 認証リクエストで使用されたリソース ID を提供する文字列。
- *inRequestErrorCode*  — 失敗の理由を示す、Adobe Pass Authentication エラーコードを表示する文字列。指定可能な値は、「User Not Authenticated Error」および「User Not Authorized Error」です。詳しくは、以下の「Callback error codes」を参照してください。
- *inRequestDetailedErrorMessage*  — 表示に適した追加の説明文字列。 何らかの理由でこの説明文字列を使用できない場合、Adobe Pass Authentication は空の文字列を送信します **(&quot;&quot;)**.  これは、MVPD がカスタムエラーメッセージや販売関連メッセージを渡すために使用できます。 例えば、あるリソースに対する承認がサブスクライバーによって拒否された場合、MVPD は `*inRequestDetailedErrorMessage*` 例： **「現在、パッケージ内のこのチャネルにアクセスできません。 パッケージをアップグレードする場合は、\*こちら\*をクリックしてください。** メッセージは、このコールバックを通じてAdobe Pass Authentication によってプログラマーのサイトに渡されます。 プログラマーは、それを表示または無視するオプションを持ちます。 Adobe Pass認証では、 `*inRequestDetailedErrorMessage*` エラーを引き起こした可能性のある条件をプログラマーに通知する。 例： **「プロバイダーの認証サービスとの通信中にネットワークエラーが発生しました」。**



**トリガー元：**  [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[トップに戻る](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**説明：** に対する呼び出し後に返される承認済みリソースリストを提供する Access Enabler によってトリガーされるコールバック `checkPreauthorizedResources()`.

**パラメーター：**

- *authorizedResources*：認証済みリソースのリスト。

**トリガー元：** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[トップに戻る](#top)

</br>

## setMetadataStatus(key, encrypted, data) {#setMetadataStatus(key,encrypted,data)}

**説明：** Access Enabler によってトリガーされるコールバック。 `getMetadata()` を呼び出します。

**詳細情報：** [ユーザーメタデータ](#userMetadata)

**パラメーター：**

- *キー（文字列）*：リクエストがおこなわれたメタデータのキー。
- *encrypted (Boolean)*:「値」が暗号化されるかどうかを示すフラグ。 これが「true」の場合、「value」は、実際には実際の値の JSON Web 暗号化された表現になります。
- *データ（JSON オブジェクト）*：メタデータを表す JSON オブジェクト。単純なリクエストの場合 (「`TTL_AUTHN`&#39;、&#39;`TTL_AUTHZ`&#39;、&#39;`DEVICEID`&#39;) の場合、結果は文字列（認証 TTL、認証 TTL、またはデバイス ID を表します）になります。 ユーザーメタデータリクエストの場合、結果は、メタデータペイロードを表すプリミティブまたは JSON オブジェクトにすることができます。 JSON ユーザーメタデータオブジェクトの実際の構造は次のようになります。

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


例：

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**トリガー元：** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[トップに戻る](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**説明：** このコールバックを実装して、現在選択されている MVPD と、 `result` パラメーター。 The `result` パラメーターは、次のプロパティを持つ Object です。

- **MVPD** 現在選択されている MVPD。MVPD が選択されていない場合は null。
- **AE\_State** 現在のユーザーの認証の結果（「新規ユーザー」、「未認証ユーザー」、「認証ユーザー」のいずれか）

**トリガー元：** [getSelectedProvider()](#getSelProv)

</br>

[トップに戻る](#top)

</br>

### コールバックのエラーコード {#callback-error-codes}

| 一般的なエラー | |
|:--- | :--- | 
| 内部エラー | 要求を処理しようとした際にシステムエラーが発生しました。 |
| プロバイダーが選択されていませんエラー | 顧客がプロバイダー選択ダイアログでキャンセルする際に発生します |
| プロバイダを利用できませんエラー | 使用可能なプロバイダーがない場合に発生します。 |

| 認証エラー | |
|:--- | :--- | 
| 汎用認証エラー | 理由が不明な場合や公開できない場合に返されます。 |
| 内部認証エラー | 認証を試みた際にシステムエラーが発生しました。 |
| ユーザー未認証エラー | ユーザーが認証されていません。 |
| 複数の認証リクエストエラー | 最初の認証リクエストが完了する前に、追加の認証リクエストが受信されました。 |

| 認証エラー | |
|:--- | :--- | 
| 汎用認証エラー | 理由が不明な場合や公開できない場合に返されます。 |
| 内部認証エラー | 認証を試みた際にシステムエラーが発生しました。 |
| ユーザーが認証されていませんエラー | お客様は、リクエストされたコンテンツの表示を許可されていません。 |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[トップに戻る](#top)
