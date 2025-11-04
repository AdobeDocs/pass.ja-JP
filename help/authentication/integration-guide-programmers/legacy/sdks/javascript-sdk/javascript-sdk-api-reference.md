---
title: JavaScript SDK API リファレンス
description: JavaScript SDK API リファレンス
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '2883'
ht-degree: 0%

---

# （従来の）JavaScript SDK API リファレンス {#javascript-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## API リファレンス {#api-reference}

これらの関数は、MVPDとのインタラクションリクエストを開始します。 すべての呼び出しは非同期です。応答を処理するには [callbacks](#callbacks) を実装する必要があります。

- [setRequestor （）](#setReq)
- [getAuthorization （）](#getAuthZ)
- [getAuthentication （）](#getAuthN)
- [checkAuthentication （）](#checkAuthN)
- [checkAuthorization （）](#checkAuthZ)
- [checkPreauthorizedResources （）](#checkPreauthRes)
- [getMetadata （）](#getMeta)
- [setSelectedProvider （）](#setSelProv)
- [logout （）](#logout)


## setRequestor （inRequestorID、エンドポイント、オプション）{#setrequestor(inRequestorID,endpoints,options)}

**説明：** リクエストの発信元のサイトを識別します。  通信セッションの他の API 呼び出しより前に、この呼び出しを行う必要があります。

**パラメーター：**

- *inRequestorID* - Adobeが登録時に元のサイトに割り当てた一意の ID。

- *endpoints* – このパラメーターはオプションです。 次のいずれかの値を指定できます。

   - Adobeが提供する認証および承認サービスのエンドポイントを指定できる配列（デバッグ目的で別のインスタンスが使用される場合があります）。 複数の URL が指定されている場合、MVPDリストはすべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー（最初に応答し、そのMVPDをサポートするプロバイダー）に関連付けられます。 デフォルトでは（値を指定しない場合）、Adobe サービスプロバイダーが使用されます（<http://sp.auth.adobe.com/>）。

  例：
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - アプリケーション ID 値、訪問者 ID 値の更新なしの設定（バックグラウンドログインログアウト）およびMVPD設定（iFrame）を含む JSON オブジェクト。 値はすべてオプションです。
   1. これを指定すると、ライブラリで実行されるすべてのネットワーク呼び出しでExperience Cloudの visitorID がレポートされます。 この値は、後で高度な分析レポートに使用できます。
   2. アプリケーションの一意の識別子が – `applicationId` に指定されている場合、この値は、X-Device-Info HTTP ヘッダーの一部としてアプリケーションによって行われる後続のすべての呼び出しに追加されます。 この値は、後で適切なクエリを使用して [ESM](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md) レポートから取得できます。

  **メモ：** すべての JSON キーでは大文字と小文字が区別されます。

  例：

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- プログラマーは、ログインに iFrame が必要かどうかを指定し（*iFrameRequired* キー）、iFrame のサイズ（*iFrameWidth* キーと *iFrameHeight* キー）を指定することで、Adobe Pass Authentication で設定されたMVPD設定を上書きできます。 JSON オブジェクトには次のテンプレートがあります。

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


上記のテンプレートの最上位キーはすべてオプションで、デフォルト値（*backgroundLogin*、*backgroundLogut* はデフォルトで false、mvpdConfig は null です。つまり、MVPDの設定は上書きされません）。


- **注意**：上記のパラメーターに無効な値/タイプを指定すると、未定義の動作が発生します。



更新なしのログインとログアウトをアクティブ化し、MVPD1 を完全なページのリダイレクトログイン（iFrame 以外）に、MVPD2 を幅=500、高さ=300 の iFrame ログインに変更するシナリオの設定例を次に示します。

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


**コールバックがトリガーされました：** [setConfig （） ](#setconfigconfigxml-setconfigconfigxml)
</br>

[トップに戻る](#top)

</br>

## getAuthorization （inResourceID, redirect_url） {#getauthorization(inresourceid,redirect_url)}

**説明：** 指定されたリソースの認証をリクエストします。 ユーザーが認証可能なリソースにアクセスしようとするたびに、この関数を呼び出して Access Enabler から短期間有効な認証トークンを取得します。 リソース ID は、MVPDによる認証と同意されます。

現在の顧客のキャッシュされた認証トークンを使用します。 そのようなトークンが見つからない場合、は最初に認証プロセスを開始し、次に認証を続行します。

**パラメーター：**

- `inResourceID` - ユーザーが認証をリクエストするリソースの ID。
- `redirect_url` - （オプション）リダイレクト URL を指定すると、MVPD認証プロセスは、認証が開始されたページではなく、そのページにユーザーを返します。


**Callbacks triggered:** [setToken （） ](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) on success, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) on failed

>[!CAUTION]
>
>可能な限り、getAuthorization （）ではなく checkAuthorization （）を使用してください。 getAuthorization （） メソッドは、完全な認証フローを開始します（ユーザーが認証されていない場合）。これにより、プログラマ側の実装が複雑になる可能性があります。

</b>

[トップに戻る](#top)

</br>

## getAuthentication （redirect_url） {#getauthentication(redirect_url}

**説明：** 現在の顧客の認証を要求します。 通常、「ログイン」ボタンのクリックに応答して呼び出されます。 現在の顧客のキャッシュされた認証トークンを確認します。 そのようなトークンが見つからない場合は、認証プロセスを開始します。 これにより、デフォルトまたはカスタムの provider-selection ダイアログが呼び出され、選択されたプロバイダーを使用してMVPDのログインインターフェイスにリダイレクトされます。

成功すると、はユーザーの認証トークンを作成して保存します。 認証が失敗した場合、プロバイダーは適切なエラーメッセージを [setAuthenticationStatus （） ](#setauthenticationstatusisauthenticated-errorcode) コールバックに返します。

**パラメーター：**

- redirect_url - オプションで、リダイレクト URL を指定します。これにより、MVPD認証プロセスは、認証が開始されたページではなく、そのページにユーザーを返します。

**コールバックがトリガーされました：** [setAuthenticationStatus （） ](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog （） ](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData （） ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[トップに戻る](#top)

</br>

## checkAuthN {#checkauthn}

**説明：** 現在の顧客の現在の認証ステータスを確認します。  どの UI にも関連付けられていません。

**コールバックがトリガーされました：** [setAuthentcationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)

</br>

[トップに戻る](#top)

</br>

## checkAuthorization （inResourceID） {#checkauthorization(inresourceid)}

**説明：** このメソッドは、現在の顧客と指定されたリソースの認証ステータスをアプリケーションで確認するために使用されます。 まず、認証ステータスを最初に確認します。 認証されていない場合、tokenRequestFailed （） コールバックがトリガーされ、メソッドが終了します。 認証されたユーザーは、認証フローもトリガーします。 [getAuthorization （） ] （#getAuthZ メソッドの詳細を参照してください。

>[!TIP]
>
> **check-status 関数を使用** 認証または認証のステータスを確認してから認証をリクエストする必要はありません。 これらの関数を呼び出して、例えば、独自のステータス表示を更新することができます。 ユーザーの操作が必要な場合は使用しないでください。

**パラメーター：**

- `inResourceID` - ユーザーが認証をリクエストするリソースの ID。


**トリガーされたコールバック：**
[setToken （） ](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed （） ](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData （） ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources （resources） {#checkPreauthorizedResources(resources)}

**説明：** のリストに対する「preflight」認証ステータスをリクエストします
リソース。

**パラメーター：**

- *resources*: resources パラメーターは、認証を確認する必要があるリソースの配列です。 リストの各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、`getAuthorization()` 呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマーとMVPDの間で確立された同意値や、メディア RSS フラグメントです。

</br>

## checkPreauthorizedResources （resources-cache=true） {#checkPreauthorizedResources(resources-cache=true)}

この API バリアントは、JS SDK バージョン 4.0 以降で使用できます


**パラメーター：**

- *resources*: resources パラメーターは、認証を確認する必要があるリソースの配列です。 リストの各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、`getAuthorization()` 呼び出しのリソース ID と同じ制限が適用されます。つまり、プログラマーとMVPDの間で確立された同意値や、メディア RSS フラグメントです。

- *キャッシュ*：事前承認されたリソースを確認する際に内部キャッシュを使用するかどうか。 これはオプションのパラメーターで、デフォルトは **true** です。 true の場合、動作は上記の API と同じです。つまり、この関数への後続の呼び出しは、内部キャッシュを使用して事前承認済みリソースを解決します。 このパラメーターに **false** を渡すと、内部キャッシュが無効になり、**checkPreauthorizedResources** API が呼び出されるたびにサーバーが呼び出されます。

**トリガーされたコールバック：** [preauthorizedResources （） ](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[ トップに戻る ](#top)
</br>

## getMetadata （Key） {#getMetadata}

**説明：** アクセス イネーブラ ライブラリによってメタデータとして公開された情報を取得します。

メタデータには次の 2 種類があります。

- **静的** （認証トークン TTL、認証トークン TTL およびデバイス ID）
- **ユーザーメタデータ** （認証フローや承認フローの最中に、MVPDからユーザーのデバイスに渡されるユーザー固有の情報を含みます）

**詳細情報：**[ ユーザーメタデータ ](#UserMetadata)

**パラメーター：**

- *key*：リクエストされたメタデータを指定する ID:
   - キーが `"TTL_AUTHN",` の場合、クエリが実行されて認証トークンの有効期限が取得されます。

   - key が `"TTL_AUTHZ"` で、params がリソース ID を文字列として含む配列である場合、指定したリソースに関連付けられた認証トークンの有効期限を取得するためにクエリが実行されます。

   - キーが `"DEVICEID"` の場合、現在のデバイス ID を取得するためにクエリが実行されます。 この機能はデフォルトで無効になっており、プログラマーはイネーブルメントと料金についてAdobeに問い合わせる必要があります。

   - キーが次のユーザーメタデータタイプのリストからのものである場合、対応するユーザーメタデータを含む JSON オブジェクトが [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) コールバック関数に送信されます。

   - `"zip"` – 郵便番号

   - `"encryptedZip"` – 暗号化された郵便番号

   - `"householdID"` – 世帯の識別子。 MVPDがサブアカウントをサポートしていない場合、これは userID と同一になります。

   - `"maxRating"` - ユーザーの保護者による制限の上限

   - `"userID"` - ユーザー ID。 MVPDがサブアカウントをサポートし、ユーザーがメインアカウントでない場合、userID は householdID とは異なります。

   - `"channelID"` - ユーザーが表示できるチャネルのリスト

   - `"is_hoh"` - ユーザーが世帯の責任者かどうかを識別するフラグ

   - `"encryptedZip"` – 暗号化された郵便番号

   - `"typeID"` - ユーザーアカウントがプライマリ/セカンダリ アカウントかどうかを識別するフラグ

   - `"primaryOID"` – 世帯の識別子

   - `"postalCode"` – 郵便番号と類似

   - `"acctID"` - アカウント ID

   - `"acctParentID"` - アカウント親 ID

  **メモ**：プログラマーが使用できる実際のユーザーメタデータは、MVPDが提供する内容によって異なります。  使用可能なユーザーメタデータの現在のリストについては、[ ユーザーメタデータ ](#UserMetadata) を参照してください。


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


**トリガーされたコールバック：** [setMetadataStatus （） ](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[トップに戻る](#top)

</br>


## setSelectedProvider （providerid） {#setSelectedProvider}

**説明：** この関数は、ユーザーがプロバイダ選択 UI からMVPDを選択してアクセス イネーブラにプロバイダ選択を送信した場合は、この関数を呼び出します。または、ユーザーがプロバイダ選択 UI を閉じてもプロバイダを選択できない場合は、null パラメータを指定してこの関数を呼び出します。

**コールバック
triggered:**[ setAuthentcationStatus （） ](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData （） ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[トップに戻る](#top)

</br>

## getSelectedProvider （） {#getSelectedProvider}

**説明：** プロバイダー選択ダイアログでの顧客の選択結果を取得します。 これは、最初の認証チェック後であればいつでも使用できます。

この関数は非同期であり、その結果を `selectedProvider()` コールバック関数に返します。

- **MVPD** 現在選択されているMVPD。MVPDが選択されていない場合は null。
- **AE_State** 「新規ユーザー」、「ユーザー未認証」または「ユーザー認証済み」のうちの現在の顧客の認証結果

**コールバックがトリガーされました：** [selectedProvider （） ](#getselectedprovider-getselectedprovider)

</br>

[トップに戻る](#top)

</br>

## ログアウト {#logout}

**説明：** 現在の顧客をログアウトし、そのユーザーのすべての認証情報と認証情報を消去します。 顧客のシステムからすべての authN および authZ トークンを削除します。

**コールバックがトリガーされました：** [setAuthentcationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)
</br>

[トップに戻る](#top)

</br>

## コールバック定義 {#calllback-definitions}

非同期リクエスト呼び出しに対する応答を処理するには、次のコールバックを実装する必要があります。

- [entitlementLoaded （）](#entitlementloaded-entitlementloaded)
- [setConfig （）](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog （）](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame （）](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus （）](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData （）](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken （）](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed （）](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources （）](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus （）](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider （）](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded （） {#entitlementLoaded}

**説明：** アクセス イネーブラの初期化が完了し、要求を受信する準備ができたときにトリガーされます。 このコールバックを実装すると、Access Enabler API を使用して通信を開始できるタイミングがわかります。
</br>

[トップに戻る](#top)

</br>

## setConfig （configXML） {#setconfig(configXML)}

**説明：** このコールバックを実装して、設定情報およびMVPD リストを受け取ります。

**パラメーター：**

- *configXML*:MVPD リストを含む、現在のリクエスターの設定を保持している xml オブジェクト。


**トリガー：** [setRequestor （） ](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[トップに戻る](#top)

</br>

## displayProviderDialog （providers） {#displayproviderdialog(providers)}

**説明：** このコールバックを実装して、独自のカスタムプロバイダー選択 UI を呼び出します。 顧客に選択肢を提供するには、ダイアログで表示名（およびオプションのロゴ）を使用する必要があります。 顧客が選択してダイアログを閉じたら、選択したプロバイダーに関連付けられている ID を *setSelectedProvider （）* への呼び出しに送信します。

**パラメーター：**

- *providers* - リクエストされた MVPD を表すオブジェクトの配列。

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**トリガー：** [getAuthentication （） ](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization （） ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[ トップに戻る ](#top)

</br>

## createIFrame （inWidth, inHeight） {#createIFrame(inWidth,inHeight)}

**説明：** 認証ログインページの UI を表示する iFrame を必要とするMVPDをユーザーが選択した場合に、このコールバックを実装します。

**トリガー：:**[ setSelectedProvider （） ](#setselectedproviderproviderid-setselectedprovider)

</br> [ トップに戻る ](#top)

</br>

## setAuthenticationStatus （isAuthenticated, errorCode） {#set-authn-status-isauthn-error}

**説明：** このコールバックを実装して、認証ステータス（1=認証済みまたは 0=未認証）と、認証ステータスの特定中にエラーが発生した場合の説明的なエラーメッセージ（チェックが正常に完了した場合は空の文字列）を受け取ります。

>[!NOTE]
> 
>現在の [ 事前エラーレポート ](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) システムを使用している場合は、この関数に送信された errorCode パラメーターを無視できます。  ただし、isAuthenticated フラグは、使用権フローでのユーザーの認証状態の追跡にまだ使用されています


**パラメーター：**

- *isAuthenticated* – 認証ステータスとして、1 （認証済み）または 0 （未認証）を指定します。
- *errorCode* – 認証ステータスの確認中に発生したエラー。 何も指定しない場合は、空の文字列を返します。


**トリガー：** [checkAuthentication （） ](#checkauthn-checkauthn)、[getAuthentication （） ](#getauthenticationredirecturl-getauthenticationredirecturl)、[checkAuthorization （） ](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[トップに戻る](#top)

</br>

## sendTrackingData （trackingEventType, trackingData） {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>デバイスタイプとオペレーティングシステムは、公開 Java ライブラリ（<http://java.net/projects/user-agent-utils>）とユーザーエージェント文字列を使用して取得されます。 この情報は、操作指標をデバイスカテゴリに分類する粗い方法としてのみ提供されますが、Adobeは誤った結果に対して責任を負わないことに注意してください。 それに応じて新しい機能を使用してください。

**説明：** 特定のイベントが発生したときにトラッキングデータを受け取るには、このコールバックを実装します。 例えば、これを使用して、同じ資格情報でログインしたユーザーの数を追跡できます。 現在、トラッキングは設定できません。 Adobe Pass Authentication 1.6 を使用すると、`sendTrackingData()` はデバイス、アクセス イネーブラ クライアント、およびオペレーティング システムの種類に関する情報もレポートします。 `sendTrackingData()` コールバックは、後方互換性を維持します。

- デバイスタイプに指定可能な値：
   - コンピューター
   - タブレット
   - mobile
   - gameconsole
   - 不明

- Access Enabler クライアント・タイプに指定可能な値：
   - html5
   - ios
   - android


イベントタイプと、関連付けられた情報の配列を渡します。 イベントタイプは次のとおりです。

| mvpdSelection | プロバイダー選択ダイアログで、MVPDが選択されました。 |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | 認証チェックが完了しました。 |
| authorizationDetection | 認証リクエストが完了しました。 |

</br>
データは、各イベントタイプに固有です。
</br>

| イベントタイプ （String） | データ （配列） |
|:--- | :--- |
| mvpdSelection | 0：選択されたMVPD |
|  | 1: デバイスタイプ |
|  | 2: アクセス イネーブラ クライアント タイプ |
|  | 3: OS |
| authenticationDetection | 0：トークンリクエストが成功したかどうか（true/false） |
|  | 1:MVPD ID |
|  | 2: GUID |
|  | 3: トークンは既にキャッシュにあります（true/false） |
|  | 4: デバイスタイプ |
|  | 5: アクセス イネーブラ クライアント タイプ |
|  | 6: OS |
| authorizationDetection | 0：トークンリクエストが成功したかどうか（true/false） |
|  | 1:MVPD ID |
|  | 2: GUID |
|  | 3: トークンは既にキャッシュにあります（true/false） |
|  | 4 : エラー |
|  | 5：詳細 |
|  | 6: デバイスタイプ |
|  | 7: アクセス イネーブラ クライアント タイプ |
|  | 8: OS |


**トリガー：** [checkAuthentication （） ](#checkauthn-checkauthn)、[getAuthentication （） ](#getauthenticationredirecturl-getauthenticationredirecturl)、[checkAuthorization （） ](#checkauthorizationinresourceid-checkauthorizationinresourceid)、[getAuthorization （） ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[トップに戻る](#top)

</br>

## setToken （inRequestedResourceID, inToken） {#setToken(inRequestedResourceID,inToken)}

**説明：** このコールバックを実装して、認証リクエストまたは承認確認リクエストが行われ、正常に完了した短期間有効のメディアトークン（inToken）とリソースの ID （inRequestedResourceID）を受け取ります。

**トリガー：** [checkAuthorization （） ](#checkAuthZ), [getAuthorization （） ](#getAuthZ)
</br>

[トップに戻る](#top)

</br>

## tokenRequestFailed （inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage） {#token-request-failed-error-msg}

**説明：** 認証または承認確認リクエストが失敗したときに呼び出される、このコールバックを実装します。 オプションで、MVPDで使用し、プログラマーが表示するカスタムメッセージを提供できます。

>[!IMPORTANT]
>
>このコールバック関数は、古いオリジナルのAdobe Pass認証エラーレポートシステムの一部です。 これは後方互換性のために保持されますが、現在の高度なエラーレポートシステムを使用して独自のコールバックを実装している場合、この関数を使用する必要はありません。 新しいエラーレポートシステムは、承認（または他の操作）が失敗した理由に関する詳細を、エラーまたは警告のタイプごとに推奨されるアクションコースと共に提供します。

**パラメーター：**

- *inRequestedResourceID* – 承認リクエストで使用されたリソース ID を提供する文字列。
- *inRequestErrorCode* - エラーの理由を示すAdobe Pass認証エラーコードを表示する文字列。使用可能な値は「User Not Authenticated Error」と「User Not Authorized Error」です。詳しくは、以下の「Callback error codes」を参照してください。
- *inRequestDetailedErrorMessage* – 表示に適した追加の説明文字列。 何らかの理由でこの記述文字列が使用できない場合、Adobe Pass認証は空の文字列 **（&quot;）** を送信します。  これは、MVPDでカスタムエラーメッセージや営業関連のメッセージを渡すために使用できます。 例えば、ある購読者がリソースの認証を拒否された場合、MVPDは次のような `*inRequestDetailedErrorMessage*` を返すことがあります。**「現在、パッケージ内のこのチャネルへのアクセス権はありません。 パッケージをアップグレードするには、\*こちら\*をクリックしてください。** メッセージは、このコールバックを通じてAdobe Pass認証によってプログラマーのサイトに渡されます。 その後、プログラマーは表示または無視するオプションを選択できます。 Adobe Pass認証では、`*inRequestDetailedErrorMessage*` を使用して、エラーの原因となった可能性のある状態をプログラマーに通知することもできます。 例えば、**A network error occurred when communication with the provider&#39;s authorization service」のように指定します。**



**トリガー：** [checkAuthorization （） ](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization （） ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[トップに戻る](#top)

</br>


## preauthorizedResources （authorizedResources） {#preauthorizedResources(authorizedResources)}

**説明：** `checkPreauthorizedResources()` の呼び出し後に返された承認済みリソースの一覧を配信するアクセス イネーブラによってトリガーされたコールバック。

**パラメーター：**

- *authorizedResources*：許可されたリソースのリストです。

**トリガー：** [checkPreauthorizedResources （） ](#checkPreauthRes)
</br>

[トップに戻る](#top)

</br>

## setMetadataStatus （key, encrypted, data） {#setMetadataStatus(key,encrypted,data)}

**説明：** アクセス イネーブラによってトリガーされ、`getMetadata()` 呼び出しを介して要求されたメタデータを配信するコールバック。

**詳細情報：**[ ユーザーメタデータ ](#userMetadata)

**パラメーター：**

- *key （String）*：リクエストがおこなわれたメタデータのキー。
- *encrypted （ブール値）*:「値」が暗号化されているかどうかを示すフラグ。 これが「true」の場合、「value」は実際の値の JSON web 暗号化された表現になります。
- *data （JSON オブジェクト）*：メタデータを表現した JSON オブジェクトです。単純なリクエスト （「`TTL_AUTHN`」、「`TTL_AUTHZ`」、「`DEVICEID`」）の場合、結果は文字列（認証 TTL、認証 TTL、デバイス ID のいずれかを表します）になります。 ユーザーメタデータリクエストの場合、結果は、メタデータペイロードを表すプリミティブまたは JSON オブジェクトです。 JSON ユーザーメタデータオブジェクトの実際の構造は、次のようになります。

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


**Trigger by:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[ トップに戻る ](#top)

</br>

## selectedProvider （result） {#selectedProvider(result)}

**説明：** 現在選択されているMVPDと、`result` パラメーターにラップされた現在のユーザーの認証結果を受け取るには、このコールバックを実装します。 `result` パラメーターは、次のプロパティを持つオブジェクトです。

- **MVPD** 現在選択されているMVPD。MVPDが選択されていない場合は null。
- **AE\_State** 現在のユーザーの認証結果（「新規ユーザー」、「ユーザー未認証」、「ユーザー認証済み」）

**トリガー：** [getSelectedProvider （） ](#getSelProv)

</br>

[トップに戻る](#top)

</br>

### コールバックエラーコード {#callback-error-codes}

| 一般的なエラー | |
|:--- | :--- | 
| 内部エラー | リクエストの処理中にシステムエラーが発生しました。 |
| プロバイダーが選択されていませんエラー | 顧客がプロバイダ選択ダイアログでキャンセルすると発生します |
| プロバイダーを利用できないエラー | 利用できるプロバイダーがない場合に発生します。 |

| 認証エラー | |
|:--- | :--- | 
| 汎用認証エラー | 理由が不明または公開できない場合に返されます。 |
| 内部認証エラー | 認証を試行中にシステムエラーが発生しました。 |
| User Not Authenticated エラー | ユーザーが認証されていません。 |
| 複数の認証リクエストエラー | 最初の認証要求が完了する前に、追加の認証要求を受信しました。 |

| 認証エラー | |
|:--- | :--- | 
| 汎用認証エラー | 理由が不明または公開できない場合に返されます。 |
| 内部認証エラー | の認証中にシステムエラーが発生しました。 |
| ユーザーが承認されていませんエラー | 顧客は要求されたコンテンツを表示する権限がありません。 |

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
