---
title: iOS/tvOS SDKの概要
description: iOS/tvOS SDKの概要
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3801'
ht-degree: 0%

---

# （従来の）iOS/tvOS SDKの概要 {#iostvos-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>


## 概要 {#intro}

iOS AccessEnabler は、モバイルアプリが TV Everywhere の権利付与サービスにAdobe Pass Authentication を使用できるようにする、Objective C iOS/tvOS ライブラリです。 このインプリメンテーションは、資格 API を定義する *AccessEnabler* インタフェースと、ライブラリがトリガーするコールバックを記述する *EntitlementDelegate* および *[EntitlementStatus](#ios%20entitlement%20status)* プロトコルで構成されます。 プロトコルと共にインタフェースは、共通の名前である AccessEnabler ライブラリの下で参照されます。

## iOSと tvOS の要件 {#reqs}

iOS、tvOS プラットフォーム、Adobe Pass認証に関する現在の技術要件については、[&#x200B; プラットフォーム/デバイス/ツールの要件 &#x200B;](#ios) を参照し、SDKのダウンロードに含まれているリリースノートを参照してください。 以降のページには、特定のSDK バージョン以降に適用される変更点に関する注意事項が記載された節が表示されます。 例えば、以下は 1.7.5 SDKに関する正当な注意事項です。

## ネイティブクライアントワークフローについて {#flows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass認証クライアントと同じか、非常に似ています。 ただし、以下に説明するように、いくつかの例外があります。

- [初期化後のワークフロー](#post-init)
- [汎用初期認証ワークフロー](#generic)
- [ログアウトワークフロー](#logout)


### 初期化後のワークフロー {#post-init}

AccessEnabler でサポートされるすべての使用権限ワークフローでは、ID を確立するために以前に [`setRequestor()`](#setReq) と呼んだことを前提としています。 この呼び出しを行うと、要求者 ID を 1 回だけ指定できます。通常、アプリケーションの初期化/設定段階で指定します。


iOS ネイティブクライアントの場合、[`setRequestor()`](#setReq) への最初の呼び出しの後、進め方を選択できます。

- エンタイトルメントの呼び出しを直ちに開始し、必要に応じてサイレントにキューに入れることができます。

- [`setRequestorComplete()`](#setReqComplete) コールバックを実装することで、[`setRequestor()`](#setReq) の成功/失敗の確認を受け取ることができます。

- 上記の両方を実行できます。

[`setRequestor()`](#setReq) の成功の通知をアプリで待たせるか、AccessEnabler のコール キューメカニズムに依存させるかを選択できます。 後続のすべての承認および認証リクエストには要求者 ID と関連する設定情報が必要なため、[`setRequestor()`](#setReq) の方法は、初期化が完了するまで、すべての認証および承認 API 呼び出しを効果的にブロックします。



### 汎用初期認証ワークフロー {#generic}

このワークフローの目的は、MVPDを使用してユーザーにログインすることです。 ログインに成功すると、バックエンドサーバーはユーザーに認証トークンを発行します。 認証は通常、認証プロセスの一部として行われますが、次に認証が単独で機能する方法を説明します。には認証手順は含まれていません。

このワークフローは、ネイティブクライアントと一般的なブラウザーベースの認証ワークフローでは異なりますが、手順 1～5 は、ネイティブクライアントとブラウザーベースのクライアントで同じです。

1. アプリケーションは、AccessEnabler の `getAuthentication() `API メソッドを呼び出して認証ワークフローを開始します。このメソッドは、有効なキャッシュされた認証トークンをチェックします。
1. ユーザーが現在認証されている場合、AccessEnabler は [`setAuthenticationStatus()`](#setAuthNStatus) コールバック関数を呼び出し、成功を示す認証ステータスを渡してフローを終了します。
1. ユーザーが現在認証されていない場合、AccessEnabler は、ユーザーの最後の認証試行が特定のMVPDで成功したかどうかを確認することにより、認証フローを続行します。 MVPD ID がキャッシュされていて、`canAuthenticate` フラグが true であるか、またはMVPDが [`setSelectedProvider()`](#setSelProv) を使用して選択されている場合、MVPD選択ダイアログが表示されません。 認証フローは、MVPDのキャッシュされた値（最後に成功した認証で使用したMVPDと同じ）を使用して続行されます。 バックエンドサーバーに対してネットワーク呼び出しが実行され、ユーザーがMVPDのログインページにリダイレクトされます（以下の手順 6）。
1. MVPD ID がキャッシュされておらず、[`setSelectedProvider()`](#setSelProv) を使用してMVPDが選択されていない場合、または `canAuthenticate` フラグが false に設定されている場合、[`displayProviderDialog()`](#dispProvDialog) コールバックが呼び出されます。 このコールバックは、選択する MVPD のリストをユーザーに提示する UI を作成するようにアプリケーションに指示します。 MVPD セレクターを構築するために必要な情報を含む、MVPD オブジェクトの配列が提供されます。 各MVPD オブジェクトは、1 つのMVPD エンティティを表し、MVPDの ID （XFINITY、AT\&amp;T など）などの情報を含みます。 およびMVPD ロゴを見つけることができる URL です。
1. 特定のMVPDを選択したら、AccessEnabler にユーザーが選択した旨を通知する必要があります。 ユーザーが目的のMVPDを選択したら、[`setSelectedProvider()`](#setSelProv) メソッドを呼び出して、AccessEnabler に通知します。
1. iOS AccessEnabler は、`navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` コールバックを呼び出して、MVPD ログインページにユーザーをリダイレクトします。 AccessEnabler は、どちらかを起動することにより、アプリケーションに対して `UIWebView/WKWebView or SFSafariViewController` コントローラの作成と、コールバックの `url` パラメータで指定された URL のロードを要求します。 これは、バックエンドサーバー上の認証エンドポイントの URL です。 tvOS AccessEnabler の場合、`statusDictionary` のパラメータで [status （） &#x200B;](#status_callback_implementation) コールバックが呼び出され、2 番目の画面認証のポーリングが直ちに開始されます。 `statusDictionary` には、2 番目の画面認証に使用する必要がある `registration code` が含まれています。
1. iOS AccessEnabler の場合は、MVPDのログインページに移動し、Application `UIWebView/WKWebView or SFSafariViewController `Controller を使用して資格情報を入力します。 この転送中にいくつかのリダイレクト操作が発生し、アプリケーションは、複数のリダイレクト操作中にコントローラによって読み込まれる URL を監視する必要があることに注意してください。
1. iOS AccessEnabler の場合、`UIWebView/WKWebView or SFSafariViewController` コントローラが特定のカスタム URL をロードすると、アプリケーションはコントローラを閉じて AccessEnabler の `handleExternalURL:url `API メソッドを呼び出す必要があります。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 認証フローが完了したこと、および `UIWebView/WKWebView or SFSafariViewController` コントローラを安全に閉じることができることを示すシグナルとして、アプリケーションが解釈する必要があります。 アプリケーションで `SFSafariViewController `controller を使用する必要がある場合、特定のカスタム URL は `application's custom scheme` で定義されます（例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）。そうでない場合、この特定のカスタム URL は `ADOBEPASS_REDIRECT_URL` 定数で定義されます（例：`adobepass://ios.app`）。
1. アプリケーションが `UIWebView/WKWebView or SFSafariViewController` コントローラを閉じ、AccessEnabler の `handleExternalURL:url `API メソッドを呼び出すと、AccessEnabler はバックエンド サーバから認証トークンを取得し、認証フローが完了したことをアプリケーションに通知します。 AccessEnabler は、ステータス コードが 1 の [`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、成功を示します。 これらの手順の実行中にエラーが発生した場合、[`setAuthenticationStatus()`](#setAuthNStatus) コールバックは、認証失敗を示すステータスコード 0 と対応するエラーコードでトリガーされます。


>[!WARNING]
>
> AccessEnabler がアプリケーションに対する制御を放棄するステップでは（つまり、プロバイダ選択ダイアログが表示される場合、または UIWebView/WKWebView または SFSafariViewController が表示される場合）、ユーザーは認証フローをキャンセルできます。 このような場合、アプリケーションは、AccessEnabler にこのイベントを通知し、[`setSelectedProvider()`](#setSelProv) API メソッドを呼び出して、null をパラメータとして渡します。 これにより、AccessEnabler は内部状態をクリーンアップし、認証フローをリセットできます。 tvOS では、同じ方法を使用して認証ポーリングをキャンセルできます。


### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは、上記の認証プロセスと同様に処理されます。

1. アプリケーションは、AccessEnabler の `logout() `API メソッドを呼び出してログアウト ワークフローを開始します。 ログアウトは、ユーザーがAdobe PassMVPDサーバーと認証サーバーの両方からログアウトする必要があるために、一連の HTTP リダイレクト操作の結果です。 AccessEnabler ライブラリによって発行された単純な HTTP 要求では、このフローを完了できないため、HTTP リダイレクト操作に従うには、`UIWebView/WKWebView or SFSafariViewController` コントローラをインスタンス化する必要があります。

1. 認証フローと同様のパターンを採用する。 iOS AccessEnabler は、`navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` をトリガーして、`UIWebView/WKWebView or SFSafariViewController` コントローラを作成し、コールバックの `url` パラメータで指定された URL をロードします。 これは、バックエンドサーバー上のログアウトエンドポイントの URL です。 tvOS AccessEnabler の場合、`navigateToUrl:` コールバックも `navigateToUrl:useSVC:` コールバックも呼び出されません。

1. 複数のリダイレクトを実行する場合、アプリケーションは `UIWebView/WKWebView or SFSafariViewController `controller のアクティビティを監視し、特定のカスタム URL を読み込む瞬間を検出する必要があります。 この特定のカスタム URL は、実際には無効であり、コントローラーが実際に読み込むことを目的としたものではないことに注意してください。 これは、ログアウトフローが完了し、コントローラを安全に閉じられるというシグナルとして、アプリケーションによってのみ解釈される必要があります。 コントローラがこの特定のカスタム URL を読み込むとき、アプリケーションはコントローラを閉じて AccessEnabler の `handleExternalURL:url `API メソッドを呼び出す必要があります。 アプリケーションで `SFSafariViewController `controller を使用する必要がある場合、特定のカスタム URL は `application's custom scheme` （例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）によって定義されます。それ以外の場合、この特定のカスタム URL は ` ADOBEPASS_REDIRECT_URL  ` 定数（例：`adobepass://ios.app`）によって定義されます。

1. 最後に、AccessEnabler はステータス コードが 0 の [`setAuthenticationStatus()`](#setAuthNStatus) コールバックを呼び出し、ログアウト フローの成功を示します。

ログアウトフローは、認証フローとは異なり、ユーザーは `UIWebView/WKWebView or SFSafariViewController` ントロールコントローラーとやり取りする必要がありません。 そのため、Adobeでは、ログアウトプロセス中はコントロールを非表示（つまり非表示）にすることをお勧めします。

## トークン {#tokens}

- [定義と使用方法](#definitions)
- [キャッシュガイドライン](#caching)
- [永続性](#persistence)
- [書式設定](#format)
- [デバイス バインド](#device_binding)


### 定義と使用方法 {#definitions}

Adobe Pass認証使用権限ソリューションでは、認証および承認ワークフローが正常に完了したときにAdobe Pass認証によって生成される特定のデータ（トークン）を生成します。 これらのトークンは、クライアントのiOS デバイス上にローカルで保存されます。



トークンの有効期間は限られています。有効期限が切れた場合は、認証ワークフローまたは承認ワークフローを再開することでトークンを再発行する必要があります。



使用権限ワークフローで発行されるトークンには、次の 3 つのタイプがあります。

- **認証トークン：** ユーザー認証ワークフローの最終結果は、ユーザーに代わって AccessEnabler が認証クエリを実行するために使用できる認証 GUID になります。 この認証 GUID には、ユーザーの認証セッション自体とは異なる可能性のある、関連付けられた有効期間（TTL）値があります。 認証トークンは、認証要求を開始するデバイスに認証 GUID をバインドすることで生成されます。
- **認証トークン：** 一意の resourceID で識別される特定の保護リソースへのアクセスを許可します。 これは、認証者が発行した認証付与と、元の resourceID で構成されます。 この情報は、リクエストを開始するデバイスにバインドされます。
- **短時間のみ有効なメディアトークン：** AccessEnabler は、短時間のみ有効なメディアトークンを返すことで、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、その特定のリソースに対して以前に取得された認証トークンに基づいて生成されます。 また、このトークンはデバイスにバインドされず、関連付けられている寿命が大幅に短くなります（デフォルト：5 分）。

認証と承認に成功すると、Adobe Pass Authentication は認証、承認および短時間のみ有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイスにキャッシュし、関連する存続期間中に使用する必要があります。



### キャッシュガイドライン {#caching}

- 認証トークン
- 認証トークン
- 短時間のみ有効なメディアトークン


#### 認証トークン

- **AccessEnabler 1.7:** このSDKでは、新しいトークン・ストレージ方式が導入され、複数のプログラマーMVPD・バケットを使用できるため、複数の認証トークンが使用可能になります。 現在は、「リクエスターごとの認証」シナリオと通常の認証フローの両方で同じストレージレイアウトが使用されています。 この 2 つの違いは、認証の実行方法にあります。「要求者ごとの認証」には、ストレージ内に（異なるプログラマの）認証トークンが存在することに基づいて、AccessEnabler がバックチャネル認証を実行できるようにする新しい改善点（パッシブ認証）が含まれています。 ユーザーの認証は 1 回だけでかまいません。追加のアプリで認証トークンを取得するために、このセッションが使用されます。 このバックチャネルフローは、[`setRequestor()`](#setReq) 呼び出し時に発生し、プログラマーに対してほとんど透過的です。**ただし、ここで重要な要件は 1 つあります。プログラマーは、メインの UI スレッドから setRequestor （）を呼び出す必要があります**。
- **AccessEnabler 1.6 以前：** 認証トークンがデバイス上でキャッシュされる方法は、現在のMVPDに関連づけられている「**Authentication per Requestor」** フラグによって異なります。

<!-- end list -->

1. 「要求者ごとの認証」機能が無効になっている場合、1 つの認証トークンがグローバルなペーストボードにローカルで保存されます。このトークンは、現在のMVPDに統合されているすべてのアプリケーション間で共有されます。
1. 「要求者ごとの認証」機能が有効な場合、トークンは認証フローを実行したプログラマーに明示的に関連付けられます（トークンはグローバルなペーストボードには保存されず、そのプログラマーのアプリケーションにのみ表示可能なプライベートファイルに表示されます）。 具体的には、異なるアプリケーション間でのシングルサインオン（SSO）は無効になります。ユーザーは、新しいアプリに切り替える際に、認証フローを明示的に実行する必要があります（2 番目のアプリのプログラマーが現在のMVPDに統合されており、そのプログラマーの認証トークンがローカルキャッシュに存在しない場合）。



#### 認証トークン

AccessEnabler によってキャッシュされる認証トークンは、常にリソースごとに 1 つのみです。 複数の認証トークンがキャッシュされている場合がありますが、それらは異なるリソースに関連付けられています。 新しい認証トークンが発行され、古いトークンが *同じリソース* に既に存在する場合、新しいトークンは既存のキャッシュ値を上書きします。



#### メディアトークン

短時間のみ有効なメディアトークンはキャッシュしないでください。 メディアトークンは、1 回限りの使用に制限されているので、認証 API が呼び出されるたびにサーバーから取得する必要があります。



### 永続性 {#persistence}

トークンは、同じアプリケーションを連続して実行しても永続的である必要があります。 つまり、認証トークンと認証トークンが取得され、ユーザーがアプリケーションを閉じると、ユーザーがアプリケーションを再度開いたときに、同じトークンがアプリケーションで使用できるようになります。 さらに、これらのトークンは、複数のアプリケーションにわたって永続的であることが望ましいです。 つまり、ユーザーが 1 つのアプリケーションを使用して特定の ID プロバイダーでログインした後（認証および認証トークンの取得に成功した場合）、別のアプリケーションを通じて同じトークンを使用でき、同じ ID プロバイダーを介してログインする際に、ユーザーが資格情報を要求されなくなりました。 このタイプのシームレスな認証/承認ワークフローにより、Adobe Pass認証ソリューションは真の TV-Everywhere となっています
実装。



## iOS

iOS AccessEnabler ライブラリは、トークン・データを *貼り付けボード* と呼ばれる「クリップボードのような」データ構造に保存することで、アプリケーション間のデータ共有の問題を回避します。 このシステムレベルの共有リソースは、目的とする永続的なトークンのユースケースの実装を可能にする主な要素を提供します。

- **構造化ストレージのサポート** - ペーストボードは、単純な線形バッファのようなメモリ構造ではありません。 ユーザーが指定したキー値に基づくデータインデックス作成を可能にする、辞書に似たストレージメカニズムを提供します。
- **基になるファイルシステムを使用したデータ永続性のサポート** – 貼り付けボード構造の内容は、永続性としてマークできます。この場合、データはデバイスの内部メモリに保存されます。



特定のトークンがトークン・キャッシュに配置されると、その有効性は AccessEnabler ライブラリによって異なる時刻にチェックされます。  有効なトークンは次のように定義されます。

- トークンの TTL が期限切れではありません。
- トークンの発行者は、許可された ID プロバイダーのリストに含まれます。



## tvOS

tvOS ではペーストボードが使用できないため、tvOS AccessEnabler ライブラリは NsUserDefaults をストレージ オプションとして使用します。 これにより、認証トークンと認証トークンを保持する問題が解決されますが、保存された情報は異なるアプリケーション間で共有できません。



**iOS 10 ペーストボードの変更点 –** 以前のバージョンのiOSからアップグレードすると、ペーストボードが消去されます。 つまり、すべてのアプリケーションを再認証する必要があります。



**iOS 7 のペーストボードの変更 –** iOS 7 のペーストボードの機能が変更されたので、iOS 7 で実行されているアプリケーション間のクロス SSO は制限されます。 同じ `<Bundle Seed ID>` （`<Team ID>` とも呼ばれます）を持つアプリケーションはトークンを共有します。つまり、同じプログラマー X のアプリ A1 と A2 はトークンを共有し、アプリ A1 （プログラマー X）とアプリ A3 （プログラマー Y）はトークンを共有しません。

- バンドルシード ID/チーム ID は、同じプロビジョニングプロファイルで生成される場合、2 つのアプリで同じになります。 詳しくは、次のリンクを参照してください。
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- この「クロス SSO」制限は、使用されるAdobe Pass認証SDKに関係なく、iOS 7 に存在します。

iOS 7 以降で SSO を構成する方法の詳細については、このテクニカル ノートを参照してください（テクニカル ノートはアクセス イネーブラ v1.8 以降に適用されます）。<https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### トークン・ストレージ（AccessEnabler 1.7）

AccessEnabler 1.7 以降、トークン・ストレージは、複数の認証トークンを保持できるマルチレベルのネストされたマップ構造に基づいて、複数の Programmer-MVPDの組み合わせをサポートできます。 この新しいストレージは、AccessEnabler パブリック API に影響を与えず、プログラマ側での変更も必要ありません。 次に例を示します
に、この新しい機能を示します。

1. App1 （プログラマー 1 が開発）を開きます。
1. （Programmer1 と統合された）MVPD1 を使用して認証します。
1. 現在のアプリケーションを中断/閉じて、App2 （Programmer2 が開発）を開きます。
1. Programmer2 がMVPD2 と統合されていないので、App2 では認証されないとします。
1. App2 でMVPD2 （Programmer2 と統合）を使用して認証します。
1. App1 に切り替えます。ユーザーは Programmer1 で認証されます。

以前のバージョンの AccessEnabler では、トークン・ストレージでサポートされていた認証トークンは 1 つのみのため、ステップ 6 ではユーザーが未認証と表示されていました。



1 つのプログラマー/MVPD セッションからログアウトすると、デバイス上の他のすべてのプログラマー/MVPD認証トークンを含む、基礎となるストレージ全体がクリアされます。 一方、認証フローを取り消す（[`setSelectedProvider(null)`](#setSelProv) を呼び出す）と、基になるストレージはクリアされませんが、（現在のプログラマーのMVPDを消去することで）現在のプログラマー/MVPDの認証の試行にのみ影響を与えます。



### トークン インポーター（AccessEnabler 1.7）

AccessEnabler 1.7 に含まれるもう 1 つのストレージ関連機能により、古いストレージ領域から認証トークンをインポートできます。 この「トークン インポーター」は、連続する AccessEnabler のリリース間の互換性を実現し、ストレージ バージョンがアップグレードされた場合でも SSO 状態を維持するのに役立ちます。 インポーターは、[`setRequestor()`](#setReq) フロー中に実行され、次の 2 つのシナリオで実行されます（現在のプログラマーの有効な認証トークンが現在のストレージに存在しないと仮定）。

- 特定のプログラマーによって開発された 1.7 アプリの最初のインストール
- 新しいストレージを使用する将来の AccessEnabler へのアップグレード・パス

読み込み操作はプログラマーに対して透過的で、クライアントアプリケーションでコードを変更する必要はありません。



### トークン・サニタイザ（AccessEnabler 1.7.5）

AccessEnabler 1.7.5 以降、このサービスは [`setRequestor()`](#setReq)`. `WiFi MAC アドレスからトラッキング用の IDFA へのiOS 7 の切り替えの結果として開発されました。 サニタイザーは、現在のストレージに有効な認証トークン（デバイス ID （以前はiOS7 より前のMAC アドレスを使用して計算されていました）のみが含まれていることを確認します。 トークンサニタイザーは、無効なトークンをすべて削除します。



Token Sanitizer サービスは、AccessEnabler 1.7.5 アプリケーションをiOS 6 で使用した後、iOS 7 にアップデートする場合に最も役立ちます。 これが発生すると、（MAC アドレスの使用から IDFA にデバイス ID アルゴリズムが変更されたため）iOS 6 で取得されたすべての認証トークンが無効になります。 サニタイザーは無効なトークンをすべてクリアし、ユーザーは認証されなくなります。



トークン・サニタイザが無効なトークンを削除しないと、無効な認証トークンが存在するため、AccessEnabler は認証を取得できません。 承認エラーメッセージは不可解な場合があり、問題の原因を判断する際にあまり役に立たないので、エンドユーザーにとってはデバッグが難しくなります。



### 書式設定 {#format}

- [AuthN トークン](#authn_token)
- [AuthZ トークン](#authz_token)
- [短いメディアトークン](#short_token)
- [デバイス バインド](#device_binding)


なお、AuthN トークンと AuthZ トークンの形式は、背景情報としてのみ記載されています。 これらのトークンの構造は、Adobe Pass Authentication でいつでも変更できます。 AuthN および AuthZ トークンはローカルデバイスでは公開されないので、プログラマーはアプリを実装するために AuthN および AuthZ トークンの正確な構造を知る必要はありません。 ショートメディアトークン *は* プログラマーのアプリケーションに公開されます。



#### 認証トークン {#authn_token}

次のリストは、認証トークンの形式を示しています。

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


#### 短いメディアトークン {#short_token}

以下のリストは、ショートメディアトークンの形式を示しています。 このトークンは、プログラマーのアプリケーションに公開されます。 正常な使用権限プロセスの最後に、プログラマーのアプリケーションに渡されます。

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


### デバイス バインド {#device_binding}

上記の XML の一覧で、「`simpleTokenFingerprint`」というタグが付いていることに注意してください。 このタグの目的は、ネイティブデバイス ID の個別化情報を保持することです。 AccessEnabler ライブラリでは、これらの個別化情報を取得し、使用権限呼び出し中にAdobe Pass認証サービスで使用できるようにします。 サービスはこの情報を使用して、実際のトークンに埋め込み、トークンを特定のデバイスに効果的にバインドします。 この最終目標は、デバイス間でトークンを転送できないようにすることです。



これは明らかにセキュリティ関連の機能なので、この情報はセキュリティの観点から本質的に「機密」です。 その結果、この情報は改ざんと盗聴の両方から保護する必要があります。 盗聴の問題は、HTTPS プロトコルで認証/承認リクエストを送信することで解決されます。 改ざん防止は、デバイス識別情報にデジタル署名することで処理されます。 AccessEnabler ライブラリは、デバイスから提供された情報からデバイス ID を計算し、そのデバイス ID をリクエスト・パラメータとしてAdobe Pass認証サーバに「クリアに」送信します。 Adobe Pass認証サーバーは、Adobeの秘密鍵を使用してデバイス ID にデジタル署名し、AccessEnabler に返される認証トークンに追加します。 したがって、デバイス ID は認証トークンにバインドされます。 認証フロー中に、AccessEnabler はデバイス ID を認証トークンとともにクリアで再度送信します。 検証プロセスが失敗すると、認証/承認ワークフローが自動的に失敗します。 Adobe Pass認証サーバーは、秘密鍵をデバイス ID に適用し、認証トークンの値と比較します。 一致しない場合、使用権限フローは失敗します。



**AccessEnabler 1.7.5 のデバイス バインディングに関する注意：** AccessEnabler 1.7.5 以降、iOS デバイスの個別化情報を提供するために、デバイス ID の計算方法が変更されました。 この変更は、iOS 7 における次の変更を反映しています。iOS 7 からは、Appleは WiFi MAC アドレスをトラッキングオプションとして提供しなくなり、広告主の識別子（IDFA）を優先します。 iOS 7 で動作するアプリの個別化情報は IDFA に基づいており、その情報はエンタイトルメントフロートークンに埋め込まれているので、この変更によってユーザーエクスペリエンスに様々な影響が生じる可能性があります。 使用できる影響は、アップグレード元のiOSのバージョンと、プログラマがアップグレード元の AccessEnabler のバージョンによって異なります。 この変更の詳細については、AccessEnabler SDK 1.7.5 に含まれるリリース ノートを参照してください。

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
