---
title: プログラマー向けの概要
description: プログラマー向けの概要
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# プログラマー向けの概要 {#programmers-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#introduction}

この概要は、Adobe® パスを自社の web サイトまたはアプリケーションに統合することを計画しているコンテンツプロバイダー（プログラマー）を対象としています。 キックスタート ガイドや統合ガイドなどの追加ドキュメントについては、以下の関連情報を参照してください。

現在、視聴者は、いつでもどこでもインターネットにアクセスでき、保護されたコンテンツへの直接アクセスをプログラマーにリクエストできます。 1 回限りのイベントを視聴したい場合や、放送しているテレビシリーズ全体の視聴権を求めている場合があります。

ただし、保護されたコンテンツへのアクセスを許可する前に、顧客がそのコンテンツを表示する権限を持っているかどうかを判断する必要があります。 Multichannel Video Programming Distributor （MVPD）のサブスクリプションはありますか。 その場合、そのサブスクリプションにはプログラミングが含まれていますか？

プログラマーにとって、ビューアの使用権限を判断することは必ずしも簡単ではありません。 MVPD には、顧客の識別データとアクセス権限が保持されます。  保護されたコンテンツにアクセスしようとする視聴者は、それぞれ異なるシステムを持つ様々な MVPD にサブスクライブします。保護されたコンテンツに対する視聴者の使用権限を決定することは、すぐに複雑で技術的に困難になる可能性があることは簡単に理解できます。

![](../assets/user-ent-by-progr.png)

*図：プログラマーが直接決定したユーザーの使用権限*

Adobe Pass Authentication for TV Everywhere は、プログラマーと MVPD の間のこれらの権利取引を安全に仲介します。 Adobe Pass認証を使用すると、プログラマーは保護されたコンテンツを簡単、迅速、安全に有効なお客様に提供できます。

![](../assets/user-ent-mediatedby-authn.png)

*図：Adobe Pass認証を介したユーザーエンタイトルメント*

Adobe Pass認証は、関与する MVPD とやり取りする際のプロキシとして機能するので、一貫したクロスサイトインターフェイスでビューアを表示できます。 Adobe Pass認証を使用すると、ビューアにシングルサインオン（SSO）の認証と承認を提供することもできます。 すべての参加サービスについて認証と承認が追跡されるので、サブスクライバーは最初の認証後に自分のシステムで再度ログインする必要はありません。

* **認証** – 特定のユーザーが既知の顧客であることを MVPD で確認するプロセス。
* **Authorization** – 認証されたユーザーが指定されたリソースへの有効なサブスクリプションを持っていることを MVPD で確認するプロセス。

### Adobe Pass認証の仕組み {#HowItWorks}

プログラマーのコンテンツ表示アプリケーションは、アクセスイネーブラ クライアントコンポーネントまたはクライアントレス API の RESTful web サービスを使用してAdobe Pass認証とやり取りします（スマート TV、ゲーム機、セットトップボックスなどの非 web 対応デバイスの場合）。 Access Enabler はユーザーのシステム上で動作し、すべての使用権限ワークフローを容易にします。  Access Enabler コンポーネントは、お客様がサイトにアクセスして保護されたコンテンツをリクエストすると、Adobeのホスティング・サイトからダウンロードされます。  Adobe Pass認証サーバーは、クライアントレスソリューションで使用される RESTful web サービスをホストします。

Adobe Pass認証は、実際の使用権限ワークフローを処理すると同時に、次の目的で使用するプリミティブを提供します。

* ID を設定します。 （プログラマーは、Adobe Pass認証の使用権限フローの「リクエスター」です。）
* 特定の MVPD でユーザーを認証します。  （MVPD は、Adobe Pass認証使用権限フローの「ID プロバイダー」または「IdP」です。）
* 特定のリソースの MVPD を使用してユーザーを認証します。
* ユーザーをログアウトします。

プログラマーは、次の作業を行う上位レベルの web ページまたはプレーヤーアプリケーションを担当します。

* ユーザーインターフェイスを実装します
* Access Enabler または Clientless API Web サービスとの対話

Adobe Pass Authentication の目標は、プログラマーと MVPD の両方がエンタイトルメントの検証を処理するためのシンプルなモジュール型の方法を作成することです。

## トークンについて {#understanding-tokens}

Adobe Pass認証使用権限ソリューションは、認証および承認ワークフローが正常に完了したときに作成される特定のデータを中心に展開されます。 これらのデータはトークンと呼ばれます。 トークンの有効期限は限られています。期限が切れたら、認証ワークフローと承認ワークフローを再開してトークンを再発行する必要があります。

トークンについて詳しくは、次の節を参照してください。

* [トークンのタイプ](#token-types)
* [トークンストレージ](#token-storage)
* [トークンセキュリティ](#token-security)
* [トークン共有](#token-sharing)

### トークンのタイプ {#token-types}

認証および承認のワークフロー中に 3 種類のトークンが発行されます。 AuthN および AuthZ トークンは「長期間有効」で、ユーザーの閲覧エクスペリエンスの継続性を提供します。 メディアトークンは、ストリームリッピングによる不正を防ぐための業界のベストプラクティスに対するサポートを提供する短時間のみ有効なトークンです。 プログラマーは、MVPD との契約に基づいて、トークンのタイプごとに Time-to-Live （TTL）値を指定します。 プログラマーは、ビジネスと顧客に最適な TTL 値を決定します。

* **AuthN トークン** （「長期間有効」）：認証に成功すると、Adobe Pass認証は要求元デバイスとグローバル一意識別子（GUID）の両方に関連付けられた AuthN トークンを作成します。
   * Adobe Pass Authentication は、AuthN トークンをアクセス イネーブラに送信します。アクセス イネーブラは、クライアントのシステムに安全にトークンをキャッシュします。  AuthN トークンが存在し、期限切れでない間は、Adobe Pass Authentication を使用するすべてのアプリケーションで使用できます。 アクセス イネーブラは、認証フローに AuthN トークンを使用します。
   * どの時点でも、1 つの AuthN トークンのみがキャッシュされます。 新しい AuthN トークンが発行され、古いトークンが既に存在する場合は、Adobe Pass認証によって、キャッシュされたトークンが上書きされます。
* **AuthZ トークン** （「長期間有効」）：認証に成功すると、Adobe Pass Authentication は要求元デバイスと特定の保護されたリソースに関連付けられた AuthZ トークンを作成します。  保護されたリソースは、一意のリソース ID で識別されます。
   * Adobe Pass認証は、AuthZ トークンをアクセス イネーブラに送信します。アクセス イネーブラは、ローカル システムに安全にトークンをキャッシュします。 次に、アクセス イネーブラは AuthZ トークンを使用して、実際の表示アクセスに使用される短時間のみ有効なメディア トークンを作成します。
   * 常に、リソースごとに 1 つの AuthZ トークンのみがキャッシュされます。 Adobe Pass認証は、異なるリソースに関連付けられている場合、複数の AuthZ トークンをキャッシュできます。 新しい AuthZ トークンが発行され、同じリソースに古いトークンが既に存在する場合、Adobe Pass認証は、キャッシュされたトークンを上書きします。
* **メディアトークン** （「短時間のみ」）：アクセスイネーブラは、AuthZ トークンを使用して、短時間のみ有効な（デフォルト：7 分）メディアトークンを生成します。 これは、再生リクエストが成功した時点です。
   * 保護されたリソースへのアクセスを提供する前に、メディアサーバーはAdobe Pass認証コンポーネントであるメディアトークン検証子を使用して、メディアトークンを検証する必要があります。
   * メディアトークンはデバイスにバインドされないので、有効期間は長期間有効な AuthN および AuthZ トークンよりも大幅に短くなります（デフォルトは 7 分）。
   * 短時間のみ有効なメディアトークンは 1 回限りの使用に制限され、キャッシュされません。 認証 API が呼び出されるたびに、Adobe Pass Authentication Server から取得されます。

### トークンストレージ {#token-storage}

アクセス イネーブラは、次の環境に固有の場所に長期間有効なトークン（AuthN および AuthZ）を格納します。

* **Flash 10.1** （またはそれ以降）：長期間有効なトークンは、ローカル共有オブジェクトとして保存されます。
* **HTML5**：長期間有効なトークンは、HTML5 ブラウザーのローカルストアに安全に保持されます。
* **iOS**：長期間有効なトークンは、永続的なペーストボードに保存され、他のAdobe Pass Authentication クライアントアプリケーションからアクセスできます。
* **Android**：長期間有効なトークンは、共有データベースファイルに保存され、他のAdobe Pass Authentication クライアントアプリケーションからアクセスできます。
* **クライアントレス API デバイス**：トークンは、Adobe Pass Authentication Server に保存されます。

### トークンセキュリティ {#token-security}

Adobe Pass Authentication Server は、（デバイスのハードウェア特性から派生した）デバイス ID を使用して、長期間有効なすべてのトークンにデジタル署名を行います。 電子署名の生成、保護、検証の方法は、環境によって異なります。

* **Flash 10.1** （またはそれ以降） - Adobe ID は、デバイス資格情報（デバイスの個人化サーバーから発行された一意の証明書）に依存します。 このセキュリティは FAXS DRM テクノロジと同等です。 このサーバーサイド検証では、トークン内の一意のデバイス ID をデバイスの資格情報と比較します（Flash PlayerからAdobe Pass認証に安全に通信されます）。 また、デバイス認証情報は、FAXS クライアントのバージョンと、その発行元のFlash Player（またはAIR）のバージョンも識別します。 デバイスバインディングはHTML 5 よりも強いので、トークンの有効期間（TTL）は通常、Flashの場合より長くなります。
* **HTML5** - デバイスはクライアント側で個別に作成されます。 JavaScriptを通じて使用可能な特性を使用して、ブラウザーおよび OS バージョン、IP アドレス、ブラウザーの Cookie GUID （グローバルに一意の ID）を含む擬似デバイス ID を作成します。 このトークンデバイス ID は、デバイスの現在の疑似デバイス ID と比較される。 IP アドレスは通常の使用時に変わることがあるので、同じセッションでも、Adobe Pass認証はHTML5 トークンを 2 つの場所（localStorage と sessionStorage）に保存します。 IP が変更され、sessionStorage トークンが有効なままの場合、セッションは維持されます。 HTML 5 の場合、デバイスのバインディングはそれほど強くないので、トークンの TTL は通常、Flashの TTL よりも短くなります。
* **ネイティブクライアント** （iOSおよびAndroid） – 長期間有効なトークンは、ネイティブデバイス ID の個別化情報を保持するので、要求元デバイスにバインドされます。 認証および承認リクエストは HTTPS 経由で送信され、デバイス ID 情報は、バックエンド サーバーに送信する前に、アクセス イネーブラ ライブラリによってデジタル署名されます。 サーバー側では、デバイス ID 情報は、関連するデジタル署名に対して検証されます。
* **クライアントレス API クライアント** - クライアントレス API ソリューションには、すべての API 呼び出しのデジタル署名に関連する一連のセキュリティプロトコルがあります。 使用権限フロー中に生成されたトークンは、Adobe Pass Authentication Server に安全に保存されます。

Adobe Pass認証では、長期間有効な各トークンを検証し、コンテンツにアクセスするデバイスがトークンを発行したデバイスと同じであることを確認します。 すべてのトークンについて、クライアントサイドの検証により、デジタル署名が損なわれず、トークンの整合性が保持されます。 デバイス ID の検証に失敗すると、認証セッションが無効化され、ユーザーは再度ログインするように求められます。これにより、トークンがリセットされます。

### トークン共有 {#token-sharing}

異なるプラットフォーム上のアプリケーションはトークンを共有しません。 これには、次のようないくつかの理由があります。

* [ トークンストレージ ](#token-storage) で説明されているように、トークンの保存方法はプラットフォームによって異なります（Flash用のローカル共有オブジェクト、JavaScript用の WebStorage など）。
* トークンのセキュリティの程度は、プラットフォームによって異なります。 例えば、Flashトークンは、FAXS を使用してデバイスに強くバインドされます。 純粋なJavaScriptFlash内のトークンは、環境と同じレベルの DRM サポートを持ちません。  JS トークンをFlashアプリケーションと共有すると、より安全な環境を悪用して、安全性の低いトークンが使用される可能性が高くなります。

## プログラマ統合のライフサイクル {#prog-integ-lifecycle}

![](../assets/progr-flow-int-lifecycle.png)

*図：認証をプログラマーの web サイトおよびアプリケーションと統合する*


## 論理フロー {#logical-flows}

### 使用権限のフローチャート {#chart}

次のフローチャートは、使用権限を確認する全体的なプロセスを示しています（Adobe Pass認証アクセスイネーブラ クライアントコンポーネントを使用）。

![](../assets/ent-flowchart.png)

*図：使用権限の確認プロセス*

### 認証手順 {#authn-steps}

次の手順は、Adobe Pass認証認証フローの例を示しています。  これは、ユーザーが MVPD の有効な顧客であるかどうかをプログラマーが判断する使用権限プロセスの一部です。  このシナリオでは、ユーザは MVPD の有効なサブスクライバです。  保護されたコンテンツをプログラマーのFlashアプリケーションを使用して表示しようとしています：

1. ユーザーは、プログラマーの Web ページを参照します。このページは、プログラマーの認証アプリケーションとAdobe PassFlashアクセスイネーブラ コンポーネントをユーザーのマシンに読み込みます。 Flashアプリケーションは、アクセス イネーブラを使用して、プログラマの ID をAdobe Pass Authentication に設定し、Adobe Pass Authentication は、アクセス イネーブラに対して、そのプログラマ（「リクエスタ」）の構成データと状態データを準備します。 アクセス イネーブラは、他の API 呼び出しを実行する前にサーバからこのデータを受信する必要があります。 テクニカル ノート：プログラマは Access Enabler の `setRequestor()` メソッドを使用して ID を設定します。詳細については、「[ プログラマ統合ガイド ](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md) を参照してください。
1. ユーザーがプログラマーの保護されたコンテンツを表示しようとすると、プログラマーのアプリケーションがユーザーに MVPD のリストを表示し、ユーザーはそこからプロバイダーを選択します。
1. ユーザーはAdobe Pass Authentication Server にリダイレクトされ、そこでユーザーが選択した MVPD に対する暗号化された [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) リクエストが作成されます。 この要求は、プログラマの代わりに認証要求として MVPD に送信されます。 MVPD のシステムに応じて、ユーザーのブラウザーはログインするために MVPD のサイトにリダイレクトされるか、プログラマーのアプリにログイン iFrame が作成されます。
1. どちらの場合（リダイレクトまたは iFrame）でも、MVPD はリクエストを受け入れ、ログインページを表示します。
1. ユーザーが MVPD を使用してログインすると、MVPD はユーザーのステータスを有料顧客として検証し、MVPD は独自の HTTP セッションを作成します。
1. ユーザーが検証されると、MVPD は応答（SAML および暗号化）を作成し、MVPD はこれをAdobe Pass Authentication に返します。
1. Adobe Pass認証が MVPD 応答を受信し、Adobe Pass認証 HTTP セッションが開いていることを確認し、MVPD からの SAML 応答を検証し、プログラマーのサイトにリダイレクトします。
1. プログラマのサイトが再ロードされ、アクセス イネーブラが再ロードされた後、プログラマは setRequestor （）を再度呼び出します。  現在の構成が変更されているため、setRequestor （）の 2 番目の呼び出しが必要です。サーバで AuthN トークンが生成されるのを待っていることをアクセス イネーブラに通知するフラグが存在します。
1. アクセス イネーブラは、保留中の認証があることを確認し、Adobe Pass Authentication Server からトークンをリクエストします。 トークンは、サーバーの DRM 機能を呼び出すことにより、Flash Playerから取得されます。
1. AuthN トークンはプログラマーのFlash Player LSO キャッシュに格納されます。これで認証が完了し、セッションはAdobe Pass Authentication Server で破棄されます。

### 認証手順 {#authz-steps}

以下の手順は、引き続き [ 認証手順 ](#authn-steps) から説明します。

1. ユーザーがプログラマーの保護されたコンテンツにアクセスしようとすると、プログラマーのアプリケーションはまず、ユーザーのローカルマシンまたはデバイス上の AuthN トークンを確認します。  そのトークンがない場合は、上記の [ 認証手順 ](#authn-steps) に従います。  AuthN トークンが存在する場合、認証フローでは、プログラマのアプリケーションがアクセス イネーブラへの呼び出しを開始し、保護されたコンテンツの特定の項目に対するユーザーの閲覧権限を取得するリクエストを送信します。
1. 保護されたコンテンツの特定の項目は、「リソース識別子」で表されます。  これは、単純な文字列や、より複雑な構造になる可能性がありますが、いずれにせよ、プログラマと MVPD の間でリソース識別子の性質が事前に合意されています。  プログラマのアプリケーションは、リソース識別子をアクセス イネーブラに渡します。  アクセス イネーブラは、ユーザーのローカル マシンまたはデバイス上の AuthZ トークンをチェックします。  AuthZ トークンがない場合、アクセス イネーブラはバックエンドのAdobe Pass Authentication Server にリクエストを渡します。
1. Adobe Pass Authentication Server は、標準化されたプロトコルを使用して MVPD 認証エンドポイントと通信します。  MVPD の応答で、保護されたコンテンツを表示する権限がユーザーに与えられていることが示された場合、Adobe Pass Authentication Server は AuthZ トークンを作成して、ユーザーのマシン上に AuthZ トークンを格納するアクセスイネーブラに戻します。
1. ユーザーのマシンまたはデバイスに保存されている AuthZ トークンを使用して、プログラマーのアプリケーションは、アクセス・イネーブラを呼び出してAdobe Pass認証サーバからメディア・トークンを取得し、そのトークンをプログラマーのアプリケーションに提供します。
1. 最後に、プログラマーのアプリケーションは、メディアトークン検証子コンポーネントを使用して、適切なユーザーが適切なコンテンツを表示していることを確認し、メディアトークンを配置した状態で、ユーザーは保護されたコンテンツを表示できます。


## 登録と初期化 {#reg-and-init}

Adobe Pass認証を使用する最初の手順は、AdobeまたはAdobe Pass認証承認済みパートナーに登録することです。

登録時に、Adobe Pass認証と通信するドメインのリストを指定します。 例えば、Turner Broadcasting System ドメインには、tbs.comと tnt.tv がAdobe Pass認証登録ドメインとして含まれています。 これらの各コンテンツサイトにはAdobe Pass Authentication へのアクセス権が付与され、独自のリクエスター ID （例：「TBS」や「TNT」）が割り当てられます。 サイトを追加する場合は、追加のドメイン名をAdobeに知らせて、許可されるドメインのリストに追加し、追加のリクエスター ID を付与する必要があります。

>[!WARNING]
>
>統合をテストする際は、実稼動環境で使用する登録済みドメインにテストサーバーがあることを確認します。 許可リストに登録されていないドメインからのリクエスト（テストリクエストを含む）は無視されます。 例えば、domain.comを実稼動環境で使用するようにリクエストした場合は、テスト統合をdomain.com、test.domain.com、staging.domain.comの下にデプロイしていることを確認します。
>
>ドメインが許可リストに登録されている場合でも、URL にユーザー名やパスワードが含まれるリクエストは無視されます。 例：`//username@registered-domain/`

要求者 ID は、Adobe Pass認証のアクセス イネーブラ クライアント コンポーネントとのすべての通信で、プログラマのクライアントを一意に識別します。 プログラマーに関連付けられたすべてのAdobe Pass認証の静的データは、この ID にキーが設定されます。

>[!TIP]
>
>要求者 ID に加えて、登録時には、アクセス・イネーブラ・クライアント・コンポーネントおよびメディア・トークン・ベリファイアの機能 URL も受け取ります。

## 統合手順 {#integration-steps}

>[!TIP]
>
>Media Player の開発にAdobeのOpen Source Media Framework（「OSMF」）を使用している場合、Adobe Pass認証を使用する最も簡単な方法は、OSMF プラグイン *（非推奨）* をプレーヤーのコードに組み込むことです。
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [要求者設定](#requestor)
1. [認証と承認の処理](#authn-authz)
1. [シングルログアウトのサポート](#ssl)

### 1.依頼者の設定 {#requestor}

#### 1a. Adobeへの登録

最初の手順は、AdobeまたはAdobe Pass認証承認済みパートナーに登録することです。  登録時には、1 つ以上のグローバル一意識別子（GUID）が発行されます。 発行される各 GUID は、Adobe Pass認証へのアクセスが許可されているドメインに関連付けられています。 要求元ドメインに GUID （要求者 ID と呼ばれます）を渡して、アクセス イネーブラを操作する各セッションの ID を登録します。 詳細については、『プログラマ統合ガイド』の [ 登録と初期化 ](#reg-and-init) を参照してください。

#### 1b. Initial Access Enabler Integration

次のステップでは、既存の Media Player アプリまたは Web ページに Access Enabler を統合します。

* Flashバージョン `AccessEnabler.swf` をFlashベースのビデオプレーヤーに埋め込むことも、web ページのHTMLに直接埋め込むこともできます。 Access EnablerSWFとの通信は、ActionScriptまたはJavaScriptで行えます。 ベース API はActionScriptですが、JavaScriptで作業する場合は、ページに含める完全なラッパーライブラリを使用できます。
* Flash以外の環境では、次の操作を実行できます。
   * HTML 5/JavaScript バージョンの AccessEnabler.js を使用し、JavaScript API を通じてやり取りします。
   * AIR Native Extension for Adobe Pass Authentication を使用して、ネイティブコードを組み込みのActionScriptクラスと組み合わせます
   * Access Enabler ライブラリ（iOSまたはAndroid）のネイティブ・クライアント・バージョンのいずれかを使用します

### 2.認証・承認対応 {#authn-authz}

#### 2a. アクセス・イネーブラとの通信

Access Enabler と Web ページまたはプレイヤーアプリとの通信は非同期です。 アプリケーションが Access Enabler メソッドを呼び出すと、Access Enabler は Access Enabler ライブラリに登録したコールバックを介して応答します。

* アプリケーションが認証リクエストを行うと、有効な AuthN トークンがローカル・システムにない場合は、Access Enabler が自動的に認証リクエストを開始します。 認証に成功すると、ユーザーの AuthN トークンがローカルに保存されるので、再度ログインする必要がなくなります。 その他のコンテキスト（たとえば、MVPD Web サイトや別のプログラマ）でAdobe Pass Authentication を使用して正常に認証された場合、アクセス イネーブラはローカルの AuthN トークンにアクセスでき、追加の認証は必要ありません。
* 保護されたコンテンツにユーザーがアクセスしようとすると、Access Enabler に認証リクエストが送信されます。 アクセス イネーブラは、認証の検証後（または認証の開始後）、MVPD に（Adobe Pass認証サーバ経由で）アクセスし、保護されたコンテンツを表示する権利がお客様に与えられているかどうかを判断します。 アプリケーションは、リクエストをアクセス イネーブラに送信し、応答（認証の成功または失敗）を処理するだけで済みます。 認証に成功すると、AuthZ トークンがクライアントシステムに保存されます。  最後に、アプリケーションは、独自の認証手順で使用するための短期間有効なメディアトークンを受け取ります。

>[!NOTE]
>
>* 認証は、サービスプロバイダー（SP）としてのAdobe Pass Authentication と ID プロバイダー（IdP）としての MVPD の間で SAML 交換として行われます。
>
>* 認証では、Adobe Pass認証（SP）と MVPD （IdP）の間でバックチャネル（サーバー間）の web サービス交換が使用されます。


#### 2b. 使用権限ユーザーインターフェイスの提供 {#entitlement-ui}

コンテンツへのユーザーアクセス用に独自の UI を提供します。 実際のログインプロセスなど、一部の要素は MVPD によって提供されます。また、一部の要素はオプションでAdobe Pass Authentication の一部として使用できます。 少なくとも、次の操作を行います。

* **新規ユーザーが MVPD を識別し、初めてログインできるようにする MVPD 選択インターフェイスを実装する**。 開発の場合、Access Enabler は基本的なユーザー・インタフェースを提供し、ユーザーが MVPD を選択できるようにし、ログイン・プロセスを開始します。 実稼動の場合、独自の MVPD セレクターダイアログを実装する必要があります。 MVPD の中には、ログインのために自分のサイトにリダイレクトするものもあれば、iFrame 内にログインページを表示する必要があるものもあります。 ユーザーの MVPD が iFrame にログインページを表示する場合に対処するには、この iFrame を作成するコールバックを実装する必要があります。
* **保護されたコンテンツの識別**。 保護されたコンテンツへのアクセスには認証が必要です。 インターフェイスは、保護されるコンテンツと許可されているコンテンツを示す必要があります。  認証ステータスは、多くの場合、「ロック解除」アイコンと「ロック済み」アイコンで示されます。
* **ユーザーが認証されていることを示します**。 保護されたコンテンツの識別に使用する手段の一部として、ユーザーの認証ステータスを指定する必要があります。 Access Enabler に対してクエリーを実行すると、お客様がすでに認証済みかどうかを確認できます。

#### 2c. メディアトークンベリファイアの統合 {#int-media-token-ver}

Adobe Pass認証メディアトークン検証機能コンポーネントをメディアサーバーに組み込む必要があります。  これにより、既存のトークン検証機能が、Adobe Pass Authentication から提供される短時間のみ有効なメディアトークンを認識して、正常な認証を行うことができます。 メディアトークン検証機能は、保護されたコンテンツへのユーザーアクセスを提供する前に、最後のセキュリティステップとしてメディアトークンを検証します。 Adobeに登録すると、メディアトークン検証機能をダウンロードできる場所が表示されます。

### 3. シングルログアウトのサポート {#ssl}

ほとんどの場合、メディアプレーヤーは、シンプルな Access Enabler API を介してユーザーのログアウトを処理します。 logout （）を呼び出すと、Access Enabler は次の処理を実行します。

* 現在のユーザーをログアウトします
* ログアウトしたユーザーの認証情報と承認情報をすべてクリアします
* ユーザーのローカルシステムからすべての AuthN および AuthZ トークンを削除します。

トークンが期限切れになるまでマシンをアイドル状態のままにしておいても、ユーザーはセッションに戻り、ログアウトを正常に開始できます。 Adobe Pass Authentication は、すべてのトークンが削除されることを確認し、セッションを削除するように MVPD に通知します。

Adobe Pass Authentication と統合されていないサイトからログアウトが開始された場合、MVPD は、ブラウザーリダイレクトを通じてAdobe Pass Authentication Single Logout サービスを呼び出すことができます。

## ユーザー ID について {#user-ids}

概念的には、エンタイトルメントフローを開始する各ユーザーは、1 つの一意のユーザー ID に関連付けられます。  ただし、使用権限フローの過程を通じて、ID を取得する API に応じて、1 つのユーザー ID を様々な方法で表示できます。

Short Media トークンの sessionGUID は、安全な形式の UserID で、sendTrackingData （）呼び出しで使用できます。   現在のすべての統合では、これは、時間とデバイスをまたいだユーザーの永続的な GUID ですが、GUID のソースは、MVPD からの SAML 応答の UserID で始まります。   ただし、一部の MVPD は、将来、考えを変え、一時的な GUID の送信を開始する可能性があります。  プログラマが AuthN 応答の MVPD ソース UserID が永続的であることを確認する場合は、MVPD との契約でその ID を手配する必要があります。

Adobe Pass Authentication API では、次のようにユーザー ID を様々な方法で表します。

* `sendTrackingData()` プロパティ - MVPD UserID のAdobe ハッシュ バージョンです。  ハッシュ化されているので、このユーザー ID を MVPD からソースに追跡することはできません。   この ID は一意で、通常は永続的ですが、特定の使用状況の動作と MVPD 側の動作を比較するために MVPD と共有することはできません。   デジタル署名されていないので、不正防止にはスプーフィングできませんが、分析には十分です。  この形式のユーザー ID は、Adobe Pass認証が AuthN/AuthZ フローで生成するすべてのイベントに対してクライアントサイドで提供されます。
* ショートメディアトークンの `sessionGUID` プロパティ - `sendTrackingData()` 経由のユーザー ID と同じですが、このプロパティは整合性を保護するためにデジタル署名されています。  そのため、同時使用の不正トラッキングには、この値で十分です。 これは、当社のバリデーターライブラリを使用した後にサーバー側で処理することを目的としており、ビデオストリームをクライアントにリリースする前に不正パターンの分析が可能です。  これらのタスクを実行するのは、プログラマーの責任です。
* `getMetadata() userID `property – このプロパティを使用すると、Adobeは実際のソース MVPD ユーザー ID をプログラマーに公開できます。 プログラマーから取得した証明書の公開鍵で暗号化されるため、クライアントに公開されません。 これにより、プログラマーは MVPD からの実際のユーザー ID を得ることができるので、MVPD とのアカウントのリンクや不正調査に直接使用できます。

**まとめ**

* MVPD User ID は、通常、保証はされませんが、永続的な一意の ID で、**認証が成功した場合に MVPD から生成され、Adobeに渡されます**。 通常、一部の例外を除き、すべてのネットワークで一貫しています。
* MVPD ユーザー ID には PII が含まれておらず、アカウント番号ではありません。 PII が送信されていないことをすべての MVPD で検証したので、暗号化された形式で公開する必要はありません。


ユーザー ID の使用方法は、使用例によって異なります。

* トラッキング/分析に必要な場合、最も実用的な場所は `sendTrackingData()` から取得することです。
* ストリームのリリース、詐欺、または運用データのためにサーバーサイドで必要な場合は、メディアトークンバリデーターから取得できます。
* アカウントの連携や不正防止に役立つ情報が必要な場合は、Adobeの担当者にお問い合わせください。

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
