---
title: iOS/tvOS SDKの概要
description: iOS/tvOS SDKの概要
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '3801'
ht-degree: 0%
---
# （レガシー） iOS/tvOS SDKの概要 {#iostvos-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>


## 概要 {#intro}

iOS AccessEnablerは、モバイルアプリがTV Everywhereのエンタイトルメントサービスに対してAdobe Pass Authenticationを使用できるようにするObjective C iOS/tvOS ライブラリです。 実装は、使用権限APIを定義する&#x200B;*AccessEnabler* インターフェイスと、ライブラリがトリガーするコールバックを記述する&#x200B;*EntitlementDelegate*&#x200B;および&#x200B;*[EntitlementStatus](#ios%20entitlement%20status)* プロトコルで構成されます。 プロトコルと共にインターフェイスは、AccessEnabler ライブラリという1つの共通の名前で参照されます。

## iOSおよびtvOSの要件 {#reqs}

IOSおよびtvOS プラットフォームとAdobe Pass Authenticationに関連する現在の技術要件については、[Platform / Device / Tool Requirements](#ios)を参照し、SDK ダウンロードに含まれるリリースノートを参照してください。 このページの残りの部分では、特定のSDK バージョン以降に適用される変更点に注意するセクションが表示されます。 例えば、次は1.7.5 SDKに関する正当な注記です。

## ネイティブクライアントワークフローについて {#flows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass Authentication クライアントと同じまたは非常に似ています。 ただし、以下に示すように、いくつかの例外があります。

- [初期化後のワークフロー](#post-init)
- [汎用の初期認証ワークフロー](#generic)
- [ログアウトワークフロー](#logout)


### 初期化後のワークフロー {#post-init}

AccessEnablerでサポートされているすべての資格ワークフローでは、以前に[`setRequestor()`](#setReq)に電話してIDを確立したことがあることを前提としています。 この呼び出しを行うと、通常はアプリケーションの初期化/セットアップ段階で、リクエスト者IDを1回だけ提供できます。


IOS ネイティブクライアントでは、[`setRequestor()`](#setReq)への初回呼び出しの後、次の手順を実行する方法を選択できます。

- すぐに使用権限の呼び出しを開始し、必要に応じてサイレントでキューに入れるようにすることができます。

- [`setRequestorComplete()`](#setReqComplete) コールバックを実装すると、[`setRequestor()`](#setReq)の成功/失敗の確認を受け取ることができます。

- 上記の両方を行うことができます。

[`setRequestor()`](#setReq)の成功の通知をアプリで待機させるか、AccessEnablerのコールキューメカニズムに依存させるかを選択します。 後続のすべての認証および認証要求には要求者IDと関連する設定情報が必要なため、[`setRequestor()`](#setReq) メソッドは、初期化が完了するまで、すべての認証および認証API呼び出しを効果的にブロックします。



### 汎用の初期認証ワークフロー {#generic}

このワークフローの目的は、ユーザーにMVPDでログインすることです。 バックエンドサーバーは、ログインが成功すると、ユーザーに認証トークンを発行します。 認証は通常、認証プロセスの一部として行われますが、次の手順では、認証が単独で動作する方法を説明し、認証手順は含まれていません。

このワークフローは、一般的なブラウザーベースの認証ワークフローとはネイティブクライアントでは異なりますが、手順1～5はネイティブクライアントとブラウザーベースのクライアントの両方で同じです。

1. アプリケーションは、AccessEnablerの`getAuthentication() `API メソッドの呼び出しを使用して認証ワークフローを開始し、有効なキャッシュ済み認証トークンを確認します。
1. ユーザーが現在認証されている場合、AccessEnablerは[`setAuthenticationStatus()`](#setAuthNStatus) コールバック関数を呼び出し、成功を示す認証ステータスを渡してフローを終了します。
1. ユーザーが現在認証されていない場合、AccessEnablerは、特定のMVPDでユーザーの最後の認証試行が成功したかどうかを判断することで、認証フローを続行します。 MVPD IDがキャッシュされ、`canAuthenticate` フラグがtrueであるか、[`setSelectedProvider()`](#setSelProv)を使用してMVPDが選択された場合、MVPDの選択ダイアログでユーザーにメッセージが表示されません。 認証フローは、MVPDのキャッシュされた値（つまり、最後に成功した認証時に使用したのと同じMVPD）を使用して続行されます。 バックエンドサーバーにネットワーク呼び出しが行われ、ユーザーはMVPD ログインページにリダイレクトされます（以下の手順6）。
1. MVPD IDがキャッシュされておらず、[`setSelectedProvider()`](#setSelProv)を使用してMVPDが選択されていないか、`canAuthenticate` フラグがfalseに設定されている場合、[`displayProviderDialog()`](#dispProvDialog) コールバックが呼び出されます。 このコールバックは、ユーザーが選択できるMVPDのリストを表示するUIを作成するようにアプリケーションに指示します。 MVPD セレクターの構築に必要な情報を含む、MVPD オブジェクトの配列が提供されます。 各MVPD オブジェクトは、MVPD エンティティを表し、MVPDのID （XFINITY、AT\&amp;Tなど）などの情報を含みます。 MVPDロゴが見つかるURLです。
1. 特定のMVPDを選択したら、アプリケーションはユーザーの選択をAccessEnablerに通知する必要があります。 ユーザーが目的のMVPDを選択したら、[`setSelectedProvider()`](#setSelProv) メソッドの呼び出しを介してAccessEnablerにユーザーの選択を通知します。
1. IOS AccessEnablerは、`navigateToUrl:` コールバックまたは`navigateToUrl:useSVC:` コールバックを呼び出して、ユーザーをMVPD ログインページにリダイレクトします。 いずれかのトリガーをトリガーすると、AccessEnablerはアプリケーションに対して、`UIWebView/WKWebView or SFSafariViewController` コントローラーを作成し、コールバックの`url` パラメーターで指定されたURLを読み込むようリクエストします。 これは、バックエンドサーバー上の認証エンドポイントのURLです。 tvOS AccessEnablerの場合、[status （） ](#status_callback_implementation) コールバックが`statusDictionary` パラメーターで呼び出され、2番目の画面認証のポーリングがすぐに開始されます。 `statusDictionary`には、2回目の画面認証に使用する必要がある`registration code`が含まれています。
1. IOS AccessEnablerの場合、ユーザーはMVPDのログインページにアクセスし、アプリケーション `UIWebView/WKWebView or SFSafariViewController `controllerのメディアを通じて資格情報を入力します。 この転送中に複数のリダイレクト操作が発生し、アプリケーションは複数のリダイレクト操作中にコントローラーによって読み込まれるURLを監視する必要があります。
1. IOS AccessEnablerの場合、`UIWebView/WKWebView or SFSafariViewController` コントローラーが特定のカスタム URLを読み込むと、アプリケーションはコントローラーを閉じて、AccessEnablerの`handleExternalURL:url `API メソッドを呼び出す必要があります。 この特定のカスタム URLは実際には無効であり、コントローラが実際に読み込むことを意図していないことに注意してください。 認証フローが完了し、`UIWebView/WKWebView or SFSafariViewController` コントローラーを安全に閉じることができることを示すシグナルとしてのみ、アプリケーションで解釈する必要があります。 アプリケーションで`SFSafariViewController ` コントローラーを使用する必要がある場合、特定のカスタム URLは`application's custom scheme`によって定義されます（例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。そうでない場合、この特定のカスタム URLは`ADOBEPASS_REDIRECT_URL`定数（例：`adobepass://ios.app`）によって定義されます。
1. アプリケーションが`UIWebView/WKWebView or SFSafariViewController` コントローラを閉じてAccessEnablerの`handleExternalURL:url `API メソッドを呼び出すと、AccessEnablerはバックエンド サーバーから認証トークンを取得し、認証フローが完了したことをアプリケーションに通知します。 AccessEnablerは[`setAuthenticationStatus()`](#setAuthNStatus) コールバックをステータスコード 1で呼び出し、成功を示します。 これらの手順の実行中にエラーが発生した場合、[`setAuthenticationStatus()`](#setAuthNStatus) コールバックは、認証失敗を示すステータスコード 0と、対応するエラーコードでトリガーされます。


>[!WARNING]
>
> AccessEnablerがアプリの制御を放棄する手順（プロバイダー選択ダイアログが表示される場合、またはUIWebView/WKWebViewまたはSFSafariViewControllerが表示される場合）中に、ユーザーは認証フローをキャンセルできます。 このような状況では、アプリはこのイベントをAccessEnablerに通知し、nullをパラメーターとして渡して[`setSelectedProvider()`](#setSelProv) API メソッドを呼び出す責任があります。 これにより、AccessEnablerは内部状態をクリーンアップし、認証フローをリセットできます。 tvOSでは、同じ方法を使用して認証ポーリングをキャンセルできます。


### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは上記の認証プロセスと同様に処理されます。

1. AccessEnablerの`logout() `API メソッドを呼び出して、ログアウトワークフローを開始します。 ログアウトは、ユーザーがMVPD認証サーバーとAdobe Pass認証サーバーの両方からログアウトする必要があるため、一連のHTTP リダイレクト操作の結果です。 このフローは、AccessEnabler ライブラリが発行した単純なHTTP リクエストでは完了できないため、HTTP リダイレクト操作に従うには、`UIWebView/WKWebView or SFSafariViewController` コントローラーをインスタンス化する必要があります。

1. 認証フローと類似したパターンを採用する。 IOS AccessEnablerは、`navigateToUrl:` コールバックまたは`navigateToUrl:useSVC:`をトリガーして`UIWebView/WKWebView or SFSafariViewController` コントローラーを作成し、コールバックの`url` パラメーターで指定されたURLを読み込みます。 これは、バックエンドサーバー上のログアウトエンドポイントのURLです。 tvOS AccessEnablerの場合、`navigateToUrl:` コールバックも`navigateToUrl:useSVC:` コールバックも呼び出されません。

1. 複数のリダイレクトを行う場合、アプリケーションは`UIWebView/WKWebView or SFSafariViewController ` コントローラーのアクティビティを監視し、特定のカスタム URLを読み込む瞬間を検出する必要があります。 この特定のカスタム URLは実際には無効であり、コントローラが実際に読み込むことを意図していないことに注意してください。 ログアウトフローが完了し、コントローラーを安全に閉じることができることを示すシグナルとしてのみ、アプリケーションによって解釈される必要があります。 コントローラーがこの特定のカスタム URLを読み込むと、アプリケーションはコントローラーを閉じ、AccessEnablerの`handleExternalURL:url `API メソッドを呼び出す必要があります。 アプリケーションで`SFSafariViewController ` コントローラーを使用する必要がある場合、特定のカスタム URLは`application's custom scheme` （例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）によって定義されます。定義されていない場合、この特定のカスタム URLは` ADOBEPASS_REDIRECT_URL  `定数（例：`adobepass://ios.app`）によって定義されます。

1. 最後に、AccessEnablerはステータスコードが0の[`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、ログアウトフローの成功を示します。

ログアウト フローは、ユーザーが`UIWebView/WKWebView or SFSafariViewController` コントローラーと何らかの操作を行う必要がないという点で、認証フローとは異なります。 したがって、Adobeでは、ログアウトプロセス中にコントロールを非表示（非表示）にすることをお勧めします。

## トークン {#tokens}

- [定義と用途](#definitions)
- [キャッシュのガイドライン](#caching)
- [永続性](#persistence)
- [書式設定](#format)
- [デバイスのバインディング](#device_binding)


### 定義と用途 {#definitions}

Adobe Pass Authenticationの使用権限ソリューションは、認証ワークフローと認証ワークフローが正常に完了した際にAdobe Pass Authenticationが生成する特定のデータ（トークン）の生成を中心に展開されます。 これらのトークンは、クライアントのiOS デバイスにローカルに保存されます。



トークンの有効期間は限られています。有効期限が切れると、認証ワークフローや承認ワークフローの再開始を通じてトークンを再発行する必要があります。



エンタイトルメントワークフロー中に発行されるトークンには、次の3つのタイプがあります。

- **認証トークン：** ユーザー認証ワークフローの最終結果は、AccessEnablerがユーザーの代理で認証クエリを実行するために使用できる認証GUIDになります。 この認証GUIDには、ユーザーの認証セッション自体とは異なる場合がある、関連する有効期間（TTL）値が設定されます。 認証トークンは、認証要求を開始するデバイスに認証GUIDをバインドすることによって生成されます。
- **認証トークン：**&#x200B;一意のresourceIDで識別された特定の保護されたリソースへのアクセス権を付与します。 これは、承認者によって発行された承認付与と、元のresourceIDで構成されます。 この情報は、リクエストを開始するデバイスにバインドされます。
- **短期間有効なメディアトークン：** AccessEnablerは、短期間有効なメディアトークンを返すことにより、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、特定のリソースに対して以前に取得した認証トークンにもとづいて生成されます。 また、このトークンはデバイスにバインドされず、関連する寿命が大幅に短くなります（デフォルト：5分）。

認証と認証が成功すると、Adobe Pass Authenticationは認証、認証、短期間有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイスにキャッシュされ、関連するライフスパンの期間にわたって使用する必要があります。



### キャッシュのガイドライン {#caching}

- 認証トークン
- 認証トークン
- 短期間有効なメディアトークン


#### 認証トークン

- **AccessEnabler 1.7:**&#x200B;このSDKでは、複数のProgrammer-MVPD バケット、つまり複数の認証トークンを有効にする、トークン ストレージの新しい方法が導入されています。 現在では、「要求者ごとの認証」シナリオと通常の認証フローの両方に同じストレージレイアウトが使用されています。 2つの違いは、認証が実行される方法だけです。「要求者ごとの認証」には、ストレージ内の認証トークンの存在（別のプログラマーの場合）に基づいて、AccessEnablerがバックチャネル認証を実行できるようにする新しい改善（パッシブ認証）が含まれています。 ユーザーは1回だけ認証する必要があり、このセッションは追加のアプリで認証トークンを取得するために使用されます。 このバックチャネルフローは[`setRequestor()`](#setReq)呼び出し中に発生し、ほとんどがプログラマーに対して透過的です。 **ただし、ここで重要な要件が1つあります。プログラマーはメイン UI スレッドからsetRequestor （）を呼び出す必要があります。**
- **AccessEnabler 1.6以前：** デバイスで認証トークンをキャッシュする方法は、現在のMVPDに関連付けられている「**リクエスト者ごとの認証」** フラグによって異なります。

<!-- end list -->

1. 「Authentication per Requestor」機能が無効になっている場合、1つの認証トークンがグローバルなペーストボードにローカルに保存されます。このトークンは、現在のMVPDと統合されているすべてのアプリケーション間で共有されます。
1. 「要求者ごとの認証」機能が有効になっている場合、トークンは認証フローを実行したプログラマーに明示的に関連付けられます（トークンはグローバルペーストボードには保存されず、そのプログラマーのアプリケーションにのみ表示されるプライベートファイルに保存されます）。 具体的には、異なるアプリケーション間のシングルサインオン（SSO）は無効になります。新しいアプリに切り替える際には、認証フローを明示的に実行する必要があります（2番目のアプリのプログラマーが現在のMVPDと統合されており、そのプログラマーの認証トークンがローカルキャッシュに存在しないという事実を提供します）。



#### 認証トークン

常に、AccessEnablerによってキャッシュされるのは、リソースごとに1つの認証トークンのみです。 複数の認証トークンをキャッシュすることもできますが、それらは異なるリソースに関連付けられています。 新しい認証トークンが発行され、同じリソース *に対して古い認証トークンが既に存在する場合、新しいトークンは既存のキャッシュ値を上書きします。*



#### メディアトークン

短期間有効なメディアトークンは、まったくキャッシュしないでください。 メディアトークンは、1回限りの使用に制限されているため、認証APIが呼び出されるたびにサーバーから取得する必要があります。



### 永続性 {#persistence}

トークンは、同じアプリケーションの連続した実行全体で永続的である必要があります。 つまり、認証トークンと認証トークンが取得され、ユーザーがアプリケーションを閉じると、ユーザーがアプリケーションを再度開いたときに同じトークンがアプリケーションで使用できるようになります。 さらに、これらのトークンは複数のアプリケーションにわたって永続的であることが望ましい。 つまり、ユーザーが1つのアプリケーションを使用して特定のID プロバイダーでログインした後（認証トークンと認証トークンを正常に取得した後）、同じID プロバイダーを介してログインする際に、同じトークンを別のアプリケーションで使用でき、同じID プロバイダーを介してログインする際に資格情報の入力を求めるメッセージが表示されなくなります。 この種のシームレスな認証/認証ワークフローは、Adobe Pass Authentication ソリューションを真のTV-Everywhereにするものです
実装：



## iOS

IOS AccessEnabler ライブラリは、トークン データを&#x200B;*貼り付けボード*&#x200B;と呼ばれる「クリップボードのような」データ構造に保存することで、アプリケーション間のデータ共有の問題を回避します。 このシステムレベルの共有リソースは、目的の永続トークンのユースケースの実装を可能にする主要な要素を提供します。

- **構造化ストレージのサポート** – 貼り付けボードは、単純な線形バッファーのようなメモリ構造ではありません。 ユーザーが指定したキー値に基づいてデータインデックスを作成できる、ディクショナリのようなストレージメカニズムを提供します。
- **基礎となるファイルシステムを使用したデータ永続性のサポート** – 貼り付けボード構造の内容は永続性としてマークできます。この場合、データはデバイスの内部メモリに保存されます。



特定のトークンがトークンキャッシュに配置されると、その有効性はAccessEnabler ライブラリによって異なる時間にチェックされます。  有効なトークンは、次のように定義されます。

- トークンのTTLが期限切れになっていません。
- トークンの発行者は、許可されたID プロバイダーのリストに含まれます。



## tvOS

tvOSではペーストボードは使用できないため、tvOS AccessEnabler ライブラリではNsUserDefaultsをストレージオプションとして使用します。 これにより、認証トークンと認証トークンを保持する問題は解決されますが、保存された情報を異なるアプリケーション間で共有することはできません。



**iOS 10 ペーストボードの変更点 –**&#x200B;以前のバージョンのiOSからアップグレードすると、ペーストボードは消去されます。 つまり、すべてのアプリケーションを再認証する必要があります。



**iOS 7のペーストボードの変更 –** iOS 7でのペーストボードの機能の変更により、iOS 7で実行中のアプリケーション間のクロス SSOが制限されます。 同じ`<Bundle Seed ID>` （別名`<Team ID>`）を持つアプリケーションはトークンを共有します。つまり、同じプログラマーXのアプリ A1とアプリ A2はトークンを共有し、アプリ A1 （プログラマーX）とアプリ A3 （プログラマーY）はトークンを共有しません。

- バンドルシード ID/チーム IDは、同じプロビジョニングプロファイルによって生成された2つのアプリ間で同じです。 詳細については、このリンクを参照してください。
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- この「クロス SSO」制限は、使用されるAdobe Pass Authentication SDKに関係なく、iOS 7に存在します。

IOS 7以降でのSSOの設定について詳しくは、このテクニカルノートを参照してください（このテクニカルノートはAccess Enabler v1.8以降に適用されます）。<https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### トークンストレージ（AccessEnabler 1.7）

AccessEnabler 1.7以降、トークン ストレージは、複数の認証トークンを保持できるマルチレベルのネストされたマップ構造に依存して、複数のプログラマーとMVPDの組み合わせをサポートできます。 この新しいストレージは、AccessEnabler パブリック APIに影響を与えることはなく、プログラマー側での変更は必要ありません。 この例では
この新機能について説明します。

1. Open App1 （Programmer1によって開発）。
1. MVPD1 （Programmer1と統合）で認証します。
1. 現在のアプリケーションを一時停止/終了し、App2 （Programmer2によって開発）を開きます。
1. Programmer2はMVPD2と統合されていないと仮定します。したがって、ユーザーはApp2で認証されません。
1. App2でMVPD2 （Programmer2と統合）を使用して認証します。
1. App1に切り替えます。ユーザーはProgrammer1で認証されます。

以前のバージョンのAccessEnablerでは、トークン ストレージでサポートされているのは1つの認証トークンのみであったため、ステップ 6では認証されていないユーザーとして表示されます。



1つのプログラマー/MVPD セッションからログアウトすると、デバイス上の他のすべてのプログラマー/MVPD認証トークンを含む、基盤となるストレージ全体がクリアされます。 一方、認証フローをキャンセルする（[`setSelectedProvider(null)`](#setSelProv)を呼び出す）と、基になるストレージは消去されませんが、現在のプログラマー/MVPD認証試行（現在のプログラマーのMVPDを消去することで）にのみ影響します。



### トークンインポーター（AccessEnabler 1.7）

AccessEnabler 1.7に含まれているもう1つのストレージ関連の機能により、古いストレージ領域から認証トークンを読み込むことができます。 この「トークンインポーター」は、ストレージのバージョンがアップグレードされた場合でもSSO状態を維持しながら、連続したAccessEnabler リリース間の互換性を実現するのに役立ちます。 インポーターは[`setRequestor()`](#setReq) フロー中に実行され、次の2つのシナリオで実行されます（現在のプログラマーに有効な認証トークンが現在のストレージに存在しないと仮定します）。

- 特定のプログラマーによって開発された1.7 アプリの最初のインストール
- 新しいストレージを使用する将来のAccessEnablerへのパスのアップグレード

読み込み操作はプログラマに対して透過的であり、クライアントアプリケーションでコードを変更する必要はありません。



### トークンサニタイザー（AccessEnabler 1.7.5）

AccessEnabler 1.7.5以降、このサービスは[`setRequestor()`](#setReq)`. `で実行されます。これは、WiFi MAC アドレスからIDFAへのトラッキング用のiOS 7の切り替えの結果として開発されました。 サニタイザーは、現在のストレージに有効な認証トークンのみが含まれていることを確認します（iOS7より前は、以前はMAC アドレスを使用して計算されていたデバイス IDで有効）。 トークンサニタイザーは、無効なトークンをすべて削除します。



Token Sanitizer サービスは、AccessEnabler 1.7.5 アプリケーションがiOS 6で使用され、ユーザーがiOS 7に更新される場合に最も便利です。 この場合、iOS 6で取得したすべての認証トークンが無効になります（Device ID アルゴリズムがMAC アドレスを使用してIDFAに変更されたため）。 サニタイザーは無効なトークンをすべてクリアし、ユーザーは未認証になります。



トークンサニタイザーが無効なトークンを削除しない場合、無効な認証トークンが存在するため、AccessEnablerは認証を取得できません。 認証エラーメッセージは暗号化されやすく、問題の原因を特定するのに過度に役立つことはないため、エンドユーザーはデバッグが困難になります。



### 書式設定 {#format}

- [認証トークン](#authn_token)
- [AuthZ トークン](#authz_token)
- [ショートメディアトークン](#short_token)
- [デバイスのバインディング](#device_binding)


AuthN トークンとAuthZ トークンの形式は、バックグラウンド情報の場合にのみ、ここに含まれています。 これらのトークンの構造は、いつでもAdobe Pass認証によって変更できます。 プログラマーは、アプリを実装するためにAuthNおよびAuthZ トークンの正確な構造を知る必要がないため、AuthNおよびAuthZ トークンはローカルデバイスで公開されません。 ショート メディア トークン *は*&#x200B;がプログラマーのアプリケーションに公開されています。



#### 認証トークン {#authn_token}

以下のリストは、認証トークンの形式を示しています。

```
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

```
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


#### ショートメディアトークン {#short_token}

以下のリストは、ショートメディアトークンの形式を示しています。 このトークンはプログラマのアプリケーションに公開されます。 プログラムの使用権限プロセスが正常に終了すると、プログラムのアプリケーションに渡されます。

```
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


### デバイスのバインディング {#device_binding}

上記のXML リストで、`simpleTokenFingerprint`というタイトルのタグに注意してください。 このタグの目的は、ネイティブデバイス IDの個別化情報を保持することです。 AccessEnabler ライブラリは、このような個別化情報を取得し、使用権限の呼び出し中にAdobe Pass Authentication サービスで利用できるようにします。 この情報を使用して実際のトークンに埋め込むことで、トークンを特定のデバイスに効果的にバインドできます。 最終的な目標は、デバイス間でトークンを転送できないようにすることです。



これは明らかにセキュリティ関連の機能なので、この情報は本質的にセキュリティの観点から「機密」です。 その結果、この情報は改ざんと盗聴の両方から保護される必要があります。 盗聴の問題は、認証/認証要求をHTTPS プロトコル経由で送信することで解決されます。 改ざん防止は、デバイス識別情報にデジタル署名することによって処理されます。 AccessEnabler ライブラリは、デバイスから提供された情報からデバイス IDを計算し、デバイス IDをリクエストパラメーターとしてAdobe Pass Authentication サーバーに「クリア」で送信します。 Adobe Pass認証サーバーは、Adobeの秘密鍵を使用してデバイス IDにデジタル署名し、AccessEnablerに返される認証トークンに追加します。 したがって、デバイス IDは認証トークンにバインドされる。 認証フロー中、AccessEnablerは認証トークンと共にデバイス IDをクリアに再度送信します。 検証プロセスが失敗すると、認証/承認ワークフローが自動的に失敗します。 Adobe Pass認証サーバーは、デバイス IDに秘密鍵を適用し、認証トークンの値と比較します。 それらが一致しない場合、その資格フローは失敗します。



**AccessEnabler 1.7.5でのデバイス バインディングに関する注意：** AccessEnabler 1.7.5以降、iOS デバイスの個別化情報を提供するためにデバイス IDを計算する方法が変更されました。 この変更は、iOS 7の変更を反映しています：iOS 7から、Appleでは、広告主向け識別子（IDFA）に代わって、トラッキング オプションとしてWiFi MAC アドレスを提供しなくなりました。 IOS 7で動作するアプリの個別化情報はIDFAに基づいており、その情報はエンタイトルメントフロートークンに埋め込まれているため、この変更によってユーザーエクスペリエンスに様々な影響が生じる可能性があります。 考えられる様々な効果は、ユーザーがアップグレードするiOSのバージョンと、プログラマーがアップグレードするAccessEnablerのバージョンとの組み合わせに基づいています。 この変更について詳しくは、AccessEnabler SDK 1.7.5に含まれているリリースノートを参照してください。

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
