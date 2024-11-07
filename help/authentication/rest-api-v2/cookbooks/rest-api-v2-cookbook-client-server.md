---
title: REST API V2 クックブック（クライアントからサーバー）
description: REST API V2 クックブック（クライアントからサーバー）
source-git-commit: e1e1835d0d523377c48b39170919f7120cc3ef90
workflow-type: tm+mt
source-wordcount: '695'
ht-degree: 0%

---


# REST API V2 クックブック（クライアントからサーバー） {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> REST API V2 の実装については、[ スロットルメカニズム ](/help/authentication/throttling-mechanism.md) のドキュメントで制限されています。

## クライアントサイドアプリケーションに REST API V2 を実装する手順 {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Adobe Pass REST API V2 を実装するには、次の手順に従い、いくつかの段階にグループ化する必要があります。

## A.登録フェーズ {#registration-phase}

### 手順 1：アプリケーションを登録 {#step-1-register-your-application}

アプリケーションがAdobe Pass REST API V2 を呼び出すには、API セキュリティレイヤーに必要なアクセストークンが必要です。

アクセストークンを取得するには、アプリケーションが次の手順に従う必要があります。[ 動的なクライアント登録 ](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B.認証フェーズ {#authentication-phase}

### 手順 2：既存の認証済みプロファイルを確認する {#step-2-check-for-existing-authenticated-profiles}

ストリーミングアプリケーションは、既存の認証済みプロファイルをチェック：<b>/api/v2/{serviceProvider}/profiles</b><br>
（[ 認証プロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）

* プロファイルが見つからず、ストリーミングアプリケーションが TempPass フローを実装している場合
   * [ 一時的アクセスフロー ](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) の実装方法に関するドキュメントに従います
* プロファイルが見つからず、ストリーミングアプリケーションが認証フローを実装している場合
   * ストリーミングアプリケーションは、serviceProvider:<b>/api/v2/{serviceProvider}/configuration で使用可能な MVPD のリストを取得 </b><br> ます。
（[ 使用可能な MVPD のリストの取得 ](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * ストリーミングアプリケーションは、MVPD のリストにフィルタリングを実装し、他の MVPD （TempPass、テスト MVPD、開発中の MVPD など）を非表示にすることを目的とした MVPD のみを表示できます。
   * ストリーミングアプリケーションがピッカーを表示し、ユーザーが MVPD を選択する
   * ストリーミングアプリケーションがセッション <b>/api/v2/{serviceProvider}/sessions を作成する </b><br>
（[ 認証セッションの作成 ](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)） <br>
      * 認証に使用するコードと URL が返されます
      * プロファイルが見つかった場合、ストリーミングアプリケーションは <a href="#preauthorization-phase">C に進む場合があります。事前承認フェーズ </a> または <a href="#authorization-phase">D。承認フェーズ </a>

### 手順 3：ユーザーの認証 {#step-3-authenticate-the-user}

ブラウザーまたは 2 番目の画面の Web ベースのアプリケーションを使用する場合：

* オプション 1。 ストリーミングアプリケーションは、ブラウザーまたは web ビューを開き、認証する URL を読み込むことができ、ユーザーは資格情報を送信する必要がある MVPD ログインページに移動します
   * ユーザーがログイン/パスワードを入力すると、最終リダイレクトで成功ページが表示される
* オプション 2. ストリーミングアプリケーションでは、ブラウザーを開いてコードを表示することはできません。 <b> 別の web アプリケーションを開発する必要があります </b>。ユーザーに CODE を入力し、URL をビルドして開くよう依頼します：<b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * ユーザーがログイン/パスワードを入力します。最終リダイレクトで成功ページを表示します

### 手順 4：認証済みプロファイルを確認 {#step-4-check-for-authenticated-profiles}

ストリーミングアプリケーションは、MVPD による認証チェックを実行して、ブラウザーまたは 2 番目の画面で完了します

* <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br> では、15 秒ごとにポーリングすることをお勧めします
（[ 特定の MVPD の認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * MVPD ピッカーが 2 番目の画面アプリケーションに表示されるので、ストリーミングアプリケーションで MVPD が選択されていない場合、ポーリングは CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br> で発生します。
（[ 特定のコードの認証済みプロファイルの取得 ](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* ポーリングは 30 分を超えないようにします。30 分に達してもストリーミングアプリケーションがアクティブな場合は、新しいセッションを開始し、新しいコードと URL を返します
* 認証が完了すると、返される値は認証済みプロファイルで 200 になります
* ストリーミングアプリケーションは <a href="#preauthorization-phase">C に進む場合があります。事前承認フェーズ </a> または <a href="#authorization-phase">D。承認フェーズ </a>

## C.事前認証フェーズ {#preauthorization-phase}

### 手順 5：事前承認済みリソースの確認 {#step-5-check-for-preauthorized-resources}

ストリーミングアプリケーションは、認証済みユーザーが使用できるビデオを表示する準備を行い、のチェックボックスをオンにする可能性があります。
アクセス （これらのリソースへの）。

* 手順はオプションで、認証済みユーザーパッケージでは使用できないリソースをアプリケーションがフィルタリングする場合に実行されます
* <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br> の呼び出し
（[ 特定の MVPD を使用した事前認証決定の取得 ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.承認フェーズ {#authorization-phase}

### 手順 6：許可されたリソースの確認 {#step-6-check-for-authorized-resources}

ストリーミングアプリケーションは、ユーザーが選択したビデオ/アセット/リソースを再生する準備をします。

* すべてのプレイ開始にステップが必要です
* <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br> を呼び出します。
（[ 特定の MVPD を使用した認証決定の取得 ](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * 決定= 「許可」、ストリーミングデバイスがストリーミングを開始します
   * decision = &#39;Deny&#39;、ストリーミングデバイスは、そのビデオへのアクセス権がないことをユーザーに通知します

## E. ログアウトフェーズ {#logout-phase}

### 手順 7：ログアウト {#step-7-logout}

ストリーミングデバイス：ユーザーが MVPD からのログアウトを希望している

* <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br> を呼び出します
（[ 特定の MVPD のログアウトの開始 ](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 応答 actionType=&#39;interactive&#39;および url が存在する場合は、ブラウザー/2 番目の画面で url を開いて、MVPD でのログアウトを完了します。
