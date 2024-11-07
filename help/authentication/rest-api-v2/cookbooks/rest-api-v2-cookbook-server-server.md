---
title: REST API V2 クックブック（サーバー間）
description: REST API V2 クックブック（サーバー間）
source-git-commit: c17e52dd52fa14c50d59945598d1913f02be2468
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# REST API V2 クックブック（サーバー間） {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。


## 概要 {#overview}

このクックブックドキュメントの目的は、サーバー間アーキテクチャで REST API V2 を使用してAdobe Pass認証を実装するためのベストプラクティスを詳しく説明することです。  基本的な要件、段階的なフローの実装および実稼動環境と運用に関する一般的な考慮事項を提供します。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) のドキュメントで制限されています。


## Components {#components}

動作しているサーバー間ソリューションには、次のコンポーネントが含まれます。


| タイプ | コンポーネント | 説明 |
| --- | --- | --- |
| ストリーミングデバイス | ストリーミングアプリ | ユーザーのストリーミングデバイス上に存在し、認証済みビデオを再生するプログラマーアプリケーション。 |
| | \[ オプション\] AuthN モジュール | ストリーミングデバイスにユーザーエージェント（Web ブラウザー）がある場合、AuthN モジュールは MVPD IdP 上でユーザーを認証する必要があります。 |
| \[ オプション\] AuthN デバイス | AuthN アプリ | ストリーミングデバイスにユーザーエージェント（Web ブラウザー）がない場合、AuthN アプリケーションは、Web ブラウザーを使用して別のユーザーのデバイスからアクセスする、プログラマー向けの Web アプリケーションです。 |
| プログラマ基盤 | プログラマーサービス | ストリーミングデバイスをAdobe Pass サービスにリンクして、認証と承認の決定を取得するサービス。 |
| Adobe基盤 | Adobe Pass サービス | MVPD IdP および AuthZ サービスと統合され、認証と承認の決定を行うサービス。 |
| MVPD インフラストラクチャ | MVPD IdP | ユーザーの ID を検証するために、資格情報ベースの認証サービスを提供する MVPD エンドポイント。 |
| | MVPD AuthZ サービス | ユーザーの購読、保護者による制限などに基づいて認証の決定を行う MVPD エンドポイント。 |


フローで使用されるその他の用語は、
[ 用語集 ](/help/authentication/glossary.md)。

次の図に、フロー全体を示します。

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### サーバー間アーキテクチャで REST API V2 を実装する手順 {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Adobe Pass REST API V2 を実装するには、以下の手順（フェーズにグループ化）に従う必要があります。

## 0.前提条件 {#prerequisites}

サーバー間の実装では、ストリーミングアプリとプログラマーサービスは、プログラマーサービスが以下を実行できるように、プロトコルを確立する必要があります。
* デバイス上のストリーミングアプリを一意に識別します
* ストリーミングアプリの代わりに動作し、Adobe Pass サービスと通信します
* ストリーミングアプリとデバイスに関する情報（IP アドレス、ソースポート、デバイス情報など）を収集してAdobe Passに渡します
* 決定と手順をストリーミングアプリに返す

デバイス ID、クライアント ID、クライアントシークレット（以下で定義）などのパラメーターは、ストリーミングアプリまたはプログラマーサービスのいずれかに保存できます。

## A.登録フェーズ {#registration-phase}

### 手順 1：アプリケーションを登録 {#step-1-register-your-application}

アプリケーションがAdobe Pass REST API V2 を呼び出すには、API セキュリティレイヤーに必要なアクセストークンが必要です。 サーバー間実装の場合、アプリケーションインスタンスの代わりにプログラマーサービスを登録できます。
クライアント ID とクライアント秘密鍵の値は、ストリーミングデバイスごとに取得する必要があります。

アクセストークンを取得するには、プログラマーサービスがストリーミングアプリの代わりに動作することができ、次の手順に従う必要があります。[ 動的クライアント登録 ](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B.認証フェーズ {#authentication-phase}

### 手順 2：既存の認証済みプロファイルを確認する {#step-2-check-for-existing-authenticated-profiles}

プログラマーサービスは、ストリーミングアプリに代わって、既存の認証済みプロファイルを確認します。`/api/v2/{serviceProvider}/profiles` （[ 認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）

* プロファイルが見つからず、ストリーミングアプリケーションが TempPass フローを実装している場合
   * [ 一時的アクセスフロー ](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) の実装方法に関するドキュメントに従います
* プロファイルが見つからない場合、ストリーミングアプリケーションは認証フローを実装します
   * <b> 手順 2.a:</b> プログラマーサービスが、serviceProvider: <b>/api/v2/{serviceProvider}/configuration で使用可能な MVPD のリストを取得します </b><br>
（[ 使用可能な MVPD のリストの取得 ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * プログラマーサービスは、MVPD のリストに対してフィルタリングを実装し、他の MVPD （TempPass、テスト MVPD、開発中の MVPD など）を非表示にすることを意図した MVPD のみを表示することができます。
   * プログラマーサービスは、ストリーミングアプリがピッカーを表示するために、フィルターされた MVPD リストを返す必要があります。ユーザーは MVPD を選択します
   * ストリーミングアプリから選択した MVPD を使用して、プログラマーサービスは <b>/api/v2/{serviceProvider}/sessions セッションを作成 </b><br> ます。
（[ 認証セッションの作成 ](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)） <br>
      * 認証に使用するコードと URL が返されます
      * プロファイルが見つかった場合、プログラマーサービスは <a href="#preauthorization-phase">C に進みます。事前認証フェーズ </a>
   * プログラマーサービスは、コードと URL をストリーミングアプリに返します。

### 手順 3：ユーザーの認証 {#step-3-authenticate-the-user}

ブラウザーまたは 2 番目の画面の Web ベースのアプリケーションを使用する場合：

* オプション 1。 ストリーミングアプリは、ブラウザーまたは web ビューを開き、認証する URL を読み込み、資格情報を送信する必要がある MVPD ログインページに移動できます
   * ユーザーがログイン/パスワードを入力します。最終リダイレクトで成功ページを表示します
* オプション 2. ストリーミングアプリでは、ブラウザーを開いてコードを表示することはできません。 <b>AuthN_APP という個別の Web アプリケーションを開発する必要があります </b>。ユーザーに対し、CODE の入力、URL のビルドとオープンを行うよう依頼します。<b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * ユーザーがログイン/パスワードを入力します。最終リダイレクトで成功ページを表示します

### 手順 4：認証済みプロファイルを確認 {#step-4-check-for-authenticated-profiles}

プログラマーサービスは、ブラウザーまたは 2 番目の画面で完了するように、MVPD との認証を確認します

* <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> では、15 秒ごとにポーリングすることをお勧めします
（[ 特定の MVPD の認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * MVPD ピッカーが 2 番目の画面アプリケーションに表示されるので、ストリーミングアプリケーションで MVPD が選択されていない場合、ポーリングは CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> で発生します。
（[ 特定のコードの認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* ポーリングは 30 分を超えないようにします。30 分に達してもストリーミングアプリケーションがアクティブな場合は、新しいセッションを開始し、新しいコードと URL を返します
* 認証が完了すると、返される値は認証済みプロファイルで 200 になります
* プログラマーサービスは <a href="#preauthorization-phase">C に進むことができます。事前認証フェーズ </a>

## C.事前認証フェーズ {#preauthorization-phase}

### 手順 5：事前承認済みリソースの確認 {#step-5-check-for-preauthorized-resources}

ユーザーの有効な認証プロファイルがある場合、プログラマーサービスは次の項目を確認できます
使用可能なビデオにアクセスし、表示するストリーミングアプリケーションにリストを渡します。

* 手順はオプションで、認証済みユーザーパッケージでは使用できないリソースをアプリケーションがフィルタリングする場合に実行されます
* <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br> の呼び出し
（[ 特定の MVPD を使用した事前認証決定の取得 ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.承認フェーズ {#authorization-phase}

### 手順 6：許可されたリソースの確認 {#step-6-check-for-authorized-resources}

ストリーミングアプリは、ユーザーが選択したビデオ/アセット/リソースを再生する準備をします。

* すべてのプレイ開始にステップが必要です
* ストリーミングアプリは、この情報をプログラマーサービスに渡します
* ストリーミングアプリの基盤にあるプログラマーサービスは、<b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br> を呼び出します。
（[ 特定の MVPD を使用した認証決定の取得 ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * decision = &#39;Permit&#39;、プログラマーサービスは、ストリーミングを開始するようにストリーミングアプリに指示します
   * decision = &#39;Deny&#39;、プログラマーサービスは、そのビデオにアクセスできないことをユーザーに通知するようにストリーミングアプリに指示します
   * その過程で、プログラマーサービスは他のビジネスルールを評価し、ストリーミングアプリに適切な決定を返す場合があります

## E. ログアウトフェーズ {#logout-phase}

### 手順 7：ログアウト {#step-7-logout}

ストリーミングアプリ：ユーザーが MVPD からのログアウトを希望している

* ストリーミングアプリは、この特定のアプリに対して MVPD からログアウトする必要があることをプログラマーサービスに通知します。
* プログラマーサービスは、認証済みユーザーに関して保存している情報をクリーンアップできます
* プログラマーサービス呼び出し <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[ 特定の MVPD のログアウトの開始 ](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 応答 actionType=&#39;interactive&#39;および url が存在する場合、プログラマーサービスは URL をストリーミングアプリに返します。
* ストリーミングアプリは、既存の機能に基づいて、ブラウザーで URL を開くことができます（通常、認証に使用するのと同じ）
* ストリーミングアプリにブラウザーがない場合、または認証時のインスタンスと異なるインスタンスの場合は、MVPD セッションがブラウザーキャッシュに保持されなかったので、フローを停止できます。

## 環境と機能要件{#environments}

プログラマーは、実稼動用とステージング用の 2 つ以上の環境を少なくとも作成する必要があります。


### 実稼動

実稼働環境は可用性が高く、大きなスパイクや予期しないスパイク（ライブスポーツ、破損など）に対して適切にスケーリングする必要があります
ニュース）。



Adobe Pass サービスは、米国全体に地理的に分散した複数のデータセンターで実行されます。  Adobe Pass サービスからの応答時間を最高にするには（つまり、待ち時間を最小にするには）、プログラマーも同様に地理的に分散したサービスを作成する必要があります
インフラストラクチャ


プログラマーサービスは、Adobeがトラフィックを再ルーティングする必要がある場合、DNS キャッシュを最大 30 秒に制限する必要があります。 これは、データセンターが利用できなくなったときに発生する可能性があります。


プログラマーは、実稼動環境の公開 IP 範囲を指定する必要があります。 これらは、アクセス用にAdobe Pass インフラストラクチャの IP の許可リストに登録され、Adobeの公正な API 使用ポリシーによって管理されます。

### ステージング

ステージング環境は最小限にすることができますが、すべてのシステムコンポーネントとビジネスロジックを含める必要があります。 実稼動環境と同様に機能し、実稼動環境以外のリリースをテストできます。 ステージング環境をAdobe Pass テスト環境に接続して、プログラマーが使用したり、必要に応じてAdobeで使用したりできれば、テストとトラブルシューティングに役立ちます。

### 機能要件

プログラマーサービスは、フローを実行しているデバイスの正確なデバイス識別情報を渡す必要があります。 さらに、プログラマーサービスは、フローを実行するデバイスの IP を（x-forwarded-for ヘッダーで）接続ソースポートと共に（device info フィールドで）渡す必要があります。

プログラマーサービスは、個々の MVPD または統合アプリ（デバイス IP、ソースポート、デバイス情報、MRSS、ECID などのオプションデータ）で必要なデータと形式を送信する必要があります。<!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->。

プログラマーサービスは、キャッシュ時には認証プロファイルと決定の有効性を尊重し、通知時には認証または決定を無効にする必要があります。

プログラマーサービスは、（暗号化されたユーザーメタデータ用に） Adobeと共有される証明書を保持する必要があります。

## 関連情報 {#related}

* [REST API V2 リファレンス](/help/authentication/rest-api-v2/rest-api-v2-overview.md)
