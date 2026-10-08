---
title: Android SDKの概要
description: Android SDKの概要
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '2801'
ht-degree: 0%
---
# （レガシー） Android SDKの概要 {#android-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#intro}

Android AccessEnablerは、モバイルアプリがAdobe Pass Authentication for TV Everywhereのエンタイトルメントサービスを利用できるようにするJava Android ライブラリです。 Androidの実装は、使用権限APIを定義するAccessEnabler インターフェイスと、ライブラリがトリガーするコールバックを記述するEntitlementDelegate プロトコルで構成されます。 プロトコルと共にインターフェイスは、AccessEnabler Android ライブラリという1つの共通名で参照されます。

## Androidの要件 {#reqs}

Android プラットフォームとAdobe Pass認証に関連する現在の技術要件については、[Platform / Device / Tool Requirements](#android)を参照するか、Android SDKのダウンロードに含まれるリリースノートを参照してください。

## ネイティブクライアントワークフローについて {#native_client_workflows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass Authentication クライアントと同じか、非常に似ています。 ただし、以下に示すように、いくつかの例外があります。

- [初期化後のワークフロー](#post-init)
- [汎用の初期認証ワークフロー](#generic)
- [ログアウトワークフロー](#logout)

### 初期化後のワークフロー {#post-init}

AccessEnablerでサポートされているすべての資格ワークフローでは、以前に[`setRequestor()`](#setRequestor)に電話してIDを確立したことがあることを前提としています。 この呼び出しを行うと、通常はアプリケーションの初期化/セットアップ段階で、リクエスト者IDを1回だけ提供できます。


ネイティブクライアント（Androidなど）では、[`setRequestor()`](#setRequestor)への初回呼び出し後、次の手順を実行する方法を選択できます。

- すぐに使用権限の呼び出しを開始し、必要に応じてサイレントでキューに入れるようにすることができます。

- または、setRequestorComplete （） コールバックを実装することで、[`setRequestor()`](#setRequestor)の成功/失敗の確認を受け取ることができます。

- または、両方とも実行します。

[`setRequestor()`](#setRequestor)の成功の通知を待つか、AccessEnablerの呼び出しキューメカニズムに依存するかは、お客様次第です。 後続のすべての認証および認証要求には要求者IDと関連する設定情報が必要なため、[`setRequestor()`](#setRequestor) メソッドは、初期化が完了するまで、すべての認証および認証API呼び出しを効果的にブロックします。

### 汎用の初期認証ワークフロー {#generic}

このワークフローの目的は、ユーザーにMVPDでログインすることです。  バックエンドサーバーは、ログインが成功すると、ユーザーに認証トークンを発行します。 認証は通常、認証プロセスの一部として行われますが、次の手順では、認証が単独で動作する方法を説明し、認証手順は含まれていません。

次のネイティブクライアントワークフローは、一般的なブラウザーベースの認証ワークフローとは異なりますが、手順1～5はネイティブクライアントとブラウザーベースのクライアントの両方で同じです。

1. ページまたはプレーヤーは、[getAuthentication （） ](#getAuthN)への呼び出しで認証ワークフローを開始し、有効なキャッシュ済み認証トークンを確認します。 このメソッドにはオプションの`redirectURL` パラメーターがあります。`redirectURL`の値を指定しない場合、認証が成功すると、認証が初期化されたURLにユーザーが返されます。
1. AccessEnablerは、現在の認証ステータスを決定します。 ユーザーが現在認証されている場合、AccessEnablerは`setAuthenticationStatus()` コールバック関数を呼び出し、成功を示す認証ステータスを渡します（以下の手順7）。
1. ユーザーが認証されていない場合、AccessEnablerは、特定のMVPDでユーザーの最後の認証試行が成功したかどうかを判断することで、認証フローを続行します。 MVPD IDがキャッシュされ、`canAuthenticate` フラグがtrueであるか、[`setSelectedProvider()`](#setSelectedProvider)を使用してMVPDが選択された場合、MVPDの選択ダイアログでユーザーにメッセージが表示されません。 認証フローは、MVPDのキャッシュされた値（つまり、最後に成功した認証時に使用したのと同じMVPD）を使用して続行されます。 バックエンドサーバーにネットワーク呼び出しが行われ、ユーザーはMVPD ログインページにリダイレクトされます（以下の手順6）。
1. MVPD IDがキャッシュされておらず、[`setSelectedProvider()`](#setSelectedProvider)を使用してMVPDが選択されていないか、`canAuthenticate` フラグがfalseに設定されている場合、[`displayProviderDialog()`](#displayProviderDialog) コールバックが呼び出されます。 このコールバックは、ページまたはプレーヤーに、選択するMVPDのリストをユーザーに表示するUIを作成するように指示します。 MVPD セレクターの構築に必要な情報を含む、MVPD オブジェクトの配列が提供されます。 各MVPD オブジェクトは、MVPD エンティティを表し、MVPDのID （XFINITY、AT\&amp;Tなど）などの情報を含みます。 MVPDロゴが見つかるURLです。
1. 特定のMVPDを選択したら、ページまたはプレーヤーは、ユーザーの選択をAccessEnablerに通知する必要があります。 Flash以外のクライアントの場合、ユーザーが目的のMVPDを選択したら、[`setSelectedProvider()`](#setSelectedProvider) メソッドの呼び出しを介してAccessEnablerにユーザーの選択を通知します。 代わりに、Flash クライアントはタイプ「`mvpdSelection`」の共有`MVPDEvent`をディスパッチし、選択したプロバイダーを渡します。
1. Android アプリケーションの場合、com.android.chromeが使用可能な場合、認証URLはChrome カスタムタブに読み込まれます。
1. Chromeのカスタムタブを使用して、MVPDのログインページにアクセスし、資格情報を入力します。 この転送中に複数のリダイレクト操作が発生することに注意してください。
1. Chrome Custom Tabsで、URLがスキーム（adobepass://）とリソース「redirect\_uri」（つまり、adobepass://com.adobepass）のディープリンクに一致していることが検出されると、AccessEnablerはバックエンドサーバーから実際の認証トークンを取得します。 最後のリダイレクト URLは実際には無効であり、Chrome カスタムタブが実際に読み込むことを目的としていないことに注意してください。 SDKは、認証フローが完了したことを示すシグナルとしてのみ解釈する必要があります。
1. AccessEnablerは、認証フローが完了したことをアプリケーションに通知します。 AccessEnablerは[`setAuthenticationStatus()`](#setAuthNStatus) コールバックをステータスコード 1で呼び出し、成功を示します。 これらの手順の実行中にエラーが発生した場合、[`setAuthenticationStatus()`](#setAuthNStatus) コールバックはステータスコード 0と対応するエラーコードでトリガーされ、認証失敗を示します。

### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは上記の認証プロセスと同様に処理されます。 このパターンに従って、AccessEnablerはChrome Custom Tabsを開き、バックエンドサーバーにログアウトエンドポイントのURLを読み込みます。



**メモ：** 1つのプログラマー/MVPD セッションからログアウトすると消去されます
Experience Platform ポートフォリオ全体を含む、特定のMVPDの
sso onを通じて取得されたその他のプログラマー認証トークン
そのデバイスは。 他のMVPD用に取得されたトークン、またはSSOを通さずに取得されたトークンは
削除されます。


## トークン {#tokens}

- [定義と用途](#definitions)
- [キャッシュのガイドライン](#caching)
- [永続性](#persistence)
- [書式設定](#format)
- [デバイスのバインディング](#device_binding)



### 定義と用途 {#definitions}

Adobe Pass Authenticationの使用権限ソリューションは、認証ワークフローと認証ワークフローが正常に完了した際にAdobe Pass Authenticationが生成する特定のデータ（トークン）の生成を中心に展開されます。 これらのトークンは、クライアントのAndroid デバイスにローカルに保存されます。

トークンの有効期間は限られています。有効期限が切れると、認証ワークフローや承認ワークフローの再開始を通じてトークンを再発行する必要があります。

エンタイトルメントワークフロー中に発行されるトークンには、次の3つのタイプがあります。

- **認証トークン** - ユーザー認証ワークフローの最終結果は、AccessEnablerがユーザーの代理で認証クエリを実行するために使用できる認証GUIDになります。 この認証GUIDには、ユーザーの認証セッション自体とは異なる場合がある、関連する有効期間（TTL）値が設定されます。 Adobe Pass Authenticationは、認証GUIDを認証リクエストを開始するデバイスにバインドすることにより、認証トークンを生成します。
- **認証トークン** – 一意の`resourceID`によって特定の保護されたリソースへのアクセス権を付与します。 これは、元の`resourceID`と共に承認者によって発行された承認付与で構成されます。 この情報は、リクエストを開始するデバイスにバインドされます。
- **短期間有効なメディアトークン** - AccessEnablerは、短期間有効なメディアトークンを返すことにより、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、特定のリソースに対して以前に取得した認証トークンにもとづいて生成されます。 また、このトークンはデバイスにバインドされておらず、関連する寿命は大幅に短くなります（デフォルト：5分）。

認証と認証が成功すると、Adobe Pass Authenticationは認証、認証、短期間有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイスにキャッシュされ、関連するライフスパンの期間にわたって使用する必要があります。



### キャッシュのガイドライン {#caching}

- 認証トークン
- 認証トークン
- メディアトークン


#### 認証トークン

- **AccessEnabler 1.6以前** – 認証トークンがデバイスにキャッシュされる方法は、現在のMVPDに関連付けられている「**リクエスト者ごとの認証」** フラグによって異なります。


1. 「要求者ごとの認証」機能が&#x200B;*無効*&#x200B;の場合、単一の認証トークンがグローバル ペーストボードにローカルに保存されます。 このトークンは、現在のMVPDと統合されているすべてのアプリケーション間で共有されます。
1. 「要求者ごとの認証」機能が&#x200B;*有効*&#x200B;の場合、トークンは認証フローを実行したプログラマーに明示的に関連付けられます（トークンはグローバルペーストボードには保存されず、そのプログラマーのアプリケーションにのみ表示されるプライベートファイルに保存されます）。 具体的には、異なるアプリケーション間のシングルサインオン（SSO）が無効になります。新しいアプリに切り替える際には、認証フローを明示的に実行する必要があります（2番目のアプリのプログラマーが現在のMVPDと統合され、そのプログラマーの認証トークンがローカルキャッシュに存在しないことを条件とします）。

   **注：** AE 1.6 Google GSON テクニカルノート：[Gsonの依存関係を解決する方法](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** – このSDKでは、複数のProgrammer-MVPD バケット、つまり複数の認証トークンを有効にする、トークン ストレージの新しい方法が導入されています。 AE 1.7から、「要求者ごとの認証」シナリオと通常の認証フローの両方に同じストレージレイアウトが使用されます。 2つの違いは、認証が実行される方法だけです。「要求者ごとの認証」には、ストレージ内の認証トークンの存在（別のプログラマーの場合）に基づいて、AccessEnablerがバックチャネル認証を実行できるようにする新しい改善（パッシブ認証）が含まれています。 ユーザーは1回だけ認証する必要があり、このセッションは後続のアプリで認証トークンを取得するために使用されます。 このバックチャネルフローは[`setRequestor()`](#setRequestor)呼び出し中に発生し、ほとんどがプログラマーに対して透過的です。 ただし、ここで重要な要件が1つあります。プログラマーは、メイン UI スレッドとアクティビティ内から[`setRequestor()`](#setRequestor)を呼び出す必要があります。


#### 認証トークン

常に、リソースごとに1つの認証トークンのみがAccessEnablerによってキャッシュされます。 複数の認証トークンをキャッシュすることもできますが、それらは異なるリソースに関連付けられています。 新しい認証トークンが発行され、同じリソースに古い認証トークンがすでに存在する場合、新しいトークンは既存のキャッシュ値を上書きします。



#### メディアトークン

短期間有効なメディアトークンは、まったくキャッシュしないでください。 メディアトークンは、1回限りの使用に制限されているため、認証APIが呼び出されるたびにサーバーから取得する必要があります。



### 永続性 {#persistence}

トークンは、同じアプリケーションの連続した実行全体で永続的である必要があります。 つまり、認証トークンと認証トークンが取得され、ユーザーがアプリケーションを閉じると、ユーザーがアプリケーションを再度開いたときに同じトークンがアプリケーションで使用できるようになります。 さらに、これらのトークンは複数のアプリケーションにわたって永続的であることが望ましい。 つまり、ユーザーが1つのアプリケーションを使用して特定のID プロバイダーでログインした後（認証トークンと認証トークンを正常に取得した後）、同じID プロバイダーを介してログインする際に、同じトークンを別のアプリケーションで使用でき、同じID プロバイダーを介してログインする際に資格情報の入力を求めるメッセージが表示されなくなります。



このタイプのシームレスな認証/認証ワークフローが、Adobe Pass Authentication ソリューションを真のTV-Everywhereの実装にするものです。 純粋なエンジニアリングの観点から、Android AccessEnabler ライブラリは、トークンデータを外部ストレージにあるデータベースファイルに保存することで、アプリケーション間のデータ共有の問題を回避します。 このシステムレベルの共有リソースは、目的の永続トークンのユースケースの実装を可能にする主な要素を提供します。

- 構造化ストレージのサポート - Adobe Pass認証トークンストレージは、単純な線形バッファーのようなメモリ構造ではありません。 ユーザーが指定したキー値に基づいてデータインデックスを作成できる、ディクショナリのようなストレージメカニズムを提供します。
- 基礎となるファイルシステムを使用したデータ永続性のサポート – データベースファイルの内容はデフォルトで保持され、データはデバイスの外部メモリに保存されます。



特定のトークンがトークンキャッシュに配置されると、その有効性はAccessEnabler ライブラリによって異なる時間にチェックされます。  有効なトークンは、次のように定義されます。

- トークンのTTLが期限切れになっていません
- トークンの発行者は、許可されたID プロバイダーのリストに含まれます



AccessEnabler 1.7以降、トークン ストレージは、複数の認証トークンを保持できるマルチレベルのネストされたマップ構造に依存して、複数のプログラマーとMVPDの組み合わせをサポートできます。 この新しいストレージは、AccessEnabler パブリック APIに影響を与えることはなく、プログラマー側での変更は必要ありません。 この新しい機能を示す例を次に示します。

1. Open App1 （Programmer1によって開発）。
1. MVPD1 （Programmer1と統合）で認証します。
1. 現在のアプリケーションを一時停止/終了し、App2 （Programmer2によって開発）を開きます。
1. Programmer2はMVPD2と統合されていないと仮定します。したがって、ユーザーはApp2で認証されません。
1. App2でMVPD2 （Programmer2と統合）を使用して認証します。
1. App1に切り替えます。ユーザーはProgrammer1で認証されます。

以前のバージョンのAccessEnablerでは、トークン ストレージでサポートされているのは1つの認証トークンのみであったため、ステップ 6では認証されていないユーザーとして表示されます。


**メモ：** 1つのプログラマー/MVPD セッションからログアウトすると、SSOを使用するデバイス上の他のすべてのプログラマー認証トークンを含む、基礎となるストレージがクリアされます。 SSOを通じて他のMVPDに対して取得されたトークンや、そうでないトークンは削除されません。 認証フローをキャンセルする（[`setSelectedProvider(null)`](#setSelectedProvider)を呼び出す）と、基になるストレージは消去されませんが、現在のプログラマー/MVPD認証試行（現在のプログラマーのMVPDを消去することで）にのみ影響します。


AccessEnabler 1.7に含まれているもう1つのストレージ関連の機能により、古いストレージ領域から認証トークンを読み込むことができます。 この「トークンインポーター」は、ストレージのバージョンがアップグレードされた場合でもSSO状態を維持しながら、連続したAccessEnabler リリース間の互換性を実現するのに役立ちます。

インポーターは[`setRequestor()`](#setRequestor) フロー中に実行され、次の2つのシナリオで実行されます（現在のプログラマーに有効な認証トークンが現在のストレージに存在しないと仮定します）。

- 特定のプログラマーによって開発された1.7 アプリの最初のインストール
- 新しいストレージを使用する将来のAccessEnablerへのパスのアップグレード

読み込み操作はプログラマに対して透過的であり、クライアントアプリケーションでコードを変更する必要はありません。



### 書式設定 {#format}

- [認証トークン](#authn_token)
- [認証トークン](#authz_token)
- [ショートメディアトークン](#short_media_token)
- [デバイスのバインディング](#device_binding)

#### 認証トークン {#authn_token}

以下のリストは、認証トークンの形式を示しています。

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


#### ショートメディアトークン {#short_media_token}

以下のリストは、ショートメディアトークンの形式を示しています。  このトークンはプログラマのアプリケーションに公開されます。  プログラムの使用権限プロセスが正常に終了すると、プログラムのアプリケーションに渡されます。

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


#### デバイスのバインディング {#device_binding}

上記のXML リストで、`simpleTokenFingerprint`というタイトルのタグに注意してください。 このタグの目的は、ネイティブデバイス IDの個別化情報を保持することです。 AccessEnabler ライブラリは、このような個別化情報を取得し、使用権限の呼び出し中にAdobe Pass Authentication サービスで利用できるようにします。 この情報を使用して実際のトークンに埋め込むことで、トークンを特定のデバイスに効果的にバインドできます。 最終的な目標は、デバイス間でトークンを転送できないようにすることです。



上記のXML リストで、simpleTokenFingerprintというタグに注意してください。 このタグの目的は、ネイティブデバイス IDの個別化情報を保持することです。 AccessEnabler ライブラリは、このような個別化情報を取得し、使用権限の呼び出し中にAdobe Pass Authentication サービスで利用できるようにします。 この情報を使用して実際のトークンに埋め込むことで、トークンを特定のデバイスに効果的にバインドできます。 最終的な目標は、デバイス間でトークンを転送できないようにすることです。



これは明らかにセキュリティ関連の機能なので、この情報は本質的にセキュリティの観点から「機密」です。 その結果、この情報は改ざんと盗聴の両方から保護される必要があります。 盗聴の問題は、認証/認証要求をHTTPS プロトコル経由で送信することで解決されます。 改ざん防止は、デバイス識別情報にデジタル署名することによって処理されます。 AccessEnabler ライブラリは、デバイスから提供された情報からデバイス IDを計算し、デバイス IDをリクエストパラメーターとしてAdobe Pass Authentication サーバーに「クリア」で送信します。  Adobe Pass認証サーバーは、Adobeの秘密鍵を使用してデバイス IDにデジタル署名し、AccessEnablerに返される認証トークンに追加します。 したがって、デバイス IDは認証トークンにバインドされる。  認証フロー中、AccessEnablerは認証トークンと共にデバイス IDをクリアに再度送信します。  検証プロセスが失敗すると、認証/承認ワークフローが自動的に失敗します。  Adobe Pass認証サーバーは、デバイス IDに秘密鍵を適用し、認証トークンの値と比較します。  それらが一致しない場合、その資格フローは失敗します。


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
