---
title: Android SDK API リファレンス
description: Android SDK API リファレンス
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '4518'
ht-degree: 0%

---

# Android SDK API リファレンス {#android-sdk-api-reference}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#intro}

このドキュメントでは、Android SDK がAdobe Pass認証用に公開する、Adobe Pass認証バージョン 1.7 以降でサポートされるメソッドとコールバックについて詳しく説明します。 ここで説明するメソッドとコールバック関数は、 AccessEnabler.h および EntitlementDelegate.h ヘッダーファイルで定義されています。

詳しくは、 [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) （最新の Android AccessEnabler SDK の場合）


**注意：** Adobe Pass Authentication チームでは、Adobe Pass Authentication のみを使用することを推奨しています *公開* API:

- パブリック API を利用できます。 *そして完全にテスト済みで* サポートされるすべてのクライアントタイプで使用できます。 公開機能の場合は、各クライアントタイプに対応するバージョンの関連メソッドがあることを確認します。</span>
- 後方互換性をサポートし、パートナーの統合が中断しないようにするには、パブリック API は、できる限り安定している必要があります。 ただし、 *non* — パブリック API は、将来の任意の時点で署名を変更する権利を留保します。 現在のパブリックAdobe Pass認証 API 呼び出しの組み合わせを通じてサポートできない特定のフローが発生した場合の最善の方法は、お知らせすることです。 アドビでは、お客様のニーズを考慮に入れて、パブリック API を変更し、今後も安定したソリューションを提供できます。

## Android API {#api}

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
- 事前に許可
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

**説明：** Access Enabler オブジェクトをインスタンス化します。 アプリケーションインスタンスごとに 1 つの Access Enabler インスタンスが必要です。

| API 呼び出し：コンストラクター |
| --- |
| **公開静的** AccessEnabler **getInstance**(Context appContext、String softwareStatement、String redirectUrl)<br>        **スロー** AccessEnablerException <br><br>**公開静的** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**スロー** AccessEnablerException |

**可用性：** v3.1.2 以降

**パラメーター：**

- *appContext*:Android アプリケーションコンテキスト。
- env\_url:Adobeのステージング環境を使用したテストの場合、env\_url を「sp.auth-staging.adobe.com」に設定できます。

**廃止：**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**説明：** プログラマーの ID を確立します。 各プログラマーは、Adobe Pass認証システムのAdobeに登録する際に、一意の ID を割り当てられます。 SSO とリモートトークンを扱う場合、アプリケーションがバックグラウンドになっているときに認証状態が変わる可能性があります。システム状態と同期するために、アプリケーションがフォアグラウンドに移行したときに setRequestor を再び呼び出すことができます。

サーバ応答には、MVPD のリストと、プログラマの ID に付加されるいくつかの設定情報が含まれます。 サーバの応答は、Access Enabler コードによって内部的に使用されます。 操作のステータス (SUCCESS/FAIL) のみが、setRequestorComplete() コールバックを介してアプリケーションに表示されます。

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

- *requestorID*：プログラマーに関連付けられた一意の ID。 Adobe Pass Authentication サービスに初めて登録したときに、Adobeによって割り当てられた一意の ID をサイトに渡します。

- *signedRequestorID*：秘密鍵でデジタル署名された要求者 ID のコピー。 <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *url*：オプションのパラメーター。デフォルトでは、Adobe サービスプロバイダーが使用されます (http://sp.auth.adobe.com/)。 この配列を使用すると、Adobeが提供する認証サービスと認証サービスのエンドポイントを指定できます（デバッグ目的で異なるインスタンスが使用される場合があります）。 これを使用して、複数のAdobe Pass Authentication サービスプロバイダーインスタンスを指定できます。 その際、MVPD リストは、すべてのサービスプロバイダーのエンドポイントで構成されます。 各 MVPD は、最速のサービスプロバイダ、つまり最初に応答し、その MVPD をサポートするプロバイダに関連付けられます。

**コールバックがトリガーされました：** `setRequestorComplete()`

廃止：

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList)&lt;string> url)

[Android API に戻る…](#api)

### setRequestorComplete {#setRequestorComplete}

**説明：** 構成フェーズが完了したことをアプリケーションに通知する Access Enabler によってトリガーされるコールバック。 これは、アプリがエンタイトルメントリクエストの発行を開始できることを示すシグナルです。 設定フェーズが完了するまで、アプリケーションはエンタイトルメントリクエストを発行できません。

| コールバック：リクエスト元の設定が完了しました |
| --- |
| java public void setRequestorComplete(int status) |

**可用性：** v1.0 以降

**パラメーター：**

- *ステータス*：次のいずれかの値を取ることができます。
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`  — 構成フェーズが正常に完了しました
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR`  — 構成フェーズが失敗しました
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`  — 構成フェーズが正常に完了しました
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR`  — 構成フェーズが失敗しました

**トリガー元：** `setRequestor()`

[Android API に戻る…](#api)

### setOptions {#setOptions}

**説明：** グローバル SDK オプションを設定します。 これは、 **マップ\&lt;string string=&quot;&quot;>** を引数として。 マップの値は、SDK がおこなうすべてのネットワーク呼び出しと共に、サーバーに渡されます。

値は、現在のフロー（認証/承認）とは無関係に、サーバーに渡されます。 値を変更する場合は、このメソッドをいつでも呼び出すことができます。

| API 呼び出し： setOptions |
| --- |
| public void setOptions(HashMap&lt;string string=&quot;&quot;> options) |

**可用性：** v1.9.2 以降

**パラメーター：**

- *options*：マップ&lt;string string=&quot;&quot;> グローバル SDK オプションを含む 現在、次のオプションを使用できます。
   - **applicationProfile**  — この値に基づいてサーバーを設定するために使用できます。
   - **ap_vi** -Experience CloudID(visitorID)。 この値は、後で高度な分析レポートに使用できます。
   - **ap_ai**  — 広告 ID
   - **device_info**  — クライアント情報は、次のように記述します。 [クライアント情報デバイスの接続とアプリケーションを渡す](/help/authentication/passing-client-information-device-connection-and-application.md).

[トップに戻る…](#apis)


### checkAuthentication {#checkAuthN}

**説明：** 認証状態を確認します。 これをおこなうには、ローカルトークンストレージスペースで有効な認証トークンを検索します。 このメソッドを呼び出しても、ネットワーク呼び出しは実行されません。 アプリケーションは、ユーザーの認証状態を問い合わせ、それに応じて UI を更新する（ログイン/ログアウト UI を更新する）ために使用します。 認証ステータスは、 [*setAuthenticationStatus()*](#setAuthNStatus) コールバック。

MVPD が「要求者ごとの認証」機能をサポートしている場合は、複数の認証トークンを 1 台のデバイスに保存できます。  この機能の詳細については、 [キャッシュのガイドライン](#$caching) Android の技術概要の節を参照してください。

| API 呼び出し：認証状態の確認 |
| --- |
| public void checkAuthentication() |

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus()`

[Android API に戻る…](#api)


### getAuthentication {#getAuthN}

**説明：** 完全な認証ワークフローを開始します。 まず、認証状態を確認します。 認証済みでない場合は、認証フロー状態マシンが起動します。

- 最後の認証が成功した場合は、MVPD の選択フェーズがスキップされ、 [*navigateToUrl()*](#navigagteToUrl) コールバックがトリガーされます。 このコールバックを使用して、MVPD のログインページをユーザーに表示する WebView コントロールをインスタンス化します。
- 最後の認証に失敗した場合、またはユーザーが明示的にログアウトした場合、 [*displayProviderDialog()*](#displayProviderDialog) コールバックがトリガーされます。 アプリケーションは、このコールバックを使用して MVPD selection UI を表示します。 また、Access Enabler ライブラリに対して、 [setSelectedProvider()](#setSelectedProvider) メソッド。

ユーザーの資格情報が MVPD ログインページで検証されるので、ユーザーが MVPD のログインページで認証を行う際に実行される複数のリダイレクト操作を監視するには、アプリケーションが必要です。 正しい資格情報が入力されると、WebView コントロールは、 *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* 定数。 この URL は、WebView によって読み込まれるものではありません。 アプリケーションは、この URL を切断し、このイベントをログインフェーズが完了したシグナルとして解釈する必要があります。 次に、認証フローを完了するために、Access Enabler に制御を引き渡す必要があります ( *getAuthenticationToken()* メソッド )。

MVPD が「要求者ごとの認証」機能をサポートしている場合は、複数の認証トークンを 1 つのデバイス（プログラマーごとに 1 つ）に格納できます。  この機能の詳細については、 [キャッシュのガイドライン](#$caching) Android の技術概要の節を参照してください。

最後に、認証ステータスは、 *setAuthenticationStatus()* コールバック。



| API 呼び出し：認証フローを開始します |
| --- |
| public void getAuthentication() |

**可用性：** v1.0 以降

| API 呼び出し：認証フローを開始します |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8 以降

**パラメーター：**

- *forceAuthn*：ユーザーが既に認証されているかどうかに関係なく、認証フローを開始するかどうかを指定するフラグ。
- *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される Map。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。

**コールバックがトリガーされました：** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Android API に戻る…](#api)

### displayProviderDialog {#displayProviderDialog}

**説明** Access Enabler によってトリガーされるコールバック。ユーザーが目的の MVPD を選択できるように、適切な UI 要素をインスタンス化する必要があることをアプリケーションに通知します。 このコールバックは、選択 UI パネルを正しく構築するのに役立つ追加情報と共に、MVPD オブジェクトのリストを提供します（MVPD のロゴを示す URL、わかりやすい表示名など）。

ユーザーが目的の MVPD を選択したら、上位レイヤーアプリケーションは、 *setSelectedProvider()* ユーザーの選択に対応する MVPD の ID を渡す。

>[!NOTE]
>
> 認証フローの中止
> </br></br>
> これは、ユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このシナリオでは、アプリケーションは `setSelectedProvider()` メソッド、渡す *null* をパラメータとして使用し、Access Enabler に認証状態マシンをリセットする機会を与えます。

| コールバック： MVPD selection UI を表示します。 |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**可用性：** v1.0 以降

**パラメーター**:

- *mvpds*：アプリケーションが MVPD 選択 UI 要素の構築に使用できる MVPD 関連情報を保持する MVPD オブジェクトのリスト。

**トリガー元：** `getAuthentication(), getAuthorization()`

[Android API に戻る…](#api)


### setSelectedProvider {#setSelectedProvider}

**説明：** このメソッドは、ユーザーの MVPD 選択を Access Enabler に通知するために、アプリケーションによって呼び出されます。 アプリケーションは、このメソッドを使用して、認証に使用するサービスプロバイダーを選択または変更できます。

選択した MVPD が TempPass MVPD の場合、後で getAuthentication() を呼び出す必要なく、その MVPD で自動的に認証されます。

getAuthentication() メソッドで追加のパラメータが指定されるプロモーション一時パスでは、これは使用できないことに注意してください。

を渡す場合 *null* パラメータとして、Access Enabler は、ユーザーが認証フローをキャンセルした（つまり「戻る」ボタンを押した）と仮定し、認証状態マシンをリセットし、 *setAuthenticationStatus()* コールバック `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` エラーコード。

| API 呼び出し：現在選択されているプロバイダーを設定 |
| --- |
| public void setSelectedProvider(String mvpdId) |

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Android API に戻る…](#api)


### navigateToUrl {#navigagteToUrl}

**廃止：** Android SDK 3.0 以降、 navigateToUrl は、Chrome のカスタムタブがデバイスに存在しない場合にのみ使用されます

**説明：** Access Enabler によってトリガーされるコールバック。ユーザーが資格情報を入力するには、MVPD ログインページを表示する必要があることをアプリケーションに通知します。 Access Enabler は、MVPD ログインページの URL をパラメータとして渡します。 WebView コントロールをインスタンス化し、この URL にリダイレクトするには、アプリケーションが必要です。 また、WebView コントロールによって読み込まれる URL を監視し、 `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` 定数。 この場合、WebView コントロールを閉じるか非表示にするか、または *getAuthenticationToken()* メソッド。 Access Enabler は、バックエンド・サーバから認証トークンを取得し、トークン・ストレージにローカルに保存することで、認証フローを完了します。

>[!WARNING]
>
> **認証フローの中止**  <br>これは、ユーザーが「戻る」ボタンを押す機能を持つポイントです。これは、認証フローの中止と同じです。 このシナリオでは、アプリケーションは _setSelectedProvider()_ メソッドの受け渡し _null_ をパラメータとして追加し、Access Enabler が認証状態マシンをリセットする機会を与える。

| コールバック： MVPD ログインページを表示します。 |
| --- |
| public void navigateToUrl(String url) |

**可用性：** v1.0 以降

**パラメーター：**

- *url*:MVPD のログインページを指す URL

**トリガー元：** `getAuthentication(), setSelectedProvider()`

[Android API に戻る…](#api)


### getAuthenticationToken {#getAuthNToken}

**廃止：** Android SDK 3.0 以降、Chrome の「カスタム」タブが認証に使用されるので、このメソッドはアプリケーションでは使用されなくなりました。

**説明：** バックエンドサーバーから認証トークンをリクエストして、認証フローを完了します。 このメソッドは、MVPD ログインページをホストする WebView コントロールが、 `AccessEnabler.ADOBEPASS_REDIRECT_URL` 定数。

| API 呼び出し：認証トークンの取得 |
| --- |
| public void getAuthenticationToken() |

**可用性：** v1.0 以降

**パラメーター：**

- *cookies*：ターゲットドメインに設定される cookie（リファレンス実装については、SDK のデモアプリケーションを参照してください）。

**コールバックがトリガーされました：** `setAuthenticationStatus()`, `sendTrackingData()`

[Android API に戻る…](#api)


### setAuthenticationStatus {#setAuthNStatus}

**説明：** 認証フローの状態をアプリケーションに通知する Access Enabler によってトリガーされるコールバック。 ユーザーの操作や、その他の予期しないシナリオ（ネットワーク接続の問題など）の結果、このフローが失敗する場所は多数あります。 このコールバックは、アプリケーションに認証フローの成功/失敗ステータスを通知し、必要に応じて失敗理由に関する追加情報も提供します。

| コールバック：認証フローのステータスのレポート |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**可用性：** v1.0 以降

**パラメーター：**

- *ステータス*：次のいずれかの値を取ることができます。
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`  — 認証フローが正常に完了しました
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR`  — 認証フローに失敗しました
- *コード*：エラーの理由。 次の場合 *ステータス* 次に該当 `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`を、 *コード* が空の文字列 ( `AccessEnablerConstants.USER_AUTHENTICATED` 定数 ) です。 失敗した場合、このパラメーターは次のいずれかの値を取ることができます。
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` ：ユーザーは認証されていません。 に応じて、 *checkAuthentication()* ローカルトークンキャッシュに有効な認証トークンがない場合にメソッドを呼び出します。
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler は、上位レイヤ・アプリケーションが渡された後に認証状態マシンをリセットしました。 *null* から `setSelectedProvider()` 認証フローを中止する。  おそらく、ユーザーが認証フローをキャンセルした（つまり、「戻る」ボタンを押した）と思われます。
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR`  — ネットワークが利用できない、またはユーザーが認証フローを明示的にキャンセルしたなどの理由で、認証フローが失敗しました。

**トリガー元：** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Android API に戻る…](#api)


### checkPreauthorizedResources {#checkPreauth}

>**廃止：** Android SDK 3.6 以降、事前認証 API は checkPreauthorizedResources の代わりに、拡張エラーコードを提供しています。

**説明：** このメソッドは、ユーザーが既に特定の保護されたリソースの表示を許可されているかどうかを判断するために、アプリケーションで使用されます。 このメソッドの主な目的は、UI の修飾に使用する情報（例えば、ロックアイコンとロック解除アイコンによるアクセスステータスを示す情報）を取得することです。

| API 呼び出し：現在選択されているプロバイダーを設定 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3 以降

**パラメーター：** The `resources` パラメータは、承認を確認する必要があるリソースの配列です。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、 `getAuthorization()` 呼び出し、つまり、プログラマと MVPD またはメディア RSS フラグメントとの間に確立された値に合意する必要があります。

**コールバックがトリガーされました：** `preauthorizedResources()`

[Android API に戻る…](#api)


### checkPreauthorizedResources {#checkPreauth2}

**廃止：** Android SDK 3.6 以降、事前認証 API は checkPreauthorizedResources の代わりに、拡張エラーコードを提供しています。

**説明：** このメソッドは、ユーザーが既に特定の保護されたリソースの表示を許可されているかどうかを判断するために、アプリケーションで使用されます。 このメソッドの主な目的は、UI の修飾に使用する情報（例えば、ロックアイコンとロック解除アイコンによるアクセスステータスを示す情報）を取得することです。

| API 呼び出し：現在選択されているプロバイダーを設定 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**可用性：** v3.1 以降

**パラメーター：** The `resources` パラメータは、承認を確認する必要があるリソースの配列です。 リスト内の各要素は、リソース ID を表す文字列である必要があります。 リソース ID には、 `getAuthorization()` 呼び出し、つまり、プログラマと MVPD またはメディア RSS フラグメントとの間に確立された値に合意する必要があります。

The `cache` パラメータは、キャッシュされた事前認証応答を使用できるかどうかを指定します。 デフォルトのキャッシュが true の場合、SDK は以前にキャッシュされた応答を返します（可能な場合）。

**コールバックがトリガーされました：** `preauthorizedResources()`

[Android API に戻る…](#api)

### preauthorizedResources {#preauthResources}

**廃止：** Android SDK 3.6 以降、事前認証 API は checkPreauthorizedResources の代わりに、拡張エラーコードを提供しています。 preauthorizedResources コールバックは、新しい API では呼び出されません。


**説明：** checkPreauthorizedResources() によってトリガーされるコールバック。 ユーザーが既に表示を許可しているリソースのリストを提供します。

| API 呼び出し：現在選択されているプロバイダーを設定 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3 以降

**パラメーター：** The `resources` パラメーターは、ユーザーが既に表示を許可されているリソースの配列です。

**トリガー元：** `checkPreauthorizedResources()`

[Android API に戻る…](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**説明：** このメソッドは、アプリケーションが認証ステータスを確認するために使用します。 まず、認証状態を最初に確認します。 認証されていない場合、 *setTokenRequestFailed()* コールバックがトリガーされ、メソッドが終了します。 ユーザーが認証されると、認証フローもトリガーされます。 詳細は、 *getAuthorization()* メソッド。

| API 呼び出し：認証ステータスの確認 |
| --- |
| public void checkAuthorization(String resourceId) |

**可用性：** v1.0 以降

| API 呼び出し：認証ステータスの確認 |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8 以降

**パラメーター：**

- *resourceId*：ユーザーが認証をリクエストするリソースの ID。
- *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される Map。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。

**コールバックがトリガーされました：** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Android API に戻る…](#api)


### <span id="getAuthZ"></span>getAuthorization

**説明：** このメソッドは、アプリケーションで承認フローを開始するために使用されます。 ユーザーがまだ認証されていない場合は、認証フローも開始します。 ユーザーが認証されると、Access Enabler は、認証トークン（有効な認証トークンがローカルトークンキャッシュに存在しない場合）と短時間有効なメディアトークンの要求を発行します。 ショートメディアトークンが取得されると、認証フローは完了と見なされます。 The *setToken()* コールバックがトリガーされ、ショートメディアトークンがアプリケーションのパラメーターとして配信されます。 何らかの理由で認証が失敗した場合、 *tokenRequestFailed()* コールバックがトリガーされ、エラーコードと詳細が提供されます。

| API 呼び出し：認証フローの開始 |
| --- |
| public void getAuthorization(String resourceId) |

**可用性：** v1.0 以降

| API 呼び出し：認証フローの開始 |
| --- |
| public void getAuthorization(String resourceId, Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8 以降

**パラメーター：**

- *resourceId*：ユーザーが認証をリクエストするリソースの ID。
- *データ*:Pay-TV パスサービスに送信されるキーと値のペアで構成される Map。 Adobeは、このデータを使用して、SDK を変更することなく、将来の機能を有効にできます。

**コールバックがトリガーされました：** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **追加のコールバックがトリガーされました**  <br> このメソッドは、次のコールバックをトリガーすることもできます（認証フローも開始する場合）。 *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**注意：可能な限り、getAuthorization() の代わりに checkAuthorization() を使用してください。 getAuthorization() メソッドは、完全な認証フローを開始し（ユーザが認証されていない場合）、プログラマ側で複雑な実装を引き起こす可能性があります。**

[Android API に戻る…](#api)


### setToken {#setToken}

**説明：** 認証フローが正常に完了したことをアプリケーションに通知する、Access Enabler によってトリガーされるコールバック。 短時間のみ有効なメディアトークンは、パラメーターとしても提供されます。

| コールバック：承認フローが正常に完了しました |
| --- |
| public void setToken(String token, String resourceId) |

**可用性：** v1.0 以降

**パラメーター：**

- *トークン*：短時間のみ有効なメディアトークン。
- *resourceId*：認証が取得されたリソース

**トリガー元：** `checkAuthorization()`, `getAuthorization()`


[Android API に戻る…](#api)

### tokenRequestFailed {#tokenRequestFailed}

**説明：** Access Enabler によってトリガーされるコールバック。認証フローが失敗したことを上位層のアプリケーションに通知します。

| コールバック：承認フローに失敗しました |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode、String errorDescription) |

**可用性：** v1.0 以降

**パラメーター：**

- *resourceId*：認証が取得されたリソース
- *errorCode*：失敗シナリオに関連付けられたエラーコードです。 可能な値：
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR`  — ユーザーは、指定されたリソースを承認できませんでした
- *errorDescription*：失敗シナリオに関する追加の詳細。 何らかの理由でこの説明文字列を使用できない場合、Adobe Pass Authentication は空の文字列を送信します **(&quot;&quot;)**.

  この文字列は、MVPD がカスタムエラーメッセージや販売関連メッセージを渡すために使用できます。 例えば、あるリソースに対する承認が購読者によって拒否された場合、MVPD は次のようなメッセージを送信します。「現在、パッケージ内のこのチャネルにアクセスできません。 パッケージをアップグレードする場合は、ここをクリックしてください。」 メッセージは、このコールバックを通じてAdobe Pass Authentication によってプログラマーに渡され、プログラマーは、メッセージを表示または無視するオプションを持ちます。 Adobe Pass認証では、このパラメーターを使用して、エラーの原因となった可能性のある条件を通知することもできます。 例えば、「プロバイダーの認証サービスとの通信中にネットワークエラーが発生しました。」というエラーが表示されます。

**トリガー元：** `checkAuthorization(), getAuthorization()`

[Android API に戻る…](#api)

### ログアウト {#logout}

**説明：** ログアウトフローを開始するには、このメソッドを使用します。 ログアウトは、一連の HTTP リダイレクト操作の結果です。これは、ユーザーがAdobe Pass認証サーバーと MVPD サーバーの両方からログアウトする必要があるためです。 その結果、Access Enabler ライブラリから発行された単純な HTTP リクエストでは、このフローを完了できません。 SDK は、Chrome のカスタムタブを使用して HTTP-redirect 操作を実行します。 このフローは、ユーザーに対して表示され、完了時に閉じられます

| API 呼び出し：ログアウトフローの開始 |
| --- |
| public void logout() |

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：**

- `navigateToUrl()` （3.0 より前の SDK バージョンの場合）
- `setAuthenticationStatus()` （SDK バージョン 3.0 以降）


[Android API に戻る…](#api)


### getSelectedProvider {#getSelectedProvider}

**説明：** このメソッドを使用して、現在選択されているプロバイダーを特定します。

| API 呼び出し：現在選択されている MVPD を特定します。 |
| --- |
| public void getSelectedProvider() |

**可用性：** v1.0 以降

**パラメーター：** なし

**コールバックがトリガーされました：** `selectedProvider()`

[Android API に戻る…](#api)


### <span id="selectedProvider"></span>selectedProvider

**説明：** 現在選択されている MVPD に関する情報をアプリケーションに配信する Access Enabler によってトリガーされるコールバック。

| コールバック：現在選択されている MVPD に関する情報 |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**可用性：** v1.0 以降

**パラメーター：**

- *mvpd*：現在選択されている MVPD に関する情報を含むオブジェクト。

**トリガー元：** `getSelectedProvider()`

[Android API に戻る…](#api)


### getMetadata {#getMetadata}

**説明：** Access Enabler ライブラリでメタデータとして公開されている情報を取得するには、この方法を使用します。 この情報にアクセスするには、複合 MetadataKey オブジェクトを提供します。

| API 呼び出し：メタデータの AccessEnabler に対するクエリ |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**可用性：** v1.0 以降

プログラマーは、次の 2 種類のメタデータを使用できます。

- 静的メタデータ（認証トークン TTL、認証トークン TTL およびデバイス ID）
- ユーザーメタデータ（ユーザー ID や郵便番号などのユーザー固有の情報。認証フローおよび/または認証フロー中に MVPD からユーザーのデバイスに渡されます）

**パラメーター：**

- *metadataKey*：キーと args 変数をカプセル化したデータ構造体です。次のようになります。
   - キーが `METADATA_KEY_USER_META` 引数には name =の SerializableNameValuePair オブジェクトが含まれます。 `METADATA_ARG_USER_META` と値= `[metadata_name]`に値を入力すると、ユーザーのメタデータに対してクエリが実行されます。 使用可能なユーザーメタデータタイプの現在のリスト：
      - `zip`  — 郵便番号

      - `householdID`  — 世帯識別子。 MVPD がサブアカウントをサポートしない場合、これは `userID`.

      - `maxRating`  — ユーザーの親の最大評価

      - `userID`  — ユーザー識別子。 MVPD がサブアカウントをサポートし、ユーザがメインアカウントではない場合、 `userID` 次とは異なる `householdID`.

      - `channelID`  — ユーザーが表示する権限のあるチャネルのリスト
   - キーが `METADATA_KEY_DEVICE_ID` 次に、現在のデバイス id を取得するためのクエリが実行されます。 この機能はデフォルトで無効になっています。有効化と料金については、Adobeにお問い合わせください。
   - キーが `METADATA_KEY_TTL_AUTHZ` 引数には name =の SerializableNameValuePair オブジェクトが含まれます。 `METADATA_ARG_RESOURCE_ID` と値= `[resource_id]`を指定すると、クエリが実行され、指定したリソースに関連付けられた認証トークンの有効期限が取得されます。
   - キーが `METADATA_KEY_TTL_AUTHN` 次に、認証トークンの有効期限を取得するためにクエリが実行されます。



>[!NOTE]
>
>SDK 3.4.0 の場合の定数： `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` は com.adobe.adobepass.accessenabler.api.profile.UserProfileService から入手できます。



>[!NOTE]
>
>プログラマーが利用できる実際のユーザメタデータは、MVPD が利用できるものによって異なります。  新しいメタデータが使用可能になり、Adobe Pass Authentication システムに追加されると、このリストはさらに拡張されます。

**コールバックがトリガーされました：** [`setMetadataStatus()`](#setMetadaStatus)

**詳細情報：** [ユーザーメタデータ](/help/authentication/user-metadata-feature.md)

[Android API に戻る…](#api)

### setMetadataStatus {#setMetadaStatus}

**説明：** Access Enabler によってトリガーされるコールバック。 *getMetadata()* を呼び出します。

| コールバック：メタデータ取得リクエストの結果 |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**可用性：** v1.0 以降

**パラメーター：**

- *key*：メタデータ値が要求されるキーと関連するパラメーターを含む MetadataKey オブジェクト（リファレンス実装についてはデモアプリケーションを参照）。
- *結果*：要求されたメタデータを含む複合オブジェクト。 オブジェクトには次のフィールドがあります。
   - *simpleResult*：認証 TTL、認証 TTL、またはデバイス ID に対して要求がおこなわれた際のメタデータ値を表す文字列。 ユーザーメタデータに対してリクエストが行われた場合、この値は null です。

   - *userMetadataResult*: JSON ユーザーメタデータペイロードの Java 表現を格納したオブジェクト。\
     例：

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

は、次のように Java に変換されます。

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**ユーザーメタデータオブジェクトの実際の構造は次のようになります。**

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

この値は、単純なメタデータ（認証 TTL、認証 TTL、またはデバイス ID）に対して要求がおこなわれた場合は null です。

- *暗号化*：取得したメタデータを暗号化するかどうかを指定するブール値です。 このパラメーターは、ユーザーメタデータ要求でのみ重要です。常に暗号化されていない静的メタデータ（例：認証 TTL）は意味を持ちません。 このパラメータを True に設定した場合、ホワイトリストの秘密鍵を使用して RSA 復号を実行することで、暗号化されていない User Metadata 値を取得するのはプログラマ次第です（要求元 ID の署名に使用されるのと同じ秘密鍵）。 [`setRequestor`](#setRequestor) を呼び出す )。

**トリガー元：** [`getMetadata()`](#getMetadata)

**詳細情報：** [ユーザーメタデータ](/help/authentication/user-metadata-feature.md)


[Android API に戻る…](#api)


### getVersion {#getVersion}

**説明：** このメソッドは、AccessEnabler ライブラリのバージョンを取得するために使用できます。

| API 呼び出し：AccessEnabler のバージョンを取得する |
| --- |
| ```public static String getVersion()``` |


[Android API に戻る…](#api)

</br>

## イベントの追跡 {#tracking}

Access Enablerトリガーには、必ずしも権利付与フローとは関係のない追加のコールバックが含まれます。 という名前のイベント追跡コールバック関数の実装 *sendTrackingData()* はオプションですが、アプリケーションは、特定のイベントを追跡し、認証/承認の試行の成功/失敗の回数などの統計をコンパイルできます。 以下は、 *sendTrackingData()* callback:


### sendTrackingData {#sendTrackingData}

**説明：** 認証/承認フローの完了/失敗など、様々なイベントの発生をアプリケーションに対する Access Enabler シグナリングによってトリガーされるコールバック。 デバイスの種類、Access Enabler のクライアントの種類、およびオペレーティングシステムも、 sendTrackingData() によって報告されます。

>[!WARNING]
>
> デバイスタイプとオペレーティングシステムは、パブリック Java ライブラリ ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) およびユーザーエージェント文字列。 この情報は、運用指標をAdobeのカテゴリに分類する簡単な方法としてのみ提供されますが、誤った結果に対する責任はに与えられません。 適宜、新しい機能を使用してください。


- デバイスタイプに指定できる値：
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Access Enabler のクライアント・タイプに指定可能な値：
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| コールバック：イベントの追跡 |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**可用性：** v1.0 以降

**パラメーター：**

- *イベント*：追跡されているイベント。 次の 3 つのタイプのトラッキングイベントが使用可能です。
   - **authorizationDetection:** 認証トークンリクエストが返されたとき ( イベントタイプは `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** 認証チェックが発生した場合 ( イベントタイプは `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** ユーザーが MVPD 選択フォームで MVPD を選択したとき ( イベントタイプは `EVENT_MVPD_SELECTION`)
- *データ*：レポートされたイベントに関連付けられた追加データ。 このデータは、値のリストの形式で表示されます。

以下に、 *データ*
配列：

- イベントタイプの場合 *`EVENT_AUTHN_DETECTION`:*
   - **0**  — トークンリクエストが成功したか (true/false)、上記が true の場合は次のようになります。
   - **1** - MVPD ID 文字列
   - **2** - GUID （md5 ハッシュ化）
   - **3**  — トークンは既にキャッシュに存在します (true/false)
   - **4**  — デバイスタイプ
   - **5** - Access Enabler クライアント・タイプ
   - **6**  — オペレーティングシステムの種類

- イベントタイプの場合 `EVENT_AUTHZ_DETECTION`
   - **0**  — トークンリクエストが成功したか (true/false)、成功した場合は：
   - **1** - MVPD ID
   - **2** - GUID （md5 ハッシュ化）
   - **3**  — トークンは既にキャッシュに存在します (true/false)
   - **4**  — エラー
   - **5**  — 詳細
   - **6**  — デバイスタイプ
   - **7** - Access Enabler クライアント・タイプ
   - **8**  — オペレーティングシステムの種類

- イベントタイプの場合 `EVENT_MVPD_SELECTION`
   - **0**  — 現在選択されている MVPD の ID
   - **1**  — デバイスタイプ
   - **2** - Access Enabler クライアント・タイプ
   - **3**  — オペレーティングシステムの種類

**トリガー元：** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Android API に戻る…](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
