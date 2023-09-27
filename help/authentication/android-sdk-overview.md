---
title: Android SDK の概要
description: Android SDK の概要
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 0%

---

# Android SDK の概要 {#android-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#intro}

Android AccessEnabler は、モバイルアプリでAdobe Pass Authentication を TV Everywhere の権限付与サービスに使用できる Java Android ライブラリです。 Android の実装は、使用権限 API を定義する AccessEnabler インターフェイスと、ライブラリのトリガー時に使用するコールバックを記述する EntitlementDelegate プロトコルで構成されます。 プロトコルと共に使用されるインターフェイスは、AccessEnabler Android ライブラリという 1 つの共通名で参照されます。

## Android の要件 {#reqs}

Android プラットフォームとAdobe Pass認証に関する現在の技術要件については、 [プラットフォーム/デバイス/ツールの要件](#android)または、Android SDK のダウンロードに含まれるリリースノートを参照してください。

## ネイティブクライアントワークフローについて {#native_client_workflows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass認証クライアントと同じか、非常に似ています。 ただし、以下に説明するように、いくつかの例外があります。

- [初期化後のワークフロー](#post-init)
- [汎用初期認証ワークフロー](#generic)
- [ログアウトワークフロー](#logout)

### 初期化後のワークフロー {#post-init}

AccessEnabler でサポートされるすべての権限付与ワークフローは、以前に [`setRequestor()`](#setRequestor) をクリックして、id を確立します。 この呼び出しを実行して Requestor ID を指定するのは、通常、アプリケーションの初期化/設定フェーズ中に 1 回だけにします。


ネイティブクライアント（Android など）で、への最初の呼び出しの後に [`setRequestor()`](#setRequestor)を使用する場合、次の手順を実行する方法を選択できます。

- エンタイトルメントの呼び出しをすぐに開始し、必要に応じて、静かにキューに入れることができます。

- または、 [`setRequestor()`](#setRequestor) setRequestorComplete() コールバックを実装することによって実行されます。

- または、両方を実行します。

成功の通知を待つかどうかは君次第だ [`setRequestor()`](#setRequestor) AccessEnabler の呼び出しキュー・メカニズムに依存する場合は、 それ以降のすべての認証要求にはリクエスト元 ID と関連する設定情報が必要なので、 [`setRequestor()`](#setRequestor) メソッドは、初期化が完了するまで、すべての認証および承認 API 呼び出しを効果的にブロックします。

### 汎用初期認証ワークフロー {#generic}

このワークフローの目的は、MVPD を使用してユーザーにログインすることです。  ログインが成功した後、バックエンドサーバーはユーザーに認証トークンを発行します。 通常、認証は承認プロセスの一部として行われますが、次に、認証が分離で機能する方法を示します。また、認証手順は含まれていません。

次のネイティブクライアントワークフローは、一般的なブラウザーベースの認証ワークフローとは異なりますが、手順 1 ～ 5 は、ネイティブクライアントとブラウザーベースのクライアントの両方で同じです。

1. ページまたはプレーヤーが、 [getAuthentication()](#getAuthN)：有効なキャッシュ済み認証トークンを確認します。 このメソッドにはオプションの `redirectURL` パラメータ。 `redirectURL`認証が成功すると、ユーザーは認証の初期化に使用した URL に戻ります。
1. AccessEnabler は、現在の認証状態を決定します。 ユーザーが現在認証されている場合、AccessEnabler は、 `setAuthenticationStatus()` コールバック関数に渡し、成功を示す認証ステータスを渡す（手順 7 を参照）。
1. ユーザーが認証されていない場合、AccessEnabler は、特定の MVPD でユーザーの最後の認証試行が成功したかどうかを判断することで、認証フローを継続します。 MVPD ID がキャッシュされ、 `canAuthenticate` フラグが true の場合、またはを使用して MVPD が選択された場合 [`setSelectedProvider()`](#setSelectedProvider)を指定した場合、MVPD 選択ダイアログが表示されることはありません。 認証フローは、MVPD のキャッシュされた値（最後の正常な認証時に使用されたのと同じ MVPD）を引き続き使用します。 バックエンドサーバーに対してネットワーク呼び出しがおこなわれ、ユーザーは MVPD ログインページ（以下の手順 6）にリダイレクトされます。
1. MVPD ID がキャッシュされておらず、を使用して MVPD が選択されていない場合 [`setSelectedProvider()`](#setSelectedProvider) または `canAuthenticate` フラグが false に設定されている場合、 [`displayProviderDialog()`](#displayProviderDialog) コールバックが呼び出されます。 このコールバックは、選択する MVPD のリストをユーザーに表示する UI を作成するようにページまたはプレーヤーに指示します。 MVPD オブジェクトの配列が提供され、MVPD セレクターを構築するために必要な情報が含まれます。 各 MVPD オブジェクトは MVPD エンティティを表し、MVPD の ID（XFINITY、AT\&amp;T など）などの情報を含みます。 MVPD ロゴが見つかる URL。
1. 特定の MVPD を選択したら、ページまたはプレーヤーは、ユーザーが選択した内容を AccessEnabler に通知する必要があります。 非Flashクライアントの場合、ユーザが目的の MVPD を選択したら、AccessEnabler に対し、 [`setSelectedProvider()`](#setSelectedProvider) メソッド。 Flashクライアントが代わりに共有をディスパッチする `MVPDEvent` タイプ&quot;`mvpdSelection`&quot;、選択したプロバイダーを渡します。
1. Android アプリケーションの場合、 com.android.chrome が使用可能な場合、認証 URL は Chrome のカスタムタブに読み込まれます。
1. Chrome カスタムタブを通じて、ユーザーは MVPD のログインページに到達し、その資格情報を入力します。 この転送中に、いくつかのリダイレクト操作が発生することに注意してください。
1. Chrome のカスタムタブで、URL がリソース「redirect\_uri」( adobepass://com.adobepass ) のスキーム ( adobepass:// ) およびディープリンクと一致することが検出されると、AccessEnabler はバックエンドサーバーから実際の認証トークンを取得します。 最終的なリダイレクト URL は、実際には無効です。また、Chrome のカスタムタブで実際に読み込むことを目的としているわけではありません。 これらは、SDK が認証フローが完了したシグナルとしてのみ解釈する必要があります。
1. AccessEnabler は、アプリケーションに認証フローの完了を通知します。 AccessEnabler が [`setAuthenticationStatus()`](#setAuthNStatus) ステータスコードが 1 のコールバック。成功を示します。 これらの手順の実行中にエラーが発生した場合、 [`setAuthenticationStatus()`](#setAuthNStatus) コールバックは、ステータスコード 0 と、対応するエラーコード（認証失敗を示す）と共にトリガーされます。

### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは上記の認証プロセスと同様に処理されます。 このパターンに従って、AccessEnabler は Chrome Custom Tabs を開き、バックエンドサーバ上のログアウトエンドポイントの URL を読み込みます。



**注意：** 1 つのプログラマー/MVPD セッションからログアウトすると、そのデバイス上の SSO を通じて得られた他のすべてのプログラマ認証トークンを含む、その特定の MVPD の基になるストレージがクリアされます。 他の MVPD に対して取得されたトークン、または SSO を介さないトークンは削除されません。


## トークン {#tokens}

- [定義と使用方法](#definitions)
- [キャッシュのガイドライン](#caching)
- [永続性](#persistence)
- [形式](#format)
- [デバイスのバインディング](#device_binding)



### 定義と使用方法 {#definitions}

Adobe Pass Authentication Entitlement ソリューションは、認証および承認ワークフローが正常に完了したときにAdobe Pass Authentication が生成する特定のデータ（トークン）の生成に関連しています。 これらのトークンは、クライアントの Android デバイス上にローカルに保存されます。

トークンの有効期限は限られています。有効期限が切れたら、認証ワークフローや認証ワークフローの再開を通じてトークンを再発行する必要があります。

使用権限付与ワークフローでは、次の 3 種類のトークンが発行されます。

- **認証トークン**  — ユーザー認証ワークフローの最終結果は、AccessEnabler がユーザーに代わって認証クエリを実行する際に使用できる認証 GUID になります。 この認証 GUID には、ユーザーの認証セッション自体とは異なる可能性のある、有効期間 (TTL) 値が関連付けられます。 Adobe Pass認証は、認証要求を開始するデバイスに認証 GUID をバインドすることで、認証トークンを生成します。
- **認証トークン**  — 固有の `resourceID`. 認証者が発行した認証付与と、オリジナルの `resourceID`. この情報は、要求を開始するデバイスに結び付けられます。
- **短時間のみ有効なメディアトークン** - AccessEnabler は、短時間のみ有効なメディアトークンを返すことで、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、その特定のリソースに対して以前に取得された認証トークンに基づいて生成されます。 また、このトークンはデバイスに結び付けられず、関連する寿命が大幅に短くなります（デフォルトは 5 分）。

認証と承認が成功すると、Adobe Pass認証は認証、承認および短時間のみ有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイス上にキャッシュされ、関連するライフスパンの期間中に使用される必要があります。



### キャッシュのガイドライン {#caching}

- 認証トークン
- 認証トークン
- メディアトークン


#### 認証トークン

- **AccessEnabler 1.6 以前**  — 認証トークンがデバイス上でキャッシュされる方法は、**リクエスト元ごとの認証»** 現在の MVPD に関連付けられたフラグ：


1. 「リクエスト元ごとの認証」機能が *無効*&#x200B;を指定した場合、1 つの認証トークンがグローバルペーストボードにローカルに保存されます。 このトークンは、現在の MVPD と統合されているすべてのアプリケーション間で共有されます。
1. 「リクエスト元ごとの認証」機能が *有効*&#x200B;その場合、トークンは、認証フローを実行したプログラマに明示的に関連付けられます（トークンは、グローバルペーストボードには格納されず、そのプログラマのアプリケーションにのみ表示可能なプライベートファイルに格納されます）。 具体的には、異なるアプリケーション間のシングルサインオン (SSO) が無効になります。新しいアプリに切り替える際に、ユーザは認証フローを明示的に実行する必要があります（2 つ目のアプリのプログラマは現在の MVPD と統合され、ローカルキャッシュに認証トークンが存在しない）。

   **注意：** AE 1.6 Google GSON テクニカルノート： [Gson の依存関係を解決する方法](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7**  — この SDK は、複数の Programmer-MVPD バケット、つまり複数の認証トークンを有効にする、新しいトークンストレージメソッドを導入します。 AEM 1.7 以降では、「リクエスト元ごとの認証」シナリオと通常の認証フローの両方に同じストレージレイアウトが使用されます。 この 2 つの違いは、認証の実行方法です。「要求元ごとの認証」には、ストレージ内の認証トークン（別のプログラマ用）の存在に基づいて、AccessEnabler がバックチャネル認証を実行できる新しい改善（パッシブ認証）が含まれます。 ユーザーは 1 回だけ認証を行う必要があり、このセッションは後続のアプリで認証トークンを取得するために使用されます。 このバックチャネルフローは、 [`setRequestor()`](#setRequestor) を呼び出し、はほとんどプログラマに対して透過的である。 しかし、ここで重要な要件は、プログラマが MUST コールを呼び出すことです。 [`setRequestor()`](#setRequestor) をメイン UI スレッドから、またはアクティビティ内から呼び出します。


#### 認証トークン

いつでも、AccessEnabler によってキャッシュされるのは、リソースごとに 1 つの認証トークンだけです。 複数の認証トークンをキャッシュすることができますが、異なるリソースに関連付けられます。 新しい認証トークンが発行され、同じリソースに古い認証トークンが既に存在する場合は常に、新しいトークンは既存のキャッシュされた値を上書きします。



#### メディアトークン

短時間のみ有効なメディアトークンは、まったくキャッシュしないでください。 メディアトークンは 1 回限りの使用に制限されているので、認証 API が呼び出されるたびにサーバーから取得する必要があります。



### 永続性 {#persistence}

トークンは、同じアプリケーションの連続した実行の間、保持する必要があります。 つまり、認証および認証トークンが取得され、ユーザーがアプリケーションを閉じると、ユーザーがアプリケーションを再度開いたときに、同じトークンがアプリケーションで使用できるようになります。 さらに、これらのトークンは複数のアプリケーション間で保持することが望ましい。 つまり、ユーザーが 1 つのアプリケーションを使用して特定の ID プロバイダーにログインした後（認証および認証トークンの取得に成功）、同じトークンを別のアプリケーションで使用でき、同じ ID プロバイダー経由でログインする際に資格情報の入力を求められなくなります。



このようなシームレスな認証/承認ワークフローにより、Adobe Pass Authentication Solution が真の TV-Everywhere 実装になります。 純粋なエンジニアリングの観点から、Android AccessEnabler ライブラリは、トークンデータを外部ストレージ上のデータベースファイルに保存することで、アプリケーション間のデータ共有の問題を回避します。 このシステムレベルの共有リソースは、目的の永続的なトークンの使用例を実装するための主要な要素を提供します。

- 構造化ストレージのサポート — Adobe Pass認証トークンストレージは、単なる線形バッファのようなメモリ構造ではありません。 これは、ユーザー指定のキー値に基づいてデータのインデックスを作成できる、辞書に似たストレージメカニズムを提供します。
- 基盤となるファイルシステムを使用したデータ永続化のサポート — データベースファイルの内容はデフォルトで保持され、データはデバイスの外部メモリに保存されます。



特定のトークンがトークン・キャッシュに配置されると、その有効性が AccessEnabler ライブラリによって異なる時間にチェックされます。  有効なトークンは次のように定義されます。

- トークンの TTL が期限切れになっていません
- トークンの発行者が、許可された ID プロバイダーのリストに含まれています



AccessEnabler 1.7 以降、トークンストレージは、複数の認証トークンを保持できる複数レベルのネストされたマップ構造に依存して、複数の Programmer-MVPD の組み合わせをサポートします。 この新しいストレージは、AccessEnabler パブリック API に何ら影響を与えず、プログラマ側の変更を必要としません。 次に、この新しい機能の例を示します。

1. App1 を開きます（Programmer1 が開発）。
1. MVPD1 （Programmer1 と統合）を使用して認証します。
1. 現在のアプリケーションを休止/閉じ、（Programmer2 が開発した）App2 を開きます。
1. Programmer2 が MVPD2 と統合されていないので、ユーザーが App2 で認証されないとします。
1. App2 で MVPD2（Programmer2 と統合）を認証します。
1. App1 に切り替えます。ユーザーは引き続き Programmer1 で認証されます。

以前のバージョンの AccessEnabler では、トークンストレージは以前 1 つの認証トークンのみをサポートしていたので、手順 6 ではユーザーが未認証として表示されました。


**注意：** 1 つのプログラマー/MVPD セッションからログアウトすると、SSO を使用して、デバイス上の他のすべてのプログラマ認証トークンを含む、基になるストレージが消去されます。 他の MVPD に対して取得されたトークン、または SSO を介さないトークンは削除されません。 認証フローのキャンセル（呼び出し） [`setSelectedProvider(null)`](#setSelectedProvider)) は、基になるストレージをクリアしませんが、（現在のプログラマーの MVPD を消去することで）現在のプログラマー/MVPD 認証の試みにのみ影響します。


AccessEnabler 1.7 に含まれる別のストレージ関連機能により、古いストレージ領域から認証トークンをインポートできます。 この「トークンインポーター」は、ストレージバージョンがアップグレードされても SSO 状態を維持し、連続する AccessEnabler リリース間の互換性を実現するのに役立ちます。

インポーターは、 [`setRequestor()`](#setRequestor) フローおよびは、次の 2 つのシナリオで実行されます（現在のプログラマーの有効な認証トークンが現在のストレージに存在しない場合）。

- 特定のプログラマーが開発した 1.7 アプリの最初のインストール
- 新しいストレージを使用する将来の AccessEnabler へのパスのアップグレード

インポート操作はプログラマーに対して透過的で、クライアントアプリケーションでコードを変更する必要はありません。



### 形式 {#format}

- [認証トークン](#authn_token)
- [認証トークン](#authz_token)
- [ショートメディアトークン](#short_media_token)
- [デバイスのバインディング](#device_binding)

#### 認証トークン {#authn_token}

次に、認証トークンの形式を示します。

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

次のリストは、認証トークンの形式を示しています。

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

以下のリストは、ショートメディアトークンの形式を示しています。  このトークンは、プログラマーのアプリケーションに公開されます。  正常なエンタイトルメントプロセスの終了時に、Programmer のアプリケーションに渡されます。

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

上記の XML リストで、というタグをメモします。 `simpleTokenFingerprint`. このタグの目的は、ネイティブデバイス ID の個別化情報を保持することです。 AccessEnabler ライブラリでは、こうした個別化情報を取得し、権限付与の呼び出し時にAdobe Pass Authentication サービスで使用できるようにすることができます。 この情報を使用して実際のトークンに埋め込むので、トークンを特定のデバイスに効果的にバインドします。 この最終目標は、デバイス間でトークンを転送できないようにすることです。



上記の XML リストで、simpleTokenFingerprint というタグをメモします。 このタグの目的は、ネイティブデバイス ID の個別化情報を保持することです。 AccessEnabler ライブラリでは、こうした個別化情報を取得し、権限付与の呼び出し時にAdobe Pass Authentication サービスで使用できるようにすることができます。 この情報を使用して実際のトークンに埋め込むので、トークンを特定のデバイスに効果的にバインドします。 この最終目標は、デバイス間でトークンを転送できないようにすることです。



これは明らかにセキュリティ関連の機能なので、セキュリティの観点から言えば、この情報は本質的に「機密性」が高いものです。 その結果、この情報は改ざんや盗聴の両方から保護される必要があります。 盗聴の問題は、認証/承認リクエストを HTTPS プロトコル経由で送信することで解決されます。 改ざん保護は、デバイス識別情報にデジタル署名することにより処理される。 AccessEnabler ライブラリは、デバイスから提供された情報からデバイス ID を計算し、デバイス ID をリクエストパラメータとして「明確に」Adobe Pass認証サーバに送信します。  Adobe Pass認証サーバは、Adobeの秘密鍵でデバイス ID にデジタル署名し、AccessEnabler に返される認証トークンに追加します。 したがって、デバイス ID は認証トークンに結び付けられます。  認証フロー中、AccessEnabler は、デバイス ID を認証トークンと共にクリアで再度送信します。  検証プロセスが失敗すると、自動的に認証/承認ワークフローが失敗します。  Adobe Pass認証サーバーは秘密鍵をデバイス ID に適用し、認証トークンの値と比較します。  一致しない場合、そのエンタイトルメントフローは失敗します。


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
