---
title: Android SDKの概要
description: Android SDKの概要
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '2754'
ht-degree: 0%

---

# （従来の）Android SDKの概要 {#android-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#intro}

Android AccessEnabler は、モバイルアプリが TV Everywhere の権利付与サービスにAdobe Pass Authentication を使用できるようにする、Java Android ライブラリです。 Androidの実装は、エンタイトルメント API を定義する AccessEnabler インターフェイスと、ライブラリがトリガーするコールバックを記述する EntitlementDelegate プロトコルで構成されます。 プロトコルと共にインターフェイスは、共通の名前である AccessEnabler Android ライブラリの下で参照されます。

## Androidの要件 {#reqs}

Android Platform およびAdobe Pass認証に関連する現在の技術要件については、[ プラットフォーム/デバイス/ツールの要件 ](#android) を参照するか、Android SDKのダウンロードに含まれるリリースノートを参照してください。

## ネイティブクライアントワークフローについて {#native_client_workflows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass Authentication クライアントと同じか、非常に似ています。 ただし、以下に説明するように、いくつかの例外があります。

- [初期化後のワークフロー](#post-init)
- [汎用初期認証ワークフロー](#generic)
- [ログアウトワークフロー](#logout)

### 初期化後のワークフロー {#post-init}

AccessEnabler でサポートされるすべての使用権限ワークフローでは、ID を確立するために以前に [`setRequestor()`](#setRequestor) と呼んだことを前提としています。 この呼び出しを行うと、要求者 ID を 1 回だけ指定できます。通常、アプリケーションの初期化/設定段階で指定します。


ネイティブクライアント（Androidなど）の場合、[`setRequestor()`](#setRequestor) への最初の呼び出しの後、進め方を選択できます。

- エンタイトルメントの呼び出しを直ちに開始し、必要に応じてサイレントにキューに入れることができます。

- または、setRequestorComplete （） コールバックを実装することで、[`setRequestor()`](#setRequestor) の成功/失敗の確認を受け取ることができます。

- または、両方を実行します。

[`setRequestor()`](#setRequestor) の成功の通知を待つか、AccessEnabler のコール キューメカニズムを使用するかは、ユーザー次第です。 後続のすべての承認および認証リクエストには要求者 ID と関連する設定情報が必要なため、[`setRequestor()`](#setRequestor) の方法は、初期化が完了するまで、すべての認証および承認 API 呼び出しを効果的にブロックします。

### 汎用初期認証ワークフロー {#generic}

このワークフローの目的は、MVPDを使用してユーザーにログインすることです。  ログインに成功すると、バックエンドサーバーはユーザーに認証トークンを発行します。 認証は通常、認証プロセスの一部として行われますが、次に認証が単独で機能する方法を説明します。には認証手順は含まれていません。

次のネイティブクライアントワークフローは、一般的なブラウザーベースの認証ワークフローとは異なりますが、手順 1～5 はネイティブクライアントとブラウザーベースのクライアントで同じです。

1. ページまたはプレーヤーが、[getAuthentication （） ](#getAuthN) を呼び出して認証ワークフローを開始します。これにより、有効なキャッシュ済み認証トークンが確認されます。 このメソッドにはオプションの `redirectURL` パラメーターがあります。`redirectURL` の値を指定しない場合、認証が成功すると、認証が初期化された URL にユーザーが返されます。
1. AccessEnabler は、現在の認証ステータスを決定します。 ユーザーが現在認証されている場合、AccessEnabler は `setAuthenticationStatus()` コールバック関数を呼び出し、成功を示す認証ステータスを渡します（次の手順 7）。
1. ユーザーが認証されていない場合、AccessEnabler は、ユーザーの最後の認証試行が特定のMVPDで成功したかどうかを確認することにより、認証フローを続行します。 MVPD ID がキャッシュされていて、`canAuthenticate` フラグが true であるか、またはMVPDが [`setSelectedProvider()`](#setSelectedProvider) を使用して選択されている場合、MVPD選択ダイアログが表示されません。 認証フローは、MVPDのキャッシュされた値（最後に成功した認証で使用したMVPDと同じ）を使用して続行されます。 バックエンドサーバーに対してネットワーク呼び出しが実行され、ユーザーがMVPDのログインページにリダイレクトされます（以下の手順 6）。
1. MVPD ID がキャッシュされておらず、[`setSelectedProvider()`](#setSelectedProvider) を使用してMVPDが選択されていない場合、または `canAuthenticate` フラグが false に設定されている場合、[`displayProviderDialog()`](#displayProviderDialog) コールバックが呼び出されます。 このコールバックは、ページまたはプレーヤーに対して、選択できる MVPD のリストをユーザーに提示する UI の作成を指示します。 MVPD セレクターを構築するために必要な情報を含む、MVPD オブジェクトの配列が提供されます。 各MVPD オブジェクトは 1 つのMVPD エンティティを表し、MVPDの ID （XFINITY、AT\&amp;T など）やMVPDのロゴが見つかる URL などの情報を含みます。
1. 特定のMVPDを選択したら、ユーザーが選択したことを AccessEnabler に通知する必要があります。 Flash以外のクライアントの場合は、目的のMVPDを選択すると、[`setSelectedProvider()`](#setSelectedProvider) メソッドを呼び出して AccessEnabler に通知します。 代わりに、Flashクライアントは「`mvpdSelection`」タイプの共有 `MVPDEvent` をディスパッチし、選択したプロバイダーを渡します。
1. Android アプリケーションの場合、com.android.chrome が使用可能であれば、認証 URL はChromeのカスタムタブに読み込まれます。
1. Chromeのカスタムタブを使用して、MVPDのログインページにアクセスし、資格情報を入力します。 この転送中に複数のリダイレクト操作が発生することに注意してください。
1. Chromeのカスタム タブで、URL がスキーム（adobepass://）およびリソース「redirect\_uri」（つまり、adobepass://com.adobepass）からのディープリンクに一致することが検出されると、AccessEnabler はバックエンド サーバーから実際の認証トークンを取得します。 最終的なリダイレクト URL は実際には無効であり、Chromeのカスタムタブで実際に読み込むためのものではないことに注意してください。 これらは、認証フローが完了したことを示すシグナルとしてのみ、SDKで解釈される必要があります。
1. AccessEnabler は、認証フローが完了したことをアプリケーションに通知します。 AccessEnabler は、ステータス コードが 1 の [`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、成功を示します。 これらの手順の実行中にエラーが発生した場合、[`setAuthenticationStatus()`](#setAuthNStatus) コールバックは、ステータスコード 0 と対応するエラーコードでトリガーされ、認証の失敗を示します。

### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは、前述の認証プロセスと同様に処理されます。 このパターンに従って、AccessEnabler はChromeのカスタム タブを開き、バックエンド サーバでログアウト エンドポイントの URL をロードします。



**注意：** あるプログラマー/MVPD セッションからログアウトすると、消去されます
そのMVPDの基盤となるストレージ（以下のすべてを含む）
で SSO を介して取得した他のプログラマー認証トークン
そのデバイス。 他の MVPD 用に取得されたトークン、または SSO を介して取得されなかったトークンは、
削除されます。


## トークン {#tokens}

- [定義と使用方法](#definitions)
- [キャッシュガイドライン](#caching)
- [永続性](#persistence)
- [書式設定](#format)
- [デバイス バインド](#device_binding)



### 定義と使用方法 {#definitions}

Adobe Pass認証使用権限ソリューションでは、認証および承認ワークフローが正常に完了したときにAdobe Pass認証によって生成される特定のデータ（トークン）を生成します。 これらのトークンは、クライアントのAndroid デバイス上にローカルで保存されます。

トークンの有効期間には制限があります。有効期限が切れた場合は、認証ワークフローまたは承認ワークフロー（あるいはその両方）を再開することで、トークンを再発行する必要があります。

使用権限ワークフローで発行されるトークンには、次の 3 つのタイプがあります。

- **認証トークン** - ユーザー認証ワークフローの最終結果は、ユーザーの代わりに AccessEnabler が認証クエリを行うために使用できる認証 GUID になります。 この認証 GUID には、ユーザーの認証セッション自体とは異なる可能性のある、関連付けられた有効期間（TTL）値があります。 Adobe Pass Authentication は、認証リクエストを開始するデバイスに認証 GUID をバインドして、認証トークンを生成します。
- **認証トークン** – 一意の `resourceID` で識別される特定の保護されたリソースへのアクセスを許可します。 これは、承認当事者が原 `resourceID` とともに発行する承認付与で構成されています。 この情報は、リクエストを開始するデバイスにバインドされます。
- **短時間のみ有効なメディアトークン** - AccessEnabler は、短時間のみ有効なメディアトークンを返すことで、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、その特定のリソースに対して以前に取得された認証トークンに基づいて生成されます。 また、このトークンはデバイスにバインドされず、関連付けられている寿命が大幅に短くなります（デフォルト：5 分）。

認証と承認に成功すると、Adobe Pass Authentication は認証、承認および短時間のみ有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイスにキャッシュし、関連する存続期間中に使用する必要があります。



### キャッシュガイドライン {#caching}

- 認証トークン
- 認証トークン
- メディアトークン


#### 認証トークン

- **AccessEnabler 1.6 以前** - デバイス上で認証トークンをキャッシュする方法は、現在のMVPDに関連づけられている「**Authentication per Requestor」** フラグによって異なります。


1. 「要求者ごとの認証」機能が *無効* になっている場合、1 つの認証トークンがグローバルなペーストボードにローカルに保存されます。 このトークンは、現在のMVPDに統合されているすべてのアプリケーション間で共有されます。
1. 「要求者ごとの認証」機能が *有効* になっている場合、トークンは認証フローを実行したプログラマーに明示的に関連付けられます（トークンはグローバルなペーストボードには保存されず、そのプログラマーのアプリケーションにのみ表示できるプライベートファイルに保存されます）。 より具体的には、異なるアプリケーション間でのシングルサインオン（SSO）は無効になります。ユーザーは、新しいアプリに切り替える際に認証フローを明示的に実行する必要があります（2 番目のアプリのプログラマーが現在のMVPDと統合されており、そのプログラマーの認証トークンがローカルキャッシュに存在しない場合）。

   **メモ：** AE 1.6 Google GSON テクニカルノート：[Gson 依存関係の解決方法 ](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** – このSDKでは、新しいトークン・ストレージ方式が導入されており、複数のプログラマーMVPD・バケットを使用できるため、複数の認証トークンを使用できます。 AE 1.7 以降では、「リクエスターごとの認証」シナリオと通常の認証フローの両方で同じストレージレイアウトが使用されます。 この 2 つの違いは、認証の実行方法にあります。「要求者ごとの認証」には、ストレージ内に（異なるプログラマの）認証トークンが存在することに基づいて、AccessEnabler がバックチャネル認証を実行できるようにする新しい改善点（パッシブ認証）が含まれています。 ユーザーは 1 回だけ認証する必要があり、このセッションは後続のアプリで認証トークンを取得するために使用されます。 このバックチャネルフローは、[`setRequestor()`](#setRequestor) 呼び出し時に発生し、プログラマーに対してほとんど透過的です。 ただし、ここで重要な要件が 1 つあります。プログラマーは、メイン UI スレッドおよびアクティビティ内から [`setRequestor()`](#setRequestor) を呼び出す必要があります。


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



AccessEnabler 1.7 以降、トークン・ストレージは、複数の認証トークンを保持できるマルチレベルのネストされたマップ構造に基づいて、複数の Programmer-MVPDの組み合わせをサポートできます。 この新しいストレージは、AccessEnabler パブリック API に影響を与えず、プログラマ側での変更も必要ありません。 次に、この新しい機能を示す例を示します。

1. App1 （プログラマー 1 が開発）を開きます。
1. （Programmer1 と統合された）MVPD1 を使用して認証します。
1. 現在のアプリケーションを中断/閉じて、App2 （Programmer2 が開発）を開きます。
1. Programmer2 がMVPD2 と統合されていないので、App2 では認証されないとします。
1. App2 でMVPD2 （Programmer2 と統合）を使用して認証します。
1. App1 に切り替えます。ユーザーは Programmer1 で認証されます。

以前のバージョンの AccessEnabler では、トークン・ストレージでサポートされていた認証トークンは 1 つのみのため、ステップ 6 ではユーザーが未認証と表示されていました。


**注意：** あるプログラマー/MVPD セッションからログアウトすると、基になるストレージ（SSO を使用しているデバイス上の他のすべてのプログラマー認証トークンを含む）がクリアされます。 他の MVPD 用に取得されたトークン、または SSO を介して取得されなかったトークンは削除されません。 認証フローを取り消す（[`setSelectedProvider(null)`](#setSelectedProvider) を呼び出す）と、基になるストレージはクリアされませんが、現在のプログラマー/MVPDの認証の試行にのみ影響します（現在のプログラマーのMVPDを消去することで）。


AccessEnabler 1.7 に含まれるもう 1 つのストレージ関連機能により、古いストレージ領域から認証トークンをインポートできます。 この「トークン インポーター」は、連続する AccessEnabler のリリース間の互換性を実現し、ストレージ バージョンがアップグレードされた場合でも SSO 状態を維持するのに役立ちます。

インポーターは、[`setRequestor()`](#setRequestor) フロー中に実行され、次の 2 つのシナリオで実行されます（現在のプログラマーの有効な認証トークンが現在のストレージに存在しないと仮定）。

- 特定のプログラマーによって開発された 1.7 アプリの最初のインストール
- 新しいストレージを使用する将来の AccessEnabler へのアップグレード・パス

読み込み操作はプログラマーに対して透過的で、クライアントアプリケーションでコードを変更する必要はありません。



### 書式設定 {#format}

- [認証トークン](#authn_token)
- [認証トークン](#authz_token)
- [短いメディアトークン](#short_media_token)
- [デバイス バインド](#device_binding)

#### 認証トークン {#authn_token}

次のリストは、認証トークンの形式を示しています。

```XML
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

```XML
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

```XML
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


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
