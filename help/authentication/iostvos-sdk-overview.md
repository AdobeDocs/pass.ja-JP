---
title: iOS/tvOS SDK の概要
description: iOS/tvOS SDK の概要
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '3704'
ht-degree: 0%

---

# iOS/tvOS SDK の概要 {#iostvos-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


</br>


## はじめに {#intro}

iOS AccessEnabler は、モバイルアプリで TV Everywhere の権限付与サービスにAdobe Pass認証を使用できるようにする、Objective C iOS/tvOS ライブラリです。 この実装は、 *AccessEnabler* 使用権限 API を定義するインターフェイスと *EntitlementDelegate* および *[EntitlementStatus](#ios%20entitlement%20status)* ライブラリのコールバックを記述するプロトコルトリガー。 プロトコルと共に使用されるインターフェイスは、AccessEnabler ライブラリという 1 つの共通名の下に表示されます。

## iOSと tvOS の要件 {#reqs}

iOSおよび tvOS プラットフォームとAdobe Pass認証に関する現在の技術要件については、 [プラットフォーム/デバイス/ツールの要件](#ios)を参照し、SDK ダウンロードに含まれるリリースノートを参照してください。 このページの残りの部分には、特定の SDK バージョン以降に適用される変更に関する注意事項のセクションが表示されます。 例えば、1.7.5 SDK に関する適切な注意事項を次に示します。

## ネイティブクライアントワークフローについて {#flows}

ネイティブクライアントワークフローは、通常、ブラウザーベースのAdobe Pass認証クライアントと同じか、非常に似ています。 ただし、以下に説明するように、いくつかの例外があります。

- [初期化後のワークフロー](#post-init)
- [汎用初期認証ワークフロー](#generic)
- [ログアウトワークフロー](#logout)


### 初期化後のワークフロー {#post-init}

AccessEnabler でサポートされるすべての権限付与ワークフローは、以前に [`setRequestor()`](#setReq) をクリックして、id を確立します。 この呼び出しを実行して Requestor ID を指定するのは、通常、アプリケーションの初期化/設定フェーズ中に 1 回だけにします。


iOSネイティブクライアントで、 [`setRequestor()`](#setReq)を使用する場合、次の手順を実行する方法を選択できます。

- エンタイトルメントの呼び出しをすぐに開始し、必要に応じて、静かにキューに入れることができます。

- 次の成功/失敗の確認を受け取ることができます： [`setRequestor()`](#setReq) を実装することで [`setRequestorComplete()`](#setReqComplete) コールバック。

- 上記の両方を実行できます。

成功の通知をアプリに待機させるか、 [`setRequestor()`](#setReq) または、AccessEnabler の呼び出しキュー・メカニズムに依存している場合もあります。 それ以降のすべての認証要求にはリクエスト元 ID と関連する設定情報が必要なので、 [`setRequestor()`](#setReq) メソッドは、初期化が完了するまで、すべての認証および承認 API 呼び出しを効果的にブロックします。



### 汎用初期認証ワークフロー {#generic}

このワークフローの目的は、MVPD を使用してユーザーにログインすることです。 ログインが成功した後、バックエンドサーバーはユーザーに認証トークンを発行します。 通常、認証は承認プロセスの一部として行われますが、次に、認証が分離で機能する方法を示します。また、認証手順は含まれていません。

このワークフローは、通常のブラウザーベースの認証ワークフローとはネイティブクライアントでは異なりますが、手順 1 ～ 5 は、ネイティブクライアントとブラウザーベースのクライアントの両方で同じです。

1. アプリケーションが AccessEnabler の `getAuthentication() `キャッシュされた有効な認証トークンを確認する API メソッド。
1. ユーザーが現在認証されている場合、AccessEnabler は、 [`setAuthenticationStatus()`](#setAuthNStatus) コールバック関数で、成功を示す認証ステータスを渡し、フローを終了します。
1. ユーザーが現在認証されていない場合、AccessEnabler は、特定の MVPD でユーザーの最後の認証試行が成功したかどうかを判断することで、認証フローを継続します。 MVPD ID がキャッシュされ、 `canAuthenticate` フラグが true の場合、またはを使用して MVPD が選択された場合 [`setSelectedProvider()`](#setSelProv)を指定した場合、MVPD 選択ダイアログが表示されることはありません。 認証フローは、MVPD のキャッシュされた値（最後の正常な認証時に使用されたのと同じ MVPD）を引き続き使用します。 バックエンドサーバーに対してネットワーク呼び出しがおこなわれ、ユーザーは MVPD ログインページ（以下の手順 6）にリダイレクトされます。
1. MVPD ID がキャッシュされておらず、を使用して MVPD が選択されていない場合 [`setSelectedProvider()`](#setSelProv) または `canAuthenticate` フラグが false に設定されている場合、 [`displayProviderDialog()`](#dispProvDialog) コールバックが呼び出されます。 このコールバックは、選択する MVPD のリストをユーザーに表示する UI を作成するようにアプリケーションに指示します。 MVPD オブジェクトの配列が提供され、MVPD セレクターを構築するために必要な情報が含まれます。 各 MVPD オブジェクトは MVPD エンティティを表し、MVPD の ID などの情報（XFINITY、AT\&amp;T など）を含みます。 MVPD ロゴが見つかる URL。
1. 特定の MVPD を選択したら、アプリケーションは、ユーザーが選択した内容を AccessEnabler に通知する必要があります。 ユーザが目的の MVPD を選択したら、AccessEnabler に対し、 [`setSelectedProvider()`](#setSelProv) メソッド。
1. iOS AccessEnabler が `navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` callback を使用して、ユーザーを MVPD ログインページにリダイレクトします。 AccessEnabler は、どちらかを起動すると、アプリケーションに対して、 `UIWebView/WKWebView or SFSafariViewController` コントローラー、およびコールバックの `url` パラメーター。 バックエンドサーバー上の認証エンドポイントの URL です。 tvOS AccessEnabler の場合、 [status()](#status_callback_implementation) コールバックが `statusDictionary` パラメーターと、2 番目の画面認証に対するポーリングが直ちに開始されます。 The `statusDictionary` 次を含む `registration code` これは 2 番目の画面認証に使用する必要があります。
1. iOS AccessEnabler の場合、ユーザーは MVPD のログインページに移動し、アプリケーションのメディアを通じて資格情報を入力します `UIWebView/WKWebView or SFSafariViewController `コントローラ。 この転送中にいくつかのリダイレクト操作が発生し、複数のリダイレクト操作中に、コントローラが読み込む URL をアプリケーションで監視する必要があることに注意してください。
1. iOS AccessEnabler の場合、 `UIWebView/WKWebView or SFSafariViewController` コントローラが、アプリケーションがコントローラを閉じて AccessEnabler のを呼び出す必要がある特定のカスタム URL を読み込む `handleExternalURL:url `API メソッド。 この特定のカスタム URL は実際には無効で、コントローラが実際に読み込むことを意図していないことに注意してください。 アプリケーションでは、認証フローが完了し、安全に `UIWebView/WKWebView or SFSafariViewController` コントローラ。 アプリケーションで `SFSafariViewController `コントローラーは、 `application's custom scheme` ( 例： `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`) が含まれていない場合、この特定のカスタム URL は `ADOBEPASS_REDIRECT_URL` 定数 ( `adobepass://ios.app`) をクリックします。
1. アプリケーションが閉じると、 `UIWebView/WKWebView or SFSafariViewController` コントローラと呼び出し AccessEnabler の `handleExternalURL:url `API メソッドを使用すると、AccessEnabler はバックエンドサーバから認証トークンを取得し、認証フローが完了したことをアプリケーションに通知します。 AccessEnabler が [`setAuthenticationStatus()`](#setAuthNStatus) ステータスコードが 1 のコールバック。成功を示します。 これらの手順の実行中にエラーが発生した場合、 [`setAuthenticationStatus()`](#setAuthNStatus) コールバックは、ステータスコード 0 でトリガーされ、認証の失敗を示し、対応するエラーコードも示します。


>[!WARNING]
>
> AccessEnabler がアプリに制御を放棄する手順（例：プロバイダ選択ダイアログが表示される場合、または UIWebView/WKWebView または SFSafariViewController が表示される場合）では、ユーザーは認証フローをキャンセルする機会があります。 このような状況では、アプリケーションは、このイベントを AccessEnabler に通知し、 [`setSelectedProvider()`](#setSelProv) API メソッド。null をパラメーターとして渡します。 これにより、AccessEnabler は内部状態をクリーンアップし、認証フローをリセットできます。 tvOS では、同じ方法を使用して認証ポーリングをキャンセルできます。


### ログアウトワークフロー {#logout}

ネイティブクライアントの場合、ログアウトは上記の認証プロセスと同様に処理されます。

1. アプリケーションが、AccessEnabler の `logout() `API メソッド。 ログアウトは、一連の HTTP リダイレクト操作の結果です。これは、ユーザーがAdobe Pass認証サーバーと MVPD サーバーの両方からログアウトする必要があるためです。 このフローは、AccessEnabler ライブラリによって発行された単純な HTTP リクエストでは完了できないため、 `UIWebView/WKWebView or SFSafariViewController` HTTP リダイレクト操作に従うには、コントローラをインスタンス化する必要があります。

1. 認証フローと同様のパターンを用いる。 iOS AccessEnabler は、 `navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` を作成するには、 `UIWebView/WKWebView or SFSafariViewController` コントローラー、およびコールバックの `url` パラメーター。 これは、バックエンドサーバー上のログアウトエンドポイントの URL です。 tvOS AccessEnabler の場合、 `navigateToUrl:` コールバックまたは `navigateToUrl:useSVC:` コールバックが呼び出されます。

1. 複数のリダイレクトを経る場合、アプリケーションは、 `UIWebView/WKWebView or SFSafariViewController `コントローラーを使用し、特定のカスタム URL を読み込むときのタイミングを検出します。 この特定のカスタム URL は実際には無効で、コントローラが実際に読み込むことを意図していないことに注意してください。 ログアウトフローが完了し、コントローラを安全に閉じられることを示すシグナルとして、アプリケーションでのみ解釈する必要があります。 コントローラがこの特定のカスタム URL を読み込むと、アプリケーションはコントローラを閉じて、AccessEnabler の `handleExternalURL:url `API メソッド。 アプリケーションで `SFSafariViewController `コントローラーは、 `application's custom scheme` ( 例：`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`) が含まれていない場合、この特定のカスタム URL は ` ADOBEPASS_REDIRECT_URL  `定数 ( `adobepass://ios.app`) をクリックします。

1. 最後に、AccessEnabler が [`setAuthenticationStatus()`](#setAuthNStatus) ステータスコード 0 のコールバック。ログアウトフローの成功を示します。

ログアウトフローは、ユーザーが `UIWebView/WKWebView or SFSafariViewController`  コントローラーを使用します。 したがって、Adobeでは、ログアウトプロセス中にコントロールを非表示（非表示）にすることをお勧めします。

## トークン {#tokens}

- [定義と使用方法](#definitions)
- [キャッシュのガイドライン](#caching)
- [永続性](#persistence)
- [形式](#format)
- [デバイスのバインディング](#device_binding)


### 定義と使用方法 {#definitions}

Adobe Pass Authentication Entitlement ソリューションは、認証および承認ワークフローが正常に完了したときにAdobe Pass Authentication が生成する特定のデータ（トークン）の生成に関連しています。 これらのトークンは、クライアントのiOSデバイス上にローカルに保存されます。



トークンの有効期限は限られています。有効期限が切れたら、認証ワークフローや認証ワークフローの再開を通じてトークンを再発行する必要があります。



使用権限付与ワークフローでは、次の 3 種類のトークンが発行されます。

- **認証トークン：** ユーザー認証ワークフローの最終結果は、AccessEnabler がユーザーに代わって認証クエリを実行する際に使用できる認証 GUID になります。 この認証 GUID には、ユーザーの認証セッション自体とは異なる可能性のある、有効期間 (TTL) 値が関連付けられます。 認証トークンは、認証要求を開始するデバイスに認証 GUID をバインドすることで生成されます。
- **認証トークン：** 一意の resourceID で識別される特定の保護されたリソースへのアクセスを許可します。 これは、元の resourceID と共に、承認者が発行する承認付与で構成されます。 この情報は、要求を開始するデバイスに結び付けられます。
- **短時間のみ有効なメディアトークン：** AccessEnabler は、短時間のみ有効なメディアトークンを返すことで、特定のリソースのホスティングアプリケーションへのアクセスを許可します。 このトークンは、その特定のリソースに対して以前に取得された認証トークンに基づいて生成されます。 また、このトークンはデバイスに結び付けられず、関連する寿命が大幅に短くなります（デフォルトは 5 分）。

認証と承認が成功すると、Adobe Pass認証は認証、承認および短時間のみ有効なメディアトークンを発行します。 これらのトークンは、ユーザーのデバイス上にキャッシュされ、関連するライフスパンの期間中に使用される必要があります。



### キャッシュのガイドライン {#caching}

- 認証トークン
- 認証トークン
- 短時間のみ有効なメディアトークン


#### 認証トークン

- **AccessEnabler 1.7:** この SDK は、複数の Programmer-MVPD バケット、つまり複数の認証トークンを有効にする、新しいトークン格納方法を導入します。 現在は、同じストレージレイアウトが「要求者ごとの認証」シナリオと通常の認証フローの両方で使用されています。 この 2 つの違いは、認証の実行方法です。「要求元ごとの認証」には、ストレージ内の認証トークンの存在に基づいて、AccessEnabler がバックチャネル認証を実行できる新しい改善（パッシブ認証）が含まれます。 ユーザーは 1 回だけ認証する必要があり、このセッションは、追加のアプリで認証トークンを取得するために使用されます。 このバックチャネルフローは、 [`setRequestor()`](#setReq) を呼び出し、はほとんどプログラマに対して透過的である。 **しかし、ここで重要な要件は、プログラマが setRequestor() をメインの UI スレッドから呼び出すことです。**
- **AccessEnabler 1.6 以前：** 認証トークンがデバイス上でキャッシュされる方法は、「**リクエスト元ごとの認証»** 現在の MVPD に関連付けられたフラグ：

<!-- end list -->

1. 「要求元ごとの認証」機能が無効な場合、単一の認証トークンがグローバルペーストボードにローカルに保存され、このトークンは現在の MVPD と統合されているすべてのアプリケーション間で共有されます。
1. 「要求元ごとの認証」機能が有効な場合、トークンは認証フローを実行したプログラマに明示的に関連付けられます（トークンはグローバルなペーストボードには格納されず、そのプログラマのアプリケーションにのみ表示されるプライベートファイルに格納されます）。 具体的には、異なるアプリケーション間のシングルサインオン (SSO) が無効になります。新しいアプリに切り替える際に、ユーザは認証フローを明示的に実行する必要があります（2 つ目のアプリのプログラマは現在の MVPD と統合され、そのプログラマに認証トークンは存在しません）。



#### 認証トークン

いつでも、AccessEnabler によってキャッシュされる認証トークンは PER RESOURCE に 1 つだけです。 複数の認証トークンをキャッシュすることができますが、異なるリソースに関連付けられます。 新しい認証トークンが発行され、古い認証トークンが *同じリソース*&#x200B;を指定した場合、新しいトークンは既存のキャッシュされた値を上書きします。



#### メディアトークン

短時間のみ有効なメディアトークンは、まったくキャッシュしないでください。 メディアトークンは 1 回限りの使用に制限されているので、認証 API が呼び出されるたびにサーバーから取得する必要があります。



### 永続性 {#persistence}

トークンは、同じアプリケーションの連続した実行の間、保持する必要があります。 つまり、認証および認証トークンが取得され、ユーザーがアプリケーションを閉じると、ユーザーがアプリケーションを再度開いたときに、同じトークンがアプリケーションで使用できるようになります。 さらに、これらのトークンは複数のアプリケーション間で保持することが望ましい。 つまり、ユーザーが 1 つのアプリケーションを使用して特定の ID プロバイダーにログインした後（認証および認証トークンの取得に成功）、同じトークンを別のアプリケーションで使用でき、同じ ID プロバイダー経由でログインする際に資格情報の入力を求められなくなります。 このようなシームレスな認証/承認ワークフローにより、Adobe Pass Authentication Solution が真の TV-Everywhere 実装になります。



## iOS

iOS AccessEnabler ライブラリは、トークンデータを「クリップボードに似た」データ構造 ( *貼り付け板*. このシステムレベルの共有リソースは、目的の永続的なトークンの使用例を実装できる重要な要素を提供します。

- **構造化ストレージのサポート**  — ペーストボードは単なる単純な線形のバッファ状のメモリ構造ではありません。 これは、ユーザー指定のキー値に基づいてデータのインデックスを作成できる、辞書に似たストレージメカニズムを提供します。
- **基盤となるファイルシステムを使用したデータ永続化のサポート**  — 貼り付け基板構造の内容を永続的としてマークすることができます。その場合、データはデバイスの内部メモリに保存されます。



特定のトークンがトークン・キャッシュに配置されると、その有効性が AccessEnabler ライブラリによって異なる時間にチェックされます。  有効なトークンは次のように定義されます。

- トークンの TTL が期限切れになっていません。
- トークンの発行者は、許可された ID プロバイダーのリストに含まれています。



## tvOS

tvOS ではペーストボードは使用できないので、tvOS AccessEnabler ライブラリでは、NsUserDefaults をストレージオプションとして使用します。 これにより、認証と承認トークンが保持される問題が解決されますが、保存された情報を異なるアプリケーション間で共有することはできません。



**iOS 10 ペーストボードの変更 —** 以前のバージョンのiOSからアップグレードすると、ペーストボードは消去されます。 つまり、すべてのアプリケーションを再認証する必要があります。



**iOS 7 ペーストボードの変更 —** iOS 7 でのペーストボードの機能が変更されたため、iOS 7 で実行するアプリケーション間でのクロス SSO が制限されます。 同じものを有するアプリケーション `<Bundle Seed ID>`( 別名 `<Team ID>`) はトークンを共有します。つまり、同じプログラマー X のアプリ A1 と A2 はトークンを共有し、アプリ A1（プログラマー X）とアプリ A3（プログラマー Y）はトークンを共有しません。

- バンドルシード ID/チーム ID は、同じプロビジョニングプロファイルで生成された場合、2 つのアプリ間で同じです。 詳しくは、次のリンクを参照してください。
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- この「クロス SSO」の制限は、使用するAdobe Pass Authentication SDK に関係なく、iOS 7 に存在します。

iOS 7 以降での SSO の設定については、次のテクニカルノートを参照してください（このテクニカルノートは Access Enabler v1.8 以降に適用されます）。 <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### トークンストレージ (AccessEnabler 1.7)

AccessEnabler 1.7 以降、トークンストレージは、複数の認証トークンを保持できる複数レベルのネストされたマップ構造に依存して、複数の Programmer-MVPD の組み合わせをサポートします。 この新しいストレージは、AccessEnabler パブリック API に何ら影響を与えず、プログラマ側の変更を必要としません。 次に、この新機能の例を示します。

1. App1 を開きます（Programmer1 が開発）。
1. MVPD1 （Programmer1 と統合）を使用して認証します。
1. 現在のアプリケーションを休止/閉じ、（Programmer2 が開発した）App2 を開きます。
1. Programmer2 が MVPD2 と統合されていないので、ユーザーが App2 で認証されないとします。
1. App2 で MVPD2（Programmer2 と統合）を認証します。
1. App1 に切り替えます。ユーザーは引き続き Programmer1 で認証されます。

以前のバージョンの AccessEnabler では、トークンストレージは以前 1 つの認証トークンのみをサポートしていたので、手順 6 ではユーザーが未認証として表示されました。



1 つのプログラマー/MVPD セッションからログアウトすると、デバイス上の他のすべてのプログラマー/MVPD 認証トークンを含む、基になるストレージ全体が消去されます。 一方、認証フローをキャンセルする ( 呼び出し [`setSelectedProvider(null)`](#setSelProv)) は基になるストレージをクリアしませんが、（現在のプログラマーの MVPD を消去することで）現在のプログラマー/MVPD 認証の試みにのみ影響します。



### トークンインポータ (AccessEnabler 1.7)

AccessEnabler 1.7 に含まれる別のストレージ関連機能により、古いストレージ領域から認証トークンをインポートできます。 この「トークンインポーター」は、ストレージバージョンがアップグレードされても SSO 状態を維持し、連続する AccessEnabler リリース間の互換性を実現するのに役立ちます。 インポーターは、 [`setRequestor()`](#setReq) フローとは、次の 2 つのシナリオで実行されます（現在のプログラマーの有効な認証トークンが現在のストレージに存在しない場合）。

- 特定のプログラマーが開発した 1.7 アプリの最初のインストール
- 新しいストレージを使用する将来の AccessEnabler へのパスのアップグレード

インポート操作はプログラマーに対して透過的で、クライアントアプリケーションでコードを変更する必要はありません。



### トークン・サニタイザ (AccessEnabler 1.7.5)

AccessEnabler 1.7.5 以降では、このサービスは [`setRequestor()`](#setReq)`. `これは、iOS 7 スイッチが WiFi MACアドレスから IDFA に切り替え、トラッキングのために開発されました。 サニタイザーを使用すると、現在のストレージに有効な認証トークン (iOS7 より前に、以前はMACアドレスを使用して計算されたデバイス ID に関して有効 ) のみが含まれるようになります。 Token Sanitizer は無効なトークンをすべて削除します。



Token Sanitizer サービスは、AccessEnabler 1.7.5 アプリケーションをiOS 6 で使用し、その後、ユーザーがiOS 7 にアップデートする場合に最も役に立ちます。 これが発生すると、iOS 6 で取得されたすべての認証トークンが無効になります ( デバイス ID アルゴリズムがMACアドレスの使用から IDFA に変更されるため )。 サニタイザーは無効なトークンをすべてクリアし、ユーザーは認証を取り消されます。



Token Sanitizer が無効なトークンを削除しない場合、AccessEnabler は、無効な認証トークンが存在するため、認証を取得できませんでした。 認証エラーメッセージは暗号化され、問題の原因を特定するのに過度に役立たないので、これはエンドユーザーにとってデバッグが難しいことを証明します。



### 形式 {#format}

- [AuthN トークン](#authn_token)
- [AuthZ トークン](#authz_token)
- [ショートメディアトークン](#short_token)
- [デバイスのバインディング](#device_binding)


ここでは、AuthN および AuthZ トークンの形式については、バックグラウンド情報に関してのみ記述します。 これらのトークンの構造は、いつでもAdobe Pass認証によって変更できます。 AuthN と AuthZ トークンはローカルデバイスで公開されないので、プログラマーはアプリを実装するために AuthN と AuthZ トークンの正確な構造を知る必要はありません。 Short Media トークン *次に該当* プログラマーのアプリケーションに公開される。



#### 認証トークン {#authn_token}

次に、認証トークンの形式を示します。

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

次のリストは、認証トークンの形式を示しています。

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

以下のリストは、ショートメディアトークンの形式を示しています。 このトークンは、プログラマーのアプリケーションに公開されます。 これは、正常なエンタイトルメントプロセスの最後に、プログラマーのアプリケーションに渡されます。

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

上記の XML リストで、というタグをメモします。 `simpleTokenFingerprint`. このタグの目的は、ネイティブデバイス ID の個別化情報を保持することです。 AccessEnabler ライブラリでは、こうした個別化情報を取得し、権限付与の呼び出し時にAdobe Pass Authentication サービスで使用できるようにすることができます。 この情報を使用して実際のトークンに埋め込むので、トークンを特定のデバイスに効果的にバインドします。 この最終目標は、デバイス間でトークンを転送できないようにすることです。



これは明らかにセキュリティ関連の機能なので、セキュリティの観点から言えば、この情報は本質的に「機密性」が高いものです。 その結果、この情報は改ざんや盗聴の両方から保護される必要があります。 盗聴の問題は、認証/承認リクエストを HTTPS プロトコル経由で送信することで解決されます。 改ざん保護は、デバイス識別情報にデジタル署名することにより処理される。 AccessEnabler ライブラリは、デバイスから提供された情報からデバイス ID を計算し、デバイス ID をリクエストパラメータとして「明確に」Adobe Pass認証サーバに送信します。 Adobe Pass認証サーバは、Adobeの秘密鍵でデバイス ID にデジタル署名し、AccessEnabler に返される認証トークンに追加します。 したがって、デバイス ID は認証トークンに結び付けられます。 認証フロー中、AccessEnabler は、デバイス ID を認証トークンと共にクリアで再度送信します。 検証プロセスが失敗すると、自動的に認証/承認ワークフローが失敗します。 Adobe Pass認証サーバーは秘密鍵をデバイス ID に適用し、認証トークンの値と比較します。 一致しない場合、そのエンタイトルメントフローは失敗します。



**AccessEnabler 1.7.5 でのデバイスバインディングに関する注意：** AccessEnabler 1.7.5 以降では、デバイス ID の計算方法が変更され、iOSデバイスの個人情報が提供されるようになりました。 この変更は、iOS 7 の変更を反映しています。iOS 7 以降、Appleは WiFi MACアドレスをトラッキングオプションとして提供しなくなり、広告主識別子 (IDFA) が優先されます。 iOS 7 で実行するアプリの個別化情報は IDFA に基づいており、その情報がエンタイトルメントフロートークンに埋め込まれるので、この変更によってユーザーエクスペリエンスに様々な影響が及ぶ可能性があります。 考えられる様々な影響は、ユーザーがアップグレードするiOSのバージョンと、プログラマがアップグレードする AccessEnabler のバージョンを組み合わせたバージョンに基づいています。 この変更の詳細については、AccessEnabler SDK 1.7.5 に含まれるリリースノートを参照してください。

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
