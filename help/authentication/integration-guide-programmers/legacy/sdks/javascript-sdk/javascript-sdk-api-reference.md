---
title: JavaScript SDK API リファレンス
description: JavaScript SDK API リファレンス
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '2902'
ht-degree: 0%

---

# （レガシー） JavaScript SDK API リファレンス {#javascript-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## API リファレンス {#api-reference}

これらの関数は、MVPDとのインタラクションのリクエストを開始します。 すべての呼び出しは非同期です。応答を処理するには、[ コールバック ](#callbacks)を実装する必要があります。

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

**説明：**&#x200B;要求の元となるサイトを特定します。  通信セッションでは、他のAPI呼び出しの前にこの呼び出しを行う必要があります。

**パラメーター：**

- *inRequestorID* – 登録時にAdobeが元のサイトに割り当てた一意のID。

- *エンドポイント* – このパラメーターはオプションです。 次のいずれかの値を指定できます。

   - Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できる配列（デバッグ目的で様々なインスタンスを使用する場合があります）。 複数のURLが指定されている場合、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各MVPDは、最速のサービスプロバイダー、つまり最初に応答し、そのMVPDをサポートするプロバイダーに関連付けられます。 デフォルトでは（値が指定されていない場合）、Adobe サービスプロバイダーが使用されます（<http://sp.auth.adobe.com/>）。

  例：
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - Application ID値、Visitor ID値、refresh-less settings （background login logout）およびMVPD settings （iFrame）を含むJSON オブジェクト。 値はすべてオプションです。
   1. 指定した場合、Experience Cloud visitorIDは、ライブラリによって実行されたすべてのネットワーク呼び出しに対してレポートされます。 この値は、後で高度な分析レポートに使用できます。
   2. アプリケーションの一意の識別子が指定されている場合 – `applicationId` – 値は、X-Device-Info HTTP ヘッダーの一部としてアプリケーションによって行われたその後のすべての呼び出しに追加されます。 この値は、後で適切なクエリを使用して[ESM](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) レポートから取得できます。

  **メモ：**&#x200B;すべてのJSON キーでは、大文字と小文字が区別されます。

  例：

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- プログラマーは、ログインにiFrameが必要かどうか（*iFrameRequired* キー）、iFrame ディメンション（*iFrameWidth*&#x200B;および&#x200B;*iFrameHeight* キー）を指定することで、Adobe Pass Authenticationで設定されているMVPD設定を上書きできます。 JSON オブジェクトには、次のテンプレートがあります。

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


上記のテンプレートのすべての最上位キーはオプションで、デフォルト値（*backgroundLogin*、*backgroundLogut*&#x200B;はデフォルトでfalse、mvpdConfigはnullです。つまり、MVPDの設定は上書きされません）を持ちます。


- **注意**：上記のパラメーターに無効な値や型を指定すると、動作が未定義になります。



次のシナリオの設定例を示します。更新なしのログインとログアウトを有効にし、MVPD1をフルページリダイレクトログイン（iFrame以外）に、MVPD2をwidth=500、height=300のiFrame ログインに変更します。

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


**コールバックがトリガーされました：** [setConfig （）](#setconfigconfigxml-setconfigconfigxml)
</br>

[トップへ戻る](#top)

</br>

## getAuthorization （inResourceID, redirect_url） {#getauthorization(inresourceid,redirect_url)}

**説明：**&#x200B;は、指定されたリソースの承認を要求します。 お客様が承認可能なリソースにアクセスするたびに、この関数を呼び出して、Access Enablerから短期間有効な認証トークンを取得します。 リソース IDは、認証を提供するMVPDとの間で合意されます。

現在の顧客に対してキャッシュされた認証トークンを使用します。 このようなトークンが見つからない場合は、最初に認証プロセスを開始し、その後に認証を続行します。

**パラメーター：**

- `inResourceID` - ユーザーが認証を要求するリソースのID。
- `redirect_url` – 必要に応じてリダイレクト URLを指定します。これにより、MVPDの認証プロセスで、認証が開始されたページではなく、そのページにユーザーが戻されます。


**コールバックがトリガーされました：** [setToken （） ](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) （成功時）、[tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) （失敗時）

>[!CAUTION]
>
>可能な限り、getAuthorization （）の代わりにcheckAuthorization （）を使用してください。 getAuthorization （） メソッドは、（ユーザーが認証されていない場合）完全な認証フローを開始します。これにより、プログラマー側で複雑な実装が行われる可能性があります。

</b>

[トップへ戻る](#top)

</br>

## getAuthentication （redirect_url） {#getauthentication(redirect_url}

**説明：**&#x200B;現在の顧客の認証を要求します。 通常、ログインボタンのクリックに応じて呼び出されます。 現在の顧客のキャッシュされた認証トークンを確認します。 そのようなトークンが見つからない場合は、認証プロセスを開始します。 これにより、デフォルトまたはカスタムプロバイダー選択ダイアログが呼び出され、選択したプロバイダーを使用してMVPDのログインインターフェイスにリダイレクトされます。

成功すると、ユーザーの認証トークンを作成して保存します。 認証が失敗した場合、プロバイダーは[setAuthenticationStatus （） ](#setauthenticationstatusisauthenticated-errorcode) コールバックに適切なエラーメッセージを返します。

**パラメーター：**

- redirect_url – 必要に応じてリダイレクト URLを指定します。これにより、MVPDの認証プロセスは、認証が開始されたページではなく、そのページにユーザーを返します。

**コールバックがトリガーされました：** [setAuthenticationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)、[displayProviderDialog （） ](#displayproviderdialogproviders-displayproviderdialogproviders)、[sendTrackingData （） ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[トップへ戻る](#top)

</br>

## checkAuthN {#checkauthn}

**説明：**&#x200B;現在の顧客の現在の認証ステータスを確認します。  UIに関連付けられていません。

**コールバックがトリガーされました：** [setAuthentcationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)

</br>

[トップへ戻る](#top)

</br>

## checkAuthorization （inResourceID） {#checkauthorization(inresourceid)}

**説明：**&#x200B;このメソッドは、アプリケーションが現在の顧客と指定されたリソースの認証ステータスを確認するために使用します。 まず、認証ステータスを確認します。 認証されていない場合、tokenRequestFailed （） コールバックがトリガーされ、メソッドが終了します。 ユーザーが認証された場合は、認証フローもトリガーします。 [getAuthorization （） ] （#getAuthZ メソッドの詳細を参照してください。

>[!TIP]
>
> **check-status関数を使用する**&#x200B;認証または認証のステータスを確認してから認証をリクエストする必要はありません。 例えば、これらの関数を呼び出して、独自のステータス表示を更新できます。 ユーザーによる操作が必要な場合は、これらのツールを使用しないでください。

**パラメーター：**

- `inResourceID` - ユーザーが認証を要求するリソースのID。


**コールバックがトリガーされました：**
[setToken （） ](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)、[tokenRequestFailed （） ](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)、[sendTrackingData （） ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)、[setAuthenticationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources （リソース） {#checkPreauthorizedResources(resources)}

**説明：**のリストに対して「プリフライト」認証ステータスを要求
リソース：

**パラメーター：**

- *resources*: resources パラメーターは、認証を確認する必要があるリソースの配列です。 リスト内の各要素は、リソース IDを表す文字列である必要があります。 リソース IDは、`getAuthorization()`呼び出しのリソース IDと同じ制限を受けます。つまり、プログラマーとMVPDの間で確立された合意値、またはメディア RSS フラグメントです。

</br>

## checkPreauthorizedResources （resources-cache=true） {#checkPreauthorizedResources(resources-cache=true)}

このAPI バリアントは、JS SDK バージョン 4.0以降で使用できます


**パラメーター：**

- *resources*: resources パラメーターは、認証を確認する必要があるリソースの配列です。 リスト内の各要素は、リソース IDを表す文字列である必要があります。 リソース IDは、`getAuthorization()`呼び出しのリソース IDと同じ制限を受けます。つまり、プログラマーとMVPDの間で確立された合意値、またはメディア RSS フラグメントです。

- *cache*：事前承認済みのリソースを確認する際に内部キャッシュを使用するかどうかを指定します。 これはオプションのパラメーターで、デフォルトは&#x200B;**true**&#x200B;です。 trueの場合、動作は上記のAPIと同じであり、この関数に対する後続の呼び出しは、事前承認済みリソースを解決するために内部キャッシュを使用します。 このパラメーターに&#x200B;**false**&#x200B;を渡すと、内部キャッシュが無効になり、**checkPreauthorizedResources** APIが呼び出されるたびにサーバーコールが発生します。

**コールバックがトリガーされました：** [preauthorizedResources （） ](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[ トップへ戻る](#top)
</br>

## getMetadata （Key） {#getMetadata}

**説明：** Access Enabler ライブラリによってメタデータとして公開された情報を取得します。

メタデータには2種類あります。

- **静的** （認証トークン TTL、認証トークン TTL、およびデバイス ID）
- **User Metadata** （これには、認証および/または認証フロー中にMVPDからユーザーのデバイスに渡されるユーザー固有の情報が含まれます）

**詳細情報：** [ ユーザーメタデータ ](#UserMetadata)

**パラメーター：**

- *key*：要求されたメタデータを指定するID:
   - キーが`"TTL_AUTHN",`の場合、認証トークンの有効期限を取得するためにクエリが実行されます。

   - キーが`"TTL_AUTHZ"`で、paramsがリソース IDを文字列として含む配列である場合、クエリは、指定されたリソースに関連付けられた認証トークンの有効期限を取得するために行われます。

   - キーが`"DEVICEID"`の場合、現在のデバイス IDを取得するためにクエリが実行されます。 この機能はデフォルトで無効になっており、プログラマーは有効化と料金についてAdobeに問い合わせる必要があります。

   - キーが次のユーザーメタデータタイプのリストにある場合、対応するユーザーメタデータを含むJSON オブジェクトが[`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) コールバック関数に送信されます。

   - `"zip"` – 郵便番号

   - `"encryptedZip"` – 暗号化された郵便番号

   - `"householdID"` – 世帯ID。 MVPDがサブアカウントをサポートしていない場合、これはuserIDと同じです。

   - `"maxRating"` - ユーザーの最大保護者の評価

   - `"userID"` - ユーザーID。 MVPDがサブアカウントをサポートしており、ユーザーがメインアカウントではない場合、userIDはhouseholdIDとは異なります。

   - `"channelID"` - ユーザーが表示できるチャネルのリスト

   - `"is_hoh"` - ユーザーが世帯責任者かどうかを識別するフラグ

   - `"encryptedZip"` – 暗号化された郵便番号

   - `"typeID"` - ユーザーアカウントがプライマリ/セカンダリアカウントであるかどうかを識別するフラグ

   - `"primaryOID"` – 世帯ID

   - `"postalCode"` – 郵便番号に似ています

   - `"acctID"` - アカウント ID

   - `"acctParentID"` - アカウントの親ID

  **注意**: プログラマが使用できる実際のユーザーメタデータは、MVPDで使用可能な内容によって異なります。  使用可能なユーザーメタデータの現在のリストについては、[ ユーザーメタデータ ](#UserMetadata)を参照してください。


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


**コールバックがトリガーされました：** [setMetadataStatus （） ](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[トップへ戻る](#top)

</br>


## setSelectedProvider （providerid） {#setSelectedProvider}

**説明：** プロバイダーを選択せずにプロバイダー選択UIを閉じた場合に備えて、プロバイダー選択UIからMVPDを選択してこの関数を呼び出し、プロバイダー選択UIをAccess Enablerに送信するか、null パラメーターでこの関数を呼び出します。

**コールバック
トリガー：**[ setAuthentcationStatus （） ](#setauthenticationstatusisauthenticated-errorcode)、[sendTrackingData （） ](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[トップへ戻る](#top)

</br>

## getSelectedProvider （） {#getSelectedProvider}

**説明：** プロバイダー選択ダイアログで、顧客の選択の結果を取得します。 これは、最初の認証チェックの後いつでも使用できます。

この関数は非同期であり、その結果を`selectedProvider()` コールバック関数に返します。

- **MVPD**&#x200B;現在選択されているMVPD。MVPDが選択されていない場合はnull。
- **AE_State**&#x200B;現在の顧客に対する認証結果（「新規ユーザー」、「未認証ユーザー」、「認証済みユーザー」のいずれか）

**コールバックがトリガーされました：** [selectedProvider （） ](#getselectedprovider-getselectedprovider)

</br>

[トップへ戻る](#top)

</br>

## ログアウト {#logout}

**説明：**&#x200B;現在の顧客をログアウトし、そのユーザーのすべての認証および認証情報を消去します。 顧客のシステムからすべてのauthN トークンとauthZ トークンを削除します。

**コールバックがトリガーされました：** [setAuthentcationStatus （）](#setauthenticationstatusisauthenticated-errorcode)
</br>

[トップへ戻る](#top)

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

**説明：** Access Enablerが初期化を完了し、リクエストを受信する準備ができたときにトリガーされます。 Access Enabler APIで通信を開始できるタイミングを知るには、このコールバックを実装します。
</br>

[トップへ戻る](#top)

</br>

## setConfig （configXML） {#setconfig(configXML)}

**説明：**&#x200B;このコールバックを実装して、設定情報とMVPD リストを受け取ります。

**パラメーター：**

- *configXML*: MVPD リストを含む、現在のREQUESTORの設定を保持するxml オブジェクト。


**トリガー：** [setRequestor （） ](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[トップへ戻る](#top)

</br>

## displayProviderDialog （プロバイダ） {#displayproviderdialog(providers)}

**説明：**&#x200B;独自のカスタムプロバイダー選択UIを呼び出すには、このコールバックを実装します。 ダイアログでは、表示名（およびオプションのロゴ）を使用して、顧客の選択肢を提供する必要があります。 顧客が選択し、ダイアログを閉じた場合、選択したプロバイダーの関連IDを&#x200B;*setSelectedProvider （）*&#x200B;への呼び出しに送信します。

**パラメーター：**

- *providers* – 要求されたMVPDを表すオブジェクトの配列：

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**トリガー：** [getAuthentication （） ](#getauthenticationredirecturl-getauthenticationredirecturl)、[getAuthorization （） ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[ トップへ戻る](#top)

</br>

## createIFrame （inWidth, inHeight） {#createIFrame(inWidth,inHeight)}

**説明：** ユーザーが認証ログインページ UIを表示するiFrameを必要とするMVPDを選択した場合、このコールバックを実装します。

**トリガー：**[ setSelectedProvider （） ](#setselectedproviderproviderid-setselectedprovider)

</br> [ トップへ戻る](#top)

</br>

## setAuthenticationStatus （isAuthenticated, errorCode） {#set-authn-status-isauthn-error}

**説明：**&#x200B;このコールバックを実装して、認証ステータス（1=認証済みまたは0=未認証）と、認証ステータスの決定中にエラーが発生した場合の説明エラーメッセージ（チェックが正常に完了した場合は空の文字列）を受信します。

>[!NOTE]
> 
>現在の[Advance Error Reporting](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md) システムを使用している場合は、この関数に送信されたerrorCode パラメーターを無視できます。  ただし、isAuthenticated フラグは、エンタイトルメントフロー内のユーザーの認証ステータスを追跡するためにまだ使用されています


**パラメーター：**

- *isAuthenticated* – 認証ステータスを提供します：1 （認証済み）または0 （認証済みでない）。
- *errorCode* – 認証ステータスの決定時に発生したエラー。 空の文字列（なし）。


**トリガー：** [checkAuthentication （） ](#checkauthn-checkauthn)、[getAuthentication （） ](#getauthenticationredirecturl-getauthenticationredirecturl)、[checkAuthorization （） ](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[トップへ戻る](#top)

</br>

## sendTrackingData （trackingEventType, trackingData） {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>デバイスの種類とオペレーティング システムは、パブリック Java ライブラリ （<http://java.net/projects/user-agent-utils>）とユーザーエージェント文字列を使用して取得されます。 この情報は、操作指標をデバイスカテゴリーに分類する大まかな方法としてのみ提供されますが、Adobeは誤った結果に対して責任を負うことはできません。 それに応じて新しい機能を使用してください。

**説明：**&#x200B;特定のイベントが発生したときにトラッキングデータを受信するには、このコールバックを実装します。 例えば、同じ資格情報でログインしたユーザーの数を追跡するために使用できます。 トラッキングは現在、設定可能ではありません。 Adobe Pass Authentication 1.6では、`sendTrackingData()`は、デバイス、Access Enabler クライアント、およびオペレーティング システムの種類に関する情報も報告します。 `sendTrackingData()` コールバックは後方互換性を維持します。

- デバイスタイプの可能な値：
   - コンピューター
   - タブレット
   - mobile
   - gameconsole
   - 不明

- Access Enabler クライアントの種類に指定できる値：
   - html5
   - ios
   - android


イベントタイプと関連情報の配列を渡します。 イベントタイプは次のとおりです。

| mvpdSelection | プロバイダー選択ダイアログでMVPDを選択しました。 |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | 認証チェックが完了しました。 |
| authorizationDetection | 認証リクエストが完了しました。 |

</br>
データは各イベントタイプに固有です。
</br>

| イベントタイプ（文字列） | データ（配列） |
|:--- | :--- |
| mvpdSelection | 0：選択したMVPD |
|  | 1: デバイスタイプ |
|  | 2: Access Enabler クライアント タイプ |
|  | 3: OS |
| authenticationDetection | 0：トークンリクエストが成功したかどうか（true/false） |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: キャッシュ内のトークンが既に存在する（true/false） |
|  | 4: デバイスタイプ |
|  | 5: Access Enabler クライアント タイプ |
|  | 6: OS |
| authorizationDetection | 0：トークンリクエストが成功したかどうか（true/false） |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: キャッシュ内のトークンが既に存在する（true/false） |
|  | 4: エラー |
|  | 5：詳細 |
|  | 6: デバイスタイプ |
|  | 7: Access Enabler クライアント タイプ |
|  | 8: OS |


**トリガー：** [checkAuthentication （） ](#checkauthn-checkauthn)、[getAuthentication （） ](#getauthenticationredirecturl-getauthenticationredirecturl)、[checkAuthorization （） ](#checkauthorizationinresourceid-checkauthorizationinresourceid)、[getAuthorization （） ](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[トップへ戻る](#top)

</br>

## setToken （inRequestedResourceID, inToken） {#setToken(inRequestedResourceID,inToken)}

**説明：**&#x200B;このコールバックを実装して、承認要求または確認承認要求が行われ、正常に完了した短時間のみ有効なメディアトークン （inToken）とリソースのID （inRequestedResourceID）を受信します。

**トリガー：** [checkAuthorization （） ](#checkAuthZ)、[getAuthorization （）](#getAuthZ)
</br>

[トップへ戻る](#top)

</br>

## tokenRequestFailed （inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage） {#token-request-failed-error-msg}

**説明：**&#x200B;このコールバックを実装して、認証または確認承認要求が失敗したときに通知を受け取るようにします。 オプションで、MVPDでプログラマが表示するカスタムメッセージを提供するために使用できます。

>[!IMPORTANT]
>
>このコールバック関数は、古い元のAdobe Pass認証エラーレポートシステムの一部です。 後方互換性のために保持されますが、現在の高度なエラーレポートシステムを使用して独自のコールバックを実装している場合は、この機能をまったく使用する必要はありません。 新しいエラーレポートシステムでは、認証（または他の操作）が失敗した理由に関する詳細な情報と、各タイプのエラーまたは警告に対するアクションの推奨コースが提供されます。

**パラメーター：**

- *inRequestedResourceID* – 承認要求で使用されたリソース IDを提供する文字列。
- *inRequestErrorCode* - エラーの原因を示すAdobe Pass認証エラーコードを表示する文字列。考えられる値は「User Not Authenticated Error」および「User Not Authorized Error」です。詳細については、以下の「Callback Error Code」を参照してください。
- *inRequestDetailedErrorMessage* – 表示に適した追加の記述文字列。 この記述文字列が何らかの理由で使用できない場合、Adobe Pass Authenticationは空の文字列&#x200B;**（&quot;&quot;）**&#x200B;を送信します。  これは、MVPDでカスタムエラーメッセージやセールス関連メッセージを渡すために使用できます。 例えば、購読者がリソースの認証を拒否された場合、MVPDは次のように`*inRequestDetailedErrorMessage*`と返信できます。**「現在、パッケージ内のこのチャネルにはアクセスできません。 パッケージをアップグレードする場合は、\*こちら\*をクリックしてください。&quot;** メッセージは、このコールバックを通じてAdobe Pass認証によってプログラマーのサイトに渡されます。 その後、プログラマはそれを表示または無視するオプションを持っています。 Adobe Pass Authenticationでは、`*inRequestDetailedErrorMessage*`を使用して、エラーが発生した可能性のある条件をプログラマーに通知することもできます。 例えば、**「プロバイダーの認証サービスと通信する際にネットワークエラーが発生しました。**



**トリガー：** [checkAuthorization （） ](#checkauthorizationinresourceid-checkauthorizationinresourceid)、[getAuthorization （）](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[トップへ戻る](#top)

</br>


## preauthorizedResources （authorizedResources） {#preauthorizedResources(authorizedResources)}

**説明：** `checkPreauthorizedResources()`への呼び出し後に返された承認済みリソース リストを配信するアクセス イネーブラによってトリガーされるコールバック。

**パラメーター：**

- *authorizedResources*：承認済みリソースのリスト。

**トリガー：** [checkPreauthorizedResources （）](#checkPreauthRes)
</br>

[トップへ戻る](#top)

</br>

## setMetadataStatus （key, encrypted, data） {#setMetadataStatus(key,encrypted,data)}

**説明：** `getMetadata()`呼び出しを介して要求されたメタデータを配信するAccess Enablerによってトリガーされるコールバック。

**詳細情報：** [ ユーザーメタデータ ](#userMetadata)

**パラメーター：**

- *key （String）*：リクエストが行われたメタデータのキー。
- *encrypted （Boolean）*: 「値」が暗号化されているかどうかを示すフラグ。 これが「true」の場合、「value」は実際の値のJSON Web Encrypted表現になります。
- *data （JSON オブジェクト）*: メタデータを表すJSON オブジェクト。単純な要求（&#39;`TTL_AUTHN`&#39;、&#39;`TTL_AUTHZ`&#39;、&#39;`DEVICEID`&#39;）の場合、結果は文字列（認証TTL、認証TTLまたはデバイス IDを表す）になります。 ユーザーメタデータリクエストの場合、結果はメタデータペイロードを表すプリミティブまたはJSON オブジェクトにすることができます。 JSON ユーザーメタデータオブジェクトの実際の構造は、次のようになります。

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


**トリガー：** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[ トップに戻る](#top)

</br>

## selectedProvider （result） {#selectedProvider(result)}

**説明：**&#x200B;現在選択されているMVPDと、現在のユーザーの認証結果を`result` パラメーターにラップした値を受け取るには、このコールバックを実装します。 `result` パラメーターは、次のプロパティを持つオブジェクトです。

- **MVPD**&#x200B;現在選択されているMVPD。MVPDが選択されていない場合はnull。
- **AE\_State**&#x200B;現在のユーザーに対する認証結果（「新規ユーザー」、「ユーザーが認証されていません」、「ユーザーが認証されました」）

**トリガー：** [getSelectedProvider （） ](#getSelProv)

</br>

[トップへ戻る](#top)

</br>

### コールバックエラーコード {#callback-error-codes}

| 汎用エラー | |
|:--- | :--- |
| 内部エラー | リクエストの処理中にシステムエラーが発生しました。 |
| Provider not Selected エラー | お客様がプロバイダー選択ダイアログでキャンセルすると発生します |
| Provider not Available エラー | プロバイダーが利用できない場合に発生します。 |

| 認証エラー | |
|:--- | :--- |
| 汎用認証エラー | 理由が不明な場合、または公開できない場合に返されます。 |
| 内部認証エラー | 認証時にシステムエラーが発生しました。 |
| User Not Authenticated Error | ユーザーは認証されていません。 |
| 複数の認証要求エラー | 最初の認証リクエストが完了する前に、追加の認証リクエストを受信しました。 |

| 認証エラー | |
|:--- | :--- |
| 汎用承認エラー | 理由が不明な場合、または公開できない場合に返されます。 |
| 内部認証エラー | 承認しようとしたときにシステムエラーが発生しました。 |
| User not Authorized Error | お客様は、リクエストされたコンテンツを表示する権限がありません。 |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[トップへ戻る](#top)
