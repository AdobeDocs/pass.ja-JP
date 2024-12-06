---
title: Amazon FireOS 技術概要
description: Amazon FireOS 技術概要
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2166'
ht-degree: 0%

---

# Amazon FireOS 技術概要 {#amazon-fireos-technical-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

## 概要 {#intro}

Amazon FireOS AccessEnabler は、アプリケーションで使用される AccessEnabler スタブライブラリと、モバイルアプリで TV Everywhere の権利付与サービスにAdobe Pass Authentication を使用できるようにするシステムレベルの Java Android ライブラリの 2 つのコンポーネントで表されます。 Amazon FireOS のAndroid実装は、資格 API を定義する AccessEnabler インターフェイスと、ライブラリがトリガーするコールバックを記述する EntitlementDelegate プロトコルで構成されています。 システムレベルの AccessEnabler Android ライブラリを使用すると、Amazon サービスにアクセスして、プラットフォームレベルでシングル サインオンを有効にできます。

## ネイティブクライアントワークフローについて {#native_client_workflows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass Authentication クライアントと同じか、非常に似ています。 ただし、以下に説明するように、いくつかの例外があります。


### 初期化後のワークフロー {#post-init}

AccessEnabler でサポートされるすべての使用権限ワークフローでは、ID を確立するために以前に [`setRequestor()`](#setRequestor) と呼んだことを前提としています。 この呼び出しを行うと、要求者 ID を 1 回だけ指定できます。通常、アプリケーションの初期化/設定段階で指定します。

ネイティブクライアント（AmazonFireOS など）の場合、[`setRequestor()`](#setRequestor) への最初の呼び出しの後、進め方を選択できます。

- エンタイトルメントの呼び出しを直ちに開始し、必要に応じてサイレントにキューに入れることができます。
- または、setRequestorComplete （） コールバックを実装することで、[`setRequestor()`](#setRequestor) の成功/失敗の確認を受け取ることができます。
- または、両方を実行します。

[`setRequestor()`](#setRequestor) の成功の通知を待つか、AccessEnabler のコール キューメカニズムを使用するかは、ユーザー次第です。 後続のすべての承認および認証リクエストには要求者 ID と関連する設定情報が必要なため、[`setRequestor()`](#setRequestor) の方法は、初期化が完了するまで、すべての認証および承認 API 呼び出しを効果的にブロックします。

### 汎用初期認証ワークフロー {#generic}

このワークフローの目的は、MVPD を使用してユーザーにログインすることです。  ログインに成功すると、バックエンドサーバーはユーザーに認証トークンを発行します。 認証は通常、認証プロセスの一部として行われますが、次に認証が単独で機能する方法を説明します。には認証手順は含まれていません。

次のネイティブクライアントワークフローは、一般的なブラウザーベースの認証ワークフローとは異なりますが、手順 1～5 はネイティブクライアントとブラウザーベースのクライアントで同じです。

1. ページまたはプレーヤーが、[getAuthentication （） ](#getAuthN) を呼び出して認証ワークフローを開始します。これにより、有効なキャッシュ済み認証トークンが確認されます。 このメソッドにはオプションの `redirectURL` パラメーターがあります。`redirectURL` の値を指定しない場合、認証が成功すると、認証が初期化された URL にユーザーが返されます。
1. AccessEnabler は、現在の認証ステータスを決定します。 ユーザーが現在認証されている場合、AccessEnabler は `setAuthenticationStatus()` コールバック関数を呼び出し、成功を示す認証ステータスを渡します（次の手順 7）。
1. ユーザーが認証されていない場合、AccessEnabler は、ユーザーの最後の認証試行が特定の MVPD で成功したかどうかを確認することにより、認証フローを続行します。 MVPD ID がキャッシュされていて、`canAuthenticate` フラグが true であるか、または [`setSelectedProvider()`](#setSelectedProvider) を使用して MVPD が選択されている場合、MVPD 選択ダイアログが表示されません。 認証フローは、MVPD のキャッシュされた値（最後に認証が成功したときに使用されたのと同じ MVPD）を使用して続行されます。 バックエンドサーバーに対してネットワーク呼び出しが実行され、ユーザーが MVPD ログインページにリダイレクトされます（以下の手順 6）。
1. MVPD ID がキャッシュされておらず、MVPD が [`setSelectedProvider()`](#setSelectedProvider) を使用して選択されていない場合、または `canAuthenticate` フラグが false に設定されている場合、[`displayProviderDialog()`](#displayProviderDialog) コールバックが呼び出されます。 このコールバックは、ページまたはプレーヤーに対して、選択できる MVPD のリストをユーザーに提示する UI の作成を指示します。 MVPD セレクターを構築するために必要な情報を含む、MVPD オブジェクトの配列が提供されます。 各 MVPD オブジェクトは MVPD エンティティを記述し、MVPD の ID （XFINITY、AT\&amp;T など）や MVPD ロゴが見つかる URL などの情報を含みます。
1. 特定の MVPD が選択されたら、ページまたはプレーヤーは、ユーザーが選択した AccessEnabler に通知する必要があります。 Flash以外のクライアントの場合、ユーザーが目的の MVPD を選択すると、[`setSelectedProvider()`](#setSelectedProvider) メソッドを呼び出して AccessEnabler に通知します。 代わりに、Flashクライアントは「`mvpdSelection`」タイプの共有 `MVPDEvent` をディスパッチし、選択したプロバイダーを渡します。
1. Amazon アプリケーションの場合、[`navigateToUrl()`](#navigagteToUrl) コールバックは無視されます。 Access Enabler ライブラリを使用すると、ユーザーを認証するための共通の WebView コントロールへのアクセスが容易になります。
1. `WebView` を介して、ユーザーが MVPD のログインページに到達し、資格情報を入力します。 この転送中に複数のリダイレクト操作が発生することに注意してください。
1. WebView は認証を完了すると、閉じて、ユーザーが正常にログインしたことを AccessEnabler に通知し、AccessEnabler はバックエンド・サーバから実際の認証トークンを取得します。 AccessEnabler は、ステータス コードが 1 の [`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、成功を示します。 これらの手順の実行中にエラーが発生した場合、[`setAuthenticationStatus()`](#setAuthNStatus) コールバックは、ステータスコード 0 と対応するエラーコードでトリガーされ、ユーザーが認証されていないことを示します。

### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは、前述の認証プロセスと同様に処理されます。 このパターンに従って、AccessEnabler は `WebView` のコントロールを作成し、バックエンド サーバでログアウト エンドポイントの URL を持つコントロールをロードします。 ログアウトプロセスが完了すると、トークンがクリアされます。

ログアウトフローは、ユーザーが `WebView` ーザーとやり取りする必要がないという点で認証フローと異なることに注意してください。 ログアウトが完了すると、AccessEnabler はステータス コードが 0 の `setAuthenticationStatus()` コールバックを呼び出し、ユーザーが認証されていないことを示します。

## トークン {#tokens}

### 定義と使用方法 {#definitions}

Adobe Pass認証使用権限ソリューションでは、認証および承認ワークフローが正常に完了したときにAdobe Pass認証によって生成される特定のデータ（トークン）を生成します。 これらのトークンは、クライアントのAmazon FireOS デバイス上にローカルで保存されます。

トークンの有効期間には制限があります。有効期限が切れた場合は、認証ワークフローまたは承認ワークフロー（あるいはその両方）を再開することで、トークンを再発行する必要があります。

使用権限ワークフローで発行されるトークンには、次の 3 つのタイプがあります。

- **認証トークン** - ユーザー認証ワークフローの最終結果は、ユーザーの代わりに AccessEnabler が認証クエリを行うために使用できる認証 GUID になります。 この認証 GUID には、ユーザーの認証セッション自体とは異なる可能性のある、関連付けられた有効期間（TTL）値があります。 Adobe Pass Authentication は、認証リクエストを開始するデバイスに認証 GUID をバインドして、認証トークンを生成します。
- **認証トークン** – 一意の `resourceID` で識別される特定の保護されたリソースへのアクセスを許可します。 これは、承認当事者が原 `resourceID` とともに発行する承認付与で構成されています。 この情報は、リクエストを開始するデバイスにバインドされます。
- **短時間のみ有効なメディアトークン** - AccessEnabler は、短時間のみ有効なメディアトークンを返すことで、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、その特定のリソースに対して以前に取得された認証トークンに基づいて生成されます。 また、このトークンはデバイスにバインドされず、関連付けられている寿命が大幅に短くなります（デフォルト：5 分）。

認証と承認に成功すると、Adobe Pass Authentication は認証、承認および短時間のみ有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイスにキャッシュし、関連する存続期間中に使用する必要があります。

### キャッシュガイドライン {#caching}


#### 認証トークン

- **AccessEnabler 1.10.1 for FireOS** は、AccessEnabler for Android 1.9.1 に基づいています。この SDK では、新しいトークン ストレージ方式が導入されており、複数のプログラマ MVPD バケットが可能になるため、複数の認証トークンが可能になります。

#### 認証トークン

AccessEnabler によってキャッシュされるのは、リソースごとに 1 つの認証トークンだけです。 複数の認証トークンがキャッシュされることがありますが、それらは異なるリソースに関連付けられています。 新しい認証トークンが発行され、同じリソースに古いトークンが既に存在する場合、新しいトークンが既存のキャッシュ値を上書きします。

#### メディアトークン

短時間のみ有効なメディアトークンはキャッシュしないでください。 メディアトークンは、1 回限りの使用に制限されているので、認証 API が呼び出されるたびにサーバーから取得する必要があります。

### 永続性 {#persistence}

トークンは、同じアプリケーションを連続して実行しても永続的である必要があります。 つまり、認証トークンと認証トークンが取得され、ユーザーがアプリケーションを閉じると、ユーザーがアプリケーションを再度開いたときに、同じトークンがアプリケーションで使用できるようになります。 さらに、これらのトークンは、複数のアプリケーションにわたって永続的であることが望ましいです。 つまり、ユーザーが 1 つのアプリケーションを使用して特定の ID プロバイダーでログインした後（認証および認証トークンの取得に成功した場合）、別のアプリケーションを通じて同じトークンを使用でき、同じ ID プロバイダーを介してログインする際に、ユーザーが資格情報を要求されなくなりました。

このタイプのシームレスな認証/承認ワークフローにより、Adobe Pass認証ソリューションは真の TV-Everywhere 実装となります。 純粋にエンジニアリングの観点から見ると、Android AccessEnabler ライブラリは、トークン・データを外部ストレージ上に配置されたデータベース・ファイルに保存することで、アプリケーション間のデータ共有の問題に対応します。 このシステムレベルの共有リソースは、目的とする永続的なトークンのユースケースの実装を可能にする、主な構成要素を提供します。

- 構造化ストレージのサポート - Adobe Pass認証トークンストレージは、単純な線形バッファー状のメモリ構造ではありません。 ユーザーが指定したキー値に基づくデータインデックス作成を可能にする、辞書に似たストレージメカニズムを提供します。
- 基になるファイルシステムを使用したデータ永続性のサポート – データベースファイルの内容はデフォルトで保持され、データはデバイスの外部メモリに保存されます。

特定のトークンがトークン・キャッシュに配置されると、その有効性は AccessEnabler ライブラリによって異なる時刻にチェックされます。  有効なトークンは次のように定義されます。

- トークンの TTL が期限切れではありません
- トークンの発行者は、許可された ID プロバイダーのリストに含まれます

トークンストレージは、複数の認証トークンを保持できるマルチレベルのネストされたマップ構造に基づいて、複数の Programmer-MVPD の組み合わせをサポートできます。 この新しいストレージは、AccessEnabler パブリック API に影響を与えず、プログラマ側での変更も必要ありません。 次に、この新しい機能を示す例を示します。

1. App1 （プログラマー 1 が開発）を開きます。
1. MVPD1 （Programmer1 と統合）を使用して認証します。
1. 現在のアプリケーションを中断/閉じて、App2 （Programmer2 が開発）を開きます。
1. Programmer2 が MVPD2 と統合されていないので、ユーザーは App2 で認証されないとします。
1. App2 の MVPD2 （Programmer2 と統合）を使用して認証します。
1. App1 に切り替えます。ユーザーは Programmer1 で認証されます。

### 書式設定 {#format}

#### 認証トークン {#authn_token}

次のリストは、認証トークンの形式を示しています。

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### 認証トークン {#authz_token}

以下のリストは、認証トークンの形式を示しています。

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### 短いメディアトークン {#short_media_token}

以下のリストは、ショートメディアトークンの形式を示しています。  このトークンは、プログラマーのアプリケーションに公開されます。  正常な使用権限プロセスの最後に、プログラマーのアプリケーションに渡されます。

```JSON
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### デバイス バインド {#device_binding}

上記の XML の一覧で、「`simpleTokenFingerprint`」というタグが付いていることに注意してください。 このタグの目的は、ネイティブデバイス ID の個別化情報を保持することです。 AccessEnabler ライブラリでは、これらの個別化情報を取得し、使用権限呼び出し中にAdobe Pass認証サービスで使用できるようにします。 サービスはこの情報を使用して、実際のトークンに埋め込み、トークンを特定のデバイスに効果的にバインドします。 この最終目標は、デバイス間でトークンを転送できないようにすることです。

上記の XML の一覧で、simpleTokenFingerprint という名前のタグに注目してください。 このタグの目的は、ネイティブデバイス ID の個別化情報を保持することです。 AccessEnabler ライブラリでは、これらの個別化情報を取得し、使用権限呼び出し中にAdobe Pass認証サービスで使用できるようにします。 サービスはこの情報を使用して、実際のトークンに埋め込み、トークンを特定のデバイスに効果的にバインドします。 この最終目標は、デバイス間でトークンを転送できないようにすることです。

これは明らかにセキュリティ関連の機能なので、この情報はセキュリティの観点から本質的に「機密」です。 その結果、この情報は改ざんと盗聴の両方から保護する必要があります。 盗聴の問題は、HTTPS プロトコルで認証/承認リクエストを送信することで解決されます。 改ざん防止は、デバイス識別情報にデジタル署名することで処理されます。 AccessEnabler ライブラリは、デバイスから提供された情報からデバイス ID を計算し、そのデバイス ID をリクエスト・パラメータとしてAdobe Pass認証サーバに「クリアに」送信します。  Adobe Pass認証サーバは、Adobeの秘密鍵を使用してデバイス ID にデジタル署名し、AccessEnabler に返される認証トークンに追加します。 したがって、デバイス ID は認証トークンにバインドされます。  認証フロー中に、AccessEnabler はデバイス ID を認証トークンとともにクリアで再度送信します。  検証プロセスが失敗すると、認証/承認ワークフローが自動的に失敗します。  Adobe Pass認証サーバーは、秘密鍵をデバイス ID に適用し、認証トークンの値と比較します。  一致しない場合、使用権限フローは失敗します。
