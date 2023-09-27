---
title: MVPD の概要
description: MVPD の概要
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2736'
ht-degree: 0%

---

# MVPD の概要 {#mvpd-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#intro}

この概要は、マルチチャネルビデオプログラミングディストリビュータ (MVPD) を対象としています。 キックスタートおよび統合ガイドを含む追加のドキュメントについては、このドキュメントの最後にある関連情報の節を参照してください。



TV Everywhere (TVE) は、Pay TV の購読者が既に支払ったコンテンツを、家の内外の複数のデバイスをまたいでアクセスできるようにする、業界でよく知られている動きです。  TVE は、Pay TV プロバイダに対して、既存の顧客との関係を保ち、新しい顧客との関係を可能にする新しい機会を生み出します。 しかし、これらの機会と共に、課題が生じます。 TVE の状況では、Programmers がコンテンツを提供しますが、MVPDs は、見込み客が有効な購読者であることを確認するための顧客情報を保持します。



1 人のプログラマーとのビューア認証と認証の調整は簡単ですが、数十や数百の異なるプログラマとの調整はますます複雑になります。 しかし、Adobe® Pass では、MVPD は NBCUniversal Media、Turner Broadcasting(TBS、TNT、CNN)、Fox Broadcast Networks、Hulu などのプログラマを含む TVE エコシステム全体にアクセスするために、単一の簡単な統合を実装するだけで済みです。  Adobe Pass認証は、ユーザーの使用権限の決定を簡単かつ安全にする統合フレームワークを提供します。



最終的な行： Adobe Pass Authentication は、Programmers と MVPDs の間の権利付与トランザクションを安全に仲介し、購読コンテンツへのビューアのアクセスを容易にします。 つまり、Adobe Pass認証を使用すると、適切な顧客が適切なコンテンツに簡単かつ迅速にアクセスできます。


Adobe Pass Authentication を使用すると、MVPD は以下を受け取ります。

プログラマーとの簡単な統合。  1 つの統合で、複数のコンテンツ所有者から即座に接続を提供

顧客エンゲージメントの強化。  顧客が複数のプラットフォームやデバイスでコンテンツを閲覧する際に、スムーズでブランド化されたエクスペリエンスをサポートします。

認証を保護します。  許可されたユーザーとデバイスのみがプレミアムコンテンツへのアクセス権を付与され、（オプションで）世帯アカウントごとに接続できるデバイスと同時ストリームの数を制限します。

## FAQ {#faq}

Adobe Pass認証のセキュリティはどの程度ですか？ Adobe Pass認証アーキテクチャの第 1 の優先事項は、許可された閲覧者のみが認証され、プレミアムコンテンツへのアクセス権が付与されるようにすることです。 Adobe Pass認証は、表示デバイスへのアクセスを緊密に結び付け、特定の世帯のストリーム、セッションおよび/またはデバイスを制限するのに役立ちます。


Flash Playerは必要ですか？ TV Everywhere のAdobe Pass認証は、プレーヤーとプラットフォームに依存せず、Silverlight や Platform5 などの再生アプリケーションとの統合に役立ちます。 さらに、Adobe Pass認証では、iOSおよび Android を実行する携帯電話やタブレットなどのデバイスをネイティブでサポートしています。


Adobe Pass Authentication はどのデバイスをサポートしていますか。 Adobe Pass認証は、ブラウザー内表示エクスペリエンス用のHTML5 Web キットを備えた、ほぼすべてのデバイスでサポートされています。 さらに、Adobe Pass Authentication では、iOS、Android™、Xbox360（非推奨）、Xbox Air®（非推奨）アプリケーションなど、様々なデバイス固有のプラットフォーム向けに、ネイティブのソフトウェア開発キット (SDK) を引き続き展開しています。 最近では、Adobe Pass認証は、ブラウザーページをレンダリングできないデバイス（「スマート」TV、セットトップボックス、ゲームコンソールなど）向けに、クライアントレスソリューションを提供しました。  ブラウザーページをレンダリングする機能は、MVPD を使用してユーザーを認証するための要件です。


Adobe Pass Authentication は、TV Everywhere の新しい標準をサポートしていますか。 Adobe Pass認証は、CableLabs OLCA(Online Content Access) の仕様に準拠しており、オンラインソースから Pay TV のお客様にビデオを配信するための技術的要件とアーキテクチャを提供します。 Adobeは、2011 年 6 月に共同の CableLabs 社間テストプロジェクトに参加し、サービスプロバイダの実装のテストプロセスに合格しました。 Adobe Pass認証は、認証用の OLCA 仕様に対して検証（完全およびテスト）されます。 承認コンポーネントは完了しましたが、テスト検証は現在、CableLabs テスト環境のリリースを待っています。 Adobeは、OATC(Open Authentication Technical Consortium) の活動的なメンバーでもあり、その本体の一部として、サブコミッティの仕様 — 製図プロジェクトのいくつかに参加しています。



認証とは 認証とは、MVPD が特定のユーザが既知の顧客であることを確認するプロセスです。



認証とは 認証とは、認証されたユーザーが特定のリソースに対する有効なサブスクリプションを持っていることを MVPD が確認するプロセスです。



## アーキテクチャ {#architecture}

Adobe Pass認証は、MVPD と Programmers の両方で必要とされるビジネスルールに基づいて、迅速なバックエンド（サーバー間）統合を可能にするホスト型サービスです。 これは、すべての関係者に対する迅速な市場投入、不正を防ぐためのより安全な環境、優れた顧客体験を意味し、より多くのプラットフォームにわたってより多くの人々が利用できるテレビコンテンツを提供します。


Adobe Pass認証は、Software as a Service(SaaS) モデルを介して提供され、コンテンツに対する使用権限を検証するために、エンドユーザー、MVPD、プログラマー間でより安全な通信を実行できます。 このサービスのコアコンポーネントには、次のものが含まれます。

サーバー側 — ホストされているAdobe Pass認証サーバー。 これは、MVPD の認証システムとのバックチャネル（サーバ間）通信を行うアプリケーションサーバです。
クライアント側：クライアント側の Access Enabler - Access Enabler は、プログラマーの Web ページまたはプレーヤーアプリケーションに読み込まれる小さなファイルです。 プログラマーのコンテンツ表示アプリケーションに対してエンタイトルメント API を提供し、Adobe Pass Authentication Server と通信します。
クライアントレス Web サービス（非 Web 対応デバイス用） — スマート TV、ゲームコンソール、セットトップボックスなどのデバイスのエンタイトルメント API を提供する RESTful Web サービス。

>[!NOTE]
>
>MVPD の場合、Web サービスは、Adobe Pass Authentication からの認証および承認のリクエストを認識し、必要なデータを想定された形式で応答できる必要があります。
>

Adobe Pass認証を使用すると、フェデレーテッド ID 管理 ( シングルサインオン (SSO) 認証および認証とも呼ばれます ) をお客様に提供できます。 Adobe Pass Authentication を使用すると、MVPD でその認証が許可されている限り、購読者が最初の認証後に再度ログインする必要がなくなります。 （通常は 30 日です）。 これを実現するために、Adobe Pass認証は、お客様に認証トークン用の共通ドメインを提供します。 この認証状態情報は、特定の MVPD と統合されているすべての参加サイトで利用できます。


現在、MVPD とのAdobe Pass Authentication 統合のほとんどでは、主要な認証標準の 1 つである SAML プロトコルを使用しています。 Adobe Pass認証は、SAML アーキテクチャでプロキシサービスプロバイダーとして機能し、SAML 認証応答をAdobeの共通ドメインのセキュアトークンとして保持します。 Adobe Pass認証は SAML 2.0 に準拠しています。 ただし、Adobe Pass Authentication は、この時点で SAML SSO ソリューションで使用されるのが一般的ですが、Adobe Pass Authentication アーキテクチャは、特定のプロトコルに結び付けられていません。 したがって、OAuth 2.0 に基づくプロトコルやカスタムプロトコルなど、新しいプロトコルのサポートを時間の経過と共に追加できます。


Adobeは、MVPD の技術チームと連携し、既存の統合のニーズに合わせてAdobe Pass Authentication を設定します。 「標準」の統合と最小限のサポート要件（ドキュメントと基本的な E メールサポート）を想定し、MVPD の統合は無料でおこなえます。 MVPD が大幅なサポートやエスカレーションされたタイムラインを必要とする場合は、サポート料が請求される場合や、プロバイダーが Synacor などのソリューションに精通したサードパーティと協力する場合があります。


Adobe Pass認証では、次のように MVPD ビジネスロジックの効率的な処理もサポートしています。

Adobeは、自己完結型で、認証の要求を受け取ったときに MVPD によって適用できるビジネスロジックに対して、MVPD が認証要求を受け取ったときに、ビジネスロジックの強制を支援するために必要なデータを提供します。 このデータには、リクエストをおこなうユーザーの一意のデバイス ID と、デバイスの IP アドレスが含まれますが、これらに限定されません。

Adobeの操作や特定の処理を必要とするビジネスロジックの場合、Adobeは各 MVPD のカスタムプロパティを維持できます。 これらの MVPD 固有の設定/ポリシーには、最上位のワークフローの特定のポイントで開始できる事前定義済みのワークフローの有効化が含まれます。 カスタムプロパティのサポートの詳細については、Adobe担当者にお問い合わせください。

次の図は、MVPD とプログラマーとこれらのAdobe Pass Authentication コンポーネントとの関係を示しています。

![](assets/high-level-architecture-nflows.png)

*図：アーキテクチャとフローの概要*

## Adobe Pass Authentication Components {#components}

次に、Adobe Pass Authentication エコシステムの主なコンポーネントの一部の概要を示します。 これには次が含まれます。

* [Access Enabler/Clientless Web サービス](#ae)
* [Adobeがホストするバックエンドサーバ](#backend)
* [トークン](#tokens)

### Access Enabler/Clientless Web Services {#ae}

Access Enabler は、ユーザーとのすべての認証と承認のやり取りを容易にし、ユーザーのシステム上でローカルに実行します。 MVPD で実際のエンタイトルメントワークフローを処理するのは Access Enabler ですが、プログラマーは上位レベルの Web ページやプレーヤーアプリケーションに対する責任を維持します。

クライアントレスの Web サービスは、Web ページをレンダリングできないデバイス向けに、Adobe Pass Authentication によって提供されます。  これらのデバイスでは、Web 対応デバイス（PC、スマートフォン、タブレット）で MVPD による認証が実行される間に、エンタイトルメントプロセスが開始され、スマートデバイスでコンテンツが表示されます。

Access Enabler:

* MVPD 固有の認証および承認ワークフローを開始します。
* 不要なリクエストトラフィックを最小限に抑えるために、プログラマーのリソース/チャネルごとに成功した認証応答をキャッシュします。
* 明示的なデバイス登録など、各 MVPD に固有の事前定義済みのワークフローに対して設定できます。
* 次のフォームで使用できます。
   * SWF・ランタイムが実行できるFlash Player・ファイル
   * ブラウザーによって直接実行された JS ファイル
   * iOS、Android、Xbox など、様々なプラットフォーム向けのネイティブ Access Enabler。

### Adobeがホストするバックエンドサーバ {#backend}

Adobe Pass Authentication バックエンドサーバー (Adobeがホスト ):

* Adobe Pass Authentication とオペレーター間のサーバー間通信が必要な MVPD を使用して、認証および承認ワークフローをプロビジョニングします。
* プログラマーのサイトとアプリケーションの構成を維持します。
* ダウンロード可能な Access Enabler コンポーネント・ファイルをホストします。
* 認証および認証トークンを生成します。

### トークン {#tokens}

Adobe Pass Authentication Entitlement Solution は、認証/承認ワークフローの正常な完了時に取得される特定のデータの生成を中心としています。 これらのデータはトークンと呼ばれます。 寿命は限られており、プラットフォームに依存する場所に安全に保存されます。 有効期限が切れたら、認証ワークフローや認証ワークフローの再開を通じてトークンを再発行する必要があります。

認証/承認ワークフロー中に発行されるトークンは 3 種類あります。 2 つは「長期間有効」で、ユーザーの視聴エクスペリエンスの継続性を提供します。 3 つ目は、短時間のみ有効なトークンで、ストリームリッピングを通じた不正を軽減するための業界のベストプラクティスのサポートを提供します。 トークンの有効期間 (「TTL」) 値は、MVPD とプログラマー間の契約に基づいて設定されます。 TTL 値は、ビジネスと顧客に最適な値に設定します。

**長期間有効な認証トークン**. 認証は、お客様がAdobe Pass Authentication を使用して MVPD アカウントに正常にログオンすると成功します。 次に、Adobe Pass認証により、要求元のデバイスに結び付けられた長期間有効な認証 (「authN」) トークンが生成され、（MVPD に応じて）グローバルに一意の識別子 (「GUID」) でユーザーを匿名で識別します。

**長期間有効な認証トークン**. 認証が成功すると、Adobe Pass Authentication は長期間有効な認証 (「authZ」) トークンを作成します。 このトークンは、リクエストデバイスと特定の保護されたリソース（チャネル、シリーズ、エピソードなど）に関連付けられているので、移動できません。 Access Enabler は、長期間有効な authZ トークンを使用して、実際の表示アクセスに使用される短期間有効なメディアトークンを作成します。

**短時間のみ有効なメディアトークン**. ユーザーが認証されると、Adobe Pass Authentication は authZ トークンを生成し、そのトークンを使用して、Adobeによって署名され、交換中の改ざんを防ぐために暗号化された、単一使用の短時間有効なメディアトークンを生成します。 短時間有効なトークンは、Access Enabler API またはクライアントレス Web サービスを通じて埋め込みサイトに公開されるので、保護されたリソースへのアクセスを提供する前に、プログラマーのメディアサーバーは、Adobe Pass認証コンポーネントであるメディアトークン検証ツールを使用する必要があります。

## MVPD 統合ライフサイクル {#lifecycle}

次の図は、Adobe Pass Authentication と MVPD の統合のライフサイクルを示しています。

![](assets/mvpd-int-lifecycle.png)

*図：MVPD 統合ライフサイクル*

## 権利フローチャート {#chart}

次のフローチャートに、Adobe Pass Authentication を使用して使用権限を確認する全体的なプロセスを示します。

![](assets/authn-authz-entitlmnt-flow.png)

*図： Adobe Pass Authentication を使用して権限を確認するプロセス*

## 認証手順 {#authn-steps}

次の手順は、Adobe Pass認証フローの例を示しています。  これは、プログラマーがユーザーが MVPD の有効な顧客かどうかを判断するエンタイトルメントプロセスの一部です。  このシナリオでは、ユーザーは MVPD の有効な購読者です。  ユーザーは、次のプログラマーのアプリケーションを使用して、保護されたコンテンツを表示しようとしています。Flash:

1. ユーザーがプログラマーの Web ページを参照し、プログラマーのFlashアプリケーションとAdobe Pass Authentication Access Enabler コンポーネントをユーザーのマシンに読み込みます。 Flashアプリケーションは、Access Enabler を使用してAdobe Pass Authentication でプログラマーの識別子を設定し、Adobe Pass Authentication は、そのプログラマー (「requestor」) の構成と状態データを使用して Access Enabler を設定します。 他の API 呼び出しを実行する前に、Access Enabler がサーバからこのデータを受け取る必要があります。  技術メモ：プログラマは、Access Enabler の `setRequestor()` メソッド。詳しくは、 [プログラマー統合ガイド](/help/authentication/programmer-integration-guide-overview.md).
1. ユーザがプログラマの保護されたコンテンツを表示しようとすると、プログラマのアプリケーションは、ユーザに MVPD のリストを提示し、そこからプロバイダを選択する。
1. ユーザーがAdobe Pass認証サーバーにリダイレクトされ、ユーザーが選択した MVPD に対する暗号化された SAML リクエストが作成されます。 このリクエストは、プログラマーに代わって認証リクエストとして MVPD に送信されます。 MVPD のシステムに応じて、ユーザーのブラウザが MVPD のサイトにリダイレクトされてログインするか、プログラマーのアプリでログイン iFrame が作成されます。
1. どちらの場合も（redirect または iFrame）、MVPD はリクエストを受け入れ、そのログインページを表示します。
1. ユーザーが MVPD を使用してログインすると、MVPD はユーザーの有料顧客としてのステータスを検証し、MVPD は独自の HTTP セッションを作成します。
1. ユーザーが検証されると、MVPD は応答（SAML および暗号化）を作成し、MVPD はAdobe Pass Authentication に返します。
1. Adobe Pass認証は MVPD 応答を受け取り、Adobe Pass Authentication HTTP セッションが開いていることを確認し、 [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) MVPD からの応答を返し、プログラマーのサイトにリダイレクトします。
1. プログラマのサイトが再読み込みされ、Access Enabler が再読み込みされ、プログラマが setRequestor() を再度呼び出します。  現在の構成が変更されたので、setRequestor() の 2 回目の呼び出しが必要です。AuthN トークンがサーバ上で生成されるのを待っていることを Access Enabler に通知するフラグが存在します。
1. Access Enabler は、保留中の認証があることを確認し、Adobe Pass Authentication Server にトークンを要求します。 トークンは、サーバーの DRM 機能を呼び出すことでFlash Playerから取得されます。
1. AuthN トークンは、プログラマーのFlash PlayerLSO キャッシュに格納されます。認証が完了し、セッションはAdobe Pass認証サーバーで破棄されます。

## 認証手順 {#authz-steps}

以下の手順は、前の節 ([認証手順](#authn-steps)):

1. ユーザーがプログラマーの保護されたコンテンツにアクセスしようとすると、プログラマーのアプリケーションは、まず、ユーザーのローカルマシンまたはデバイス上の AuthN トークンを確認します。  そのトークンがない場合、 [認証手順](#authn-steps) 上に続く。  AuthN トークンが存在する場合、認証フローは、保護されたコンテンツの特定の項目に対するユーザーの表示権限を取得する要求を受けて、プログラマーのアプリケーションが Access Enabler への呼び出しを開始すると進みます。
1. 保護されたコンテンツの特定の項目は、「リソース識別子」で表されます。  これは単純な文字列か、より複雑な構造になるかもしれませんが、どの場合でも、リソース識別子の性質はプログラマーと MVPD の間で事前に合意されます。  プログラマのアプリケーションは、リソース識別子を Access Enabler に渡します。  Access Enabler は、ユーザーのローカルマシンまたはデバイス上で AuthZ トークンを確認します。  AuthZ トークンがない場合、Access Enabler はリクエストをバックエンドのAdobe Pass Authentication Server に渡します。
1. Adobe Pass認証サーバーは、標準化されたプロトコルを使用して MVPD 承認エンドポイントと通信します。  MVPD の応答が、ユーザーが保護されたコンテンツを表示する権限を持っていることを示している場合、Adobe Pass認証サーバーは AuthZ トークンを作成し、AuthZ トークンを Access Enabler に渡します。AuthZ トークンはユーザーのマシンに保存されます。
1. AuthZ トークンがユーザーのマシンまたはデバイスに保存されている場合、プログラマーのアプリケーションは Access Enabler を呼び出してAdobe Pass認証サーバからメディアトークンを取得し、そのトークンをプログラマーのアプリケーションに提供します。
1. 最後に、プログラマーのアプリケーションは、メディアトークン検証コンポーネントを使用して、適切なユーザーが適切なコンテンツを表示していることを確認し、メディアトークンを設定した状態で、保護されたコンテンツを表示できます。

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
