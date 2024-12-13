---
title: 使用権限サービスの監視の概要
description: 使用権限サービスの監視の概要
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# 使用権限サービスの監視の概要 {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#introduction}

TVE のサイトとアプリは 24 時間年中無休で利用できる必要があるため、お客様は問題をできるだけ迅速に検出して修正するために、エンタイトルメントイベントに関するリアルタイムのインサイトを必要とします。 また、トラフィックの量を提供しているプラットフォームと、実装が不良でコンバージョン率が低い可能性のあるプラットフォームを判断するために、毎月のデータを分析する必要があります。

権利付与サービス監視（ESM）は、プログラマーと MVPD に、認証イベントと承認イベントをリアルタイムに可視化するデータフィードを提供します。 データはAdobe Pass認証システムから収集され、RESTful API を介して提供されます。  ユーザーは、直接にデータを使用することも、独自にカスタムビルドされた運用ダッシュボード内からデータを使用することもできます。

ESM システムの中核となる要素は、そのメトリックとディメンションです。 ESM は、ディメンション選択に従って集計指標を含むレポートを生成します。 Adobe Pass イベントは PST タイムゾーンでログに記録されるので、ESM レポートは PST タイムゾーンでも使用できます。

ESM API は一般には使用できません。  可用性に関する質問については、Adobe担当者にお問い合わせください。

## プログラマー向け ESM {#esm-for-programmers}

### プログラマーは、次の指標を監視できます。 {#programmers-monitor-metrics}


| *指標名* | *説明* |
|-------------------------|--------------------------|
| authn-attempts | 開始された認証フローの数 |
| authn-successful | クライアントが正常に取得した認証トークンの数 |
| authn-pending | 正常に生成された認証トークンの数（クライアントが実際に取得したかどうかに関係ありません） |
| authn-failed | 外部システムを介して実行された失敗した認証の数。 |
| クライアントレスのトークン | 正常に発行されたクライアントレストークンの数 |
| クライアントレス失敗 | クライアントレス API からトークンを受信しようとして失敗した回数 |
| authz-attempts | 試行された承認数 |
| authz-successful | 成功した承認数 |
| authz-failed | アプリケーションレベルで MVPD によって拒否された認証の数 |
| authz-rejected | Adobe サービス プロバイダーによって悪意があると見なされ、DoS 攻撃防止の結果として拒否された認証試行の数 |
| authz-latency | MVPD エンドポイントで費やした合計ミリ秒数 |
| メディアトークン | 生成されたショートメディアトークンの数（再生リクエストの数に同化されます） |
| ユニークアカウント | 選択した時間間隔内に使用権限（AuthN/AuthZ）アクションを実行した一意のユーザーの数。 （この指標は、毎日の値がリクエストされた場合にのみ表示されます。） </br> これは、個々のデータセンターに対して計算されます。 「dc」ディメンションがリクエストされない場合、この指標は表示されません。 |
| ユニークセッション | 選択した期間内にAdobe Pass Authentication サービスに対して認証フロー呼び出しを実行した一意のセッションの数。 （この指標は、毎日の値がリクエストされた場合にのみ表示されます。） </br> これは、個々のデータセンターに対して計算されます。 「dc」ディメンションがリクエストされない場合、この指標は表示されません。 |
| count | イベント指向のレポートで使用されるシンプルなカウンター |

</br>

### プログラマーは、上記の指標を次のディメンションでフィルタリングできます。 {#progr-filter-metrics}


| *Dimension名* | *説明* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 年 | 4 桁の年 |
| 月 | 年間通算月（1 ～ 12） |
| 日 | 月の特定の日（1 ～ 31） |
| 時間 | 時刻 |
| 分 | 時刻の分 |
| media-company | ユーザーの使用権限プロセスを開始した web サイトを所有するメディア会社 |
| dc | （データセンター）リクエストを受信したホーム地域。 |
| 委任状 | プロキシMVPD（直接統合の場合は「ダイレクト」になります） |
| mvpd | ユーザーに使用権限を付与するMVPD |
| requestor-id | 権利付与リクエストの実行に使用する要求者 ID |
| チャネル | リソースフィールドから抽出されたチャネル web サイト（指定されている場合は MRSS ペイロードからチャネル/タイトルとして抽出され、RSS 形式でない場合はリソース値にマッピングされます）。 |
| resource-id | 承認リクエストに含まれる実際のリソースタイトル（指定された場合は、MRSS ペイロードから項目/タイトルとして抽出されます） |
| デバイス | デバイスプラットフォーム（PC、モバイル、コンソールなど） |
| eap | 認証フローが外部システムを介して実行される場合の外部認証プロバイダー。 </br> 値は次のとおりです。</br> – 該当なし – Adobe Passによって認証が提供された </br> Apple – 認証を提供した外部システムがApple |
| os ファミリ | デバイス上で動作しているオペレーティングシステム |
| browser-family | Adobe Pass認証へのアクセスに使用するユーザーエージェント |
| cdt | 現在クライアントレスに使用されているデバイスプラットフォーム（代替）。 </br> 値は次のとおりです。</br> – なし – イベントがクライアントレス SDKから発生しなかった </br> – 不明 – クライアントレス API からの deviceType パラメーターはオプションなので、値を含まない呼び出しがあります。 </br> - Clientless API を通じて送信されたその他の値（xbox、appletv、roku など） </br> |
| platform-version | クライアントレス SDKのバージョン |
| os タイプ | デバイス上で実行されている代替オペレーティング システム （現在使用されていません） |
| browser-version | ユーザーエージェントのバージョン |
| nsdk | 使用されたクライアント SDK （android、fireTV、js、iOS、tvOS、non-sdk） |
| nsdk-version | Adobe Pass認証クライアント SDKのバージョン |
| イベント | Adobe Pass認証イベント名 |
| 理由 | Adobe Pass Authentication でレポートされるエラーの理由 |
| sso-type | 基盤となる SSO メカニズムの platform/passive/adobe。 別のアプリケーションで AuthN を再利用することにより、認証トークンが発行されたことを示します |
| platform | デバイスが識別したプラットフォーム。 使用可能な値：</br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> – など |
| application-name | 使用するように設定された DCR 登録アプリケーションに対して TVE Dashboard で設定されたアプリケーション名。 |
| application-version | 使用するように設定された DCR 登録済みアプリケーションに対して、TVE ダッシュボードで設定されたアプリケーションのバージョン。 |
| customer-app | [ デバイス情報 ](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) を介して渡されるカスタムアプリケーション ID。 |
| content-category | アプリケーションからリクエストされたコンテンツのカテゴリ。 |

## MVPD 用 ESM {#esm-for-mvpds}

### MVPD は、次の指標を監視できます。

| *指標名* | *説明* |
|---|---|
| authn-attempts | 開始された認証フローの数 |
| authn-successful | クライアントが正常に取得した認証トークンの数 |
| authn-pending | 正常に生成された認証トークンの数（クライアントが実際に取得したかどうかに関係ありません） |
| authn-failed | 外部システムを介して実行された失敗した認証の数。 |
| authz-attempts | 試行された承認数 |
| authz-successful | 成功した承認数 |
| authz-failed | アプリケーションレベルで MVPD によって拒否された認証の数 |
| authz-rejected | Adobe サービス プロバイダーによって悪意があると見なされ、DoS 攻撃防止の結果として拒否された認証試行の数 |
| authz-latency | MVPD エンドポイントで費やした合計ミリ秒数 |

### MVPD は、以下のディメンションによって上記の指標をフィルタリングできます。

| *Dimension名* | *説明* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 年 | 4 桁の年 |
| 月 | 年間通算月（1 ～ 12） |
| 日 | 月の特定の日（1 ～ 31） |
| 時間 | 時刻 |
| 分 | 時刻の分 |
| mvpd | 権利付与リクエストの実行に使用される mvpd ID |
| requestor-id | 権利付与リクエストの実行に使用する要求者 ID |
| eap | 認証フローが外部システムを介して実行される場合の外部認証プロバイダー。 </br> 値は次のとおりです。</br> – 該当なし – Adobe Passによって認証が提供された </br> Apple – 認証を提供した外部システムがApple |
| cdt | 現在クライアントレスに使用されているデバイスプラットフォーム（代替）。 </br> 値は次のとおりです。</br> – なし – イベントがクライアントレス SDKから発生しなかった </br> – 不明 – クライアントレス API からの deviceType パラメーターはオプションなので、値を含まない呼び出しがあります。 </br> - Clientless API を通じて送信されたその他の値（xbox、appletv、roku など） </br> |
| sdk タイプ | 使用するクライアントSDK（Flash、HTML 5、Android ネイティブ、iOS、クライアントレスなど） |
| platform | デバイスが識別したプラットフォーム。 使用可能な値：</br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> – など |
| nsdk | 使用されたクライアント SDK （android、fireTV、js、iOS、tvOS、non-sdk） |
| nsdk-version | Adobe Pass認証クライアント SDKのバージョン |

## ユースケース {#use-cases}

ESM データは、次のユースケースで使用できます。

- **監視** - オペレーションまたは監視チームは、API を 1 分ごとに呼び出すダッシュボードまたはグラフを作成できます。 表示された情報を使用して、表示された分で問題（Adobe Pass認証またはMVPD）を検出できます。

- **デバッグ/品質テスト** - データもプラットフォーム、デバイス、ブラウザー、OS ごとに分類されるので、使用パターンを分析して特定の組み合わせ（OSX の Safari など）で問題を特定できます。

- **Analytics** – 提供されたデータを使用して、Adobe Analyticsまたはその他の分析ツールで収集されているクライアントサイドのデータを補足または監査できます。

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->