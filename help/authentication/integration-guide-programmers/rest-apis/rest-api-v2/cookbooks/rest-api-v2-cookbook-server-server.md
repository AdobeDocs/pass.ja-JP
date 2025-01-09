---
title: REST API V2 クックブック（サーバー間）
description: REST API V2 クックブック（サーバー間）
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# REST API V2 クックブック（サーバー間） {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) のドキュメントで制限されています。

このクックブックドキュメントの目的は、サーバー間アーキテクチャで REST API V2 を使用してAdobe Pass認証を実装するためのベストプラクティスを詳しく説明することです。 基本的な要件、段階的なフローの実装および実稼動環境と運用に関する一般的な考慮事項を提供します。

## Components {#components}

動作しているサーバー間ソリューションでは、次のコンポーネントが関係します。

| タイプ | コンポーネント | 説明 |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ストリーミングデバイス | ストリーミングアプリ | ユーザーのストリーミングデバイス上に存在し、認証済みビデオを再生するプログラマーアプリケーション。 |
|                           | \[ オプション\] AuthN モジュール | Streaming Device にユーザーエージェント（Web ブラウザー）がある場合、MVPD IdP 上でユーザーの認証は AuthN モジュールが行います。 |
| \[ オプション\] AuthN デバイス | AuthN アプリ | ストリーミングデバイスにユーザーエージェント（Web ブラウザー）がない場合、AuthN アプリケーションは、Web ブラウザーを使用して別のユーザーのデバイスからアクセスする、プログラマー向けの Web アプリケーションです。 |
| プログラマ基盤 | プログラマーサービス | ストリーミングデバイスをAdobe Pass サービスにリンクして、認証と承認の決定を取得するサービス。 |
| Adobe基盤 | Adobe Pass サービス | MVPD IdP および AuthZ サービスと統合され、認証と承認の決定を行うサービス。 |
| MVPD インフラストラクチャ | MVPD IdP | ユーザーの ID を検証するために、資格情報ベースの認証サービスを提供するMVPD エンドポイント。 |
|                           | MVPD AuthZ サービス | ユーザーの購読、保護者による制限などに基づいて認証の決定を行うMVPD エンドポイント。 |

フローで使用されるその他の用語は、[ 用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md) で定義されています。

次の図に、フロー全体を示します。

![REST API V2 クックブック（サーバー間） ](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### サーバー間アーキテクチャで REST API V2 を実装する手順 {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Adobe Pass REST API V2 を実装するには、以下の手順（フェーズにグループ化）に従う必要があります。

## 0.前提条件 {#prerequisites}

サーバー間実装では、ストリーミングアプリとプログラマーサービスは、プログラマーサービスが以下を実行できるようにプロトコルを確立する必要があります。

* デバイス上のストリーミングアプリを一意に識別します。
* ストリーミングアプリの代わりに機能し、Adobe Pass サービスと通信します。
* ストリーミングアプリとデバイスに関する情報（IP アドレス、ソースポート、デバイス情報など）を収集して保存し、Adobe Passに渡します。
* 決定と手順をストリーミングアプリに返します。

デバイス ID、クライアント ID、クライアントシークレット（以下で定義）などのパラメーターは、ストリーミングアプリまたはプログラマーサービスのいずれかに保存できます。

## A.登録フェーズ {#registration-phase}

### 手順 1：アプリケーションを登録 {#step-1-register-your-application}

アプリケーションがAdobe Pass REST API V2 を呼び出すには、API セキュリティレイヤーに必要なアクセストークンが必要です。

サーバー間実装の場合、プログラマーサービスはアプリケーションインスタンスの代わりに登録できますが、クライアント資格情報（クライアント ID およびクライアントの秘密鍵）の値はストリーミングデバイスごとに取得する必要があります。

アクセストークンを取得するには、プログラマーサービスがストリーミングアプリの代わりに動作することができ、[ 動的クライアント登録 ](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) ドキュメントに記載されている手順に従う必要があります。

## B.認証フェーズ {#authentication-phase}

### 手順 2：既存の認証済みプロファイルを確認する {#step-2-check-for-existing-authenticated-profiles}

プログラマーサービスは、ストリーミングアプリに代わって、既存の認証済みプロファイルを確認します。`/api/v2/{serviceProvider}/profiles` （[ 認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）

* プロファイルが見つからず、ストリーミングアプリケーションが TempPass フローを実装している場合
   * [ 一時的アクセスフロー ](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) の実装方法に関するドキュメントに従います
* プロファイルが見つからない場合、ストリーミングアプリケーションは認証フローを実装します
   * <b> 手順 2.a:</b> プログラマーサービスが、serviceProvider: <b>/api/v2/{serviceProvider}/configuration で使用可能な MVPD のリストを取得します </b><br>
（[ 使用可能な MVPD のリストの取得 ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * プログラマーサービスは、MVPD のリストに対してフィルタリングを実装し、他の MVPD （TempPass、テスト MVPD、開発中の MVPD など）を非表示にすることを目的とした MVPD のみを表示することができます。
   * プログラマーサービスは、ストリーミングアプリでピッカーを表示するために、フィルタリングされたMVPDリストを返す必要があります。ユーザーはMVPDを選択します
   * ストリーミングアプリからMVPDを選択した状態で、プログラマーサービスが <b>/api/v2/{serviceProvider}/sessions セッションを作成する </b><br>
（[ 認証セッションの作成 ](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)） <br>
      * 認証に使用するコードと URL が返されます
      * プロファイルが見つかった場合、プログラマーサービスは <a href="#preauthorization-phase">C に進みます。事前認証フェーズ </a>
   * プログラマーサービスは、コードと URL をストリーミングアプリに返します。

### 手順 3：ユーザーの認証 {#step-3-authenticate-the-user}

ブラウザーまたは 2 番目の画面の Web ベースのアプリケーションを使用する場合：

* オプション 1。 ストリーミングアプリでは、ブラウザーまたは web ビューを開き、認証する URL を読み込むことで、資格情報を送信する必要のあるMVPDのログインページにアクセスできます
   * ユーザーがログイン/パスワードを入力すると、最終リダイレクトで成功ページが表示される
* オプション 2. ストリーミングアプリでは、ブラウザーを開いてコードを表示することはできません。 <b>AuthN_APP という個別の Web アプリケーションを開発する必要があります </b>。ユーザーに CODE を入力し、URL をビルドして開くよう依頼します（<b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>）。
   * ユーザーがログイン/パスワードを入力すると、最終リダイレクトで成功ページが表示される

### 手順 4：認証済みプロファイルを確認 {#step-4-check-for-authenticated-profiles}

プログラマーサービスは、MVPDとの認証を確認して、ブラウザーまたは 2 番目の画面で完了します

* <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> では、15 秒ごとにポーリングすることをお勧めします
（[ 特定のMVPDの認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * MVPD ピッカーが 2 番目の画面アプリケーションに表示されるので、ストリーミングアプリケーションでMVPDが選択されていない場合、ポーリングは CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> で行われます
（[ 特定のコードの認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* ポーリングは、30 分に達したときにストリーミングアプリケーションがまだアクティブである場合は、30 分を超えてはなりません。新しいセッションを開始する必要があり、新しいコードと URL が返されます
* 認証が完了すると、返される値は認証済みプロファイルで 200 になります
* プログラマーサービスは <a href="#preauthorization-phase">C に進むことができます。事前認証フェーズ </a>

## C.事前認証フェーズ {#preauthorization-phase}

### 手順 5：事前承認済みリソースの確認 {#step-5-check-for-preauthorized-resources}

ユーザーの有効な認証プロファイルを使用すると、プログラマーサービスは、使用可能なビデオへのアクセスを確認し、表示するストリーミングアプリケーションにリストを渡すことができます。

* 認証済みユーザーパッケージに含まれていないリソースをアプリケーションが除外する場合は、この手順をオプションで実行します
* <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br> の呼び出し
（[ 特定のMVPDを使用した事前認証決定の取得 ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.承認フェーズ {#authorization-phase}

### 手順 6：許可されたリソースの確認 {#step-6-check-for-authorized-resources}

ストリーミングアプリは、ユーザーが選択したビデオ/アセット/リソースを再生する準備をします。

* すべてのプレイ開始にステップが必要です
* ストリーミングアプリは、この情報をプログラマーサービスに渡します
* ストリーミングアプリに代わるプログラマーサービス。<b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br> を呼び出します。
（[ 特定のMVPDを使用した認証決定の取得 ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * decision = &#39;Permit&#39;、プログラマーサービスはストリーミング アプリにストリーミングを開始するように指示します
   * decision = &#39;拒否&#39;、プログラマーサービスは、そのビデオにアクセスできないことをユーザーに通知するようにストリーミングアプリに指示します
   * その過程で、プログラマーサービスは他のビジネスルールを評価し、ストリーミングアプリに適切な決定を返す場合があります

## E. ログアウトフェーズ {#logout-phase}

### 手順 7：ログアウト {#step-7-logout}

ストリーミングアプリ：ユーザーはMVPDからのログアウトを希望しています

* ストリーミングアプリは、この特定のアプリについてMVPDからログアウトする必要があることをプログラマーサービスに通知します。
* プログラマーサービスは、認証済みユーザーに関して保存している情報をクリーンアップできます
* プログラマーサービス呼び出し <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[ 特定のMVPDのログアウトの開始 ](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 応答 actionType=&#39;interactive&#39;および url が存在する場合、プログラマーサービスは URL をストリーミングアプリに返します。
* ストリーミングアプリは、既存の機能に基づいて、ブラウザーで URL を開くことができます（通常、認証に使用するのと同じ）
* ストリーミングアプリにブラウザーがない場合、または認証時のインスタンスと異なるインスタンスの場合は、MVPD セッションがブラウザーキャッシュに保持されなかったので、フローを停止できます。

## 環境と機能要件{#environments}

プログラマーは、実稼動用とステージング用の 2 つ以上の環境を少なくとも作成する必要があります。

### 実稼動

実稼動環境は可用性が高く、大きなスパイクや予期しないスパイク（ライブスポーツ、ニュース速報など）に対して適切に拡張する必要があります。

Adobe Passサービスは、米国全体に地理的に分散した複数のデータセンターで実行されます。 Adobe Pass サービスからの応答時間を最高にするには（つまり、待ち時間を最小にする）、プログラマーは同様に地理的に分散したサービスインフラストラクチャも作成する必要があります。

プログラマーサービスは、Adobeがトラフィックを再ルーティングする必要がある場合、DNS キャッシュを最大 30 秒に制限する必要があります。 これは、データセンターが利用できなくなった場合に発生する可能性があります。

プログラマーは、実稼動環境の公開 IP 範囲を指定する必要があります。 これらは、アクセス用にAdobe Pass インフラストラクチャの IP の許可リストに登録され、Adobeの公正な API 使用ポリシーによって管理されます。

### ステージング

ステージング環境は最小限にすることができますが、すべてのシステムコンポーネントとビジネスロジックを含める必要があります。 実稼動環境と同様に機能し、実稼動環境以外のリリースをテストできます。 理想的には、ステージング環境をAdobe Pass テスト環境に接続して、プログラマーが使用したり、必要に応じてAdobeで使用したりできるので、テストとトラブルシューティングに役立ちます。

### 機能要件

プログラマーサービスは、フローを実行しているデバイスの正確なデバイス識別情報を渡す必要があります。 さらに、プログラマーサービスは、フローを実行するデバイスの IP を（x-forwarded-for ヘッダーで）接続ソースポートと共に（device info フィールドで）渡す必要があります。

プログラマーサービスは、個々の MVPD または統合アプリ（例：デバイス IP、ソースポート、デバイス情報、MRSS、ECID などのオプションデータ）で必要なデータと形式を送信する必要があります。<!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->。

プログラマーサービスは、キャッシュ時には認証プロファイルと決定の有効性を尊重し、通知時には認証または決定を無効にする必要があります。

プログラマーサービスは、（暗号化されたユーザーメタデータ用に） Adobeと共有される証明書を保持する必要があります。

## 関連情報 {#related}

* [REST API V2 リファレンス](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
