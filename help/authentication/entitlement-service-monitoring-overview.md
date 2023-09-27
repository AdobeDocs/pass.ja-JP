---
title: エンタイトルメントサービスの監視の概要
description: エンタイトルメントサービスの監視の概要
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1182'
ht-degree: 0%

---

# エンタイトルメントサービスの監視の概要 {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#introduction}

TVE のサイトとアプリは24/7で利用できる必要があるので、お客様は、できるだけ早く問題を検出して修正するために、使用権限イベントに関するリアルタイムのインサイトを得る必要があります。 また、トラフィックの大部分を提供しているプラットフォームと、実装が悪くコンバージョン率が低いプラットフォームを判断するために、月別データを分析する必要があります。

Entitlement Service Monitoring(ESM) は、Authentication と Authorization イベントをリアルタイムで表示できるデータフィードをプログラマーと MVPD に提供します。 データは、Adobe Pass認証システムから収集され、RESTful API を通じて提供されます。  お客様は、直接または独自に作成した操作用ダッシュボード内からデータを使用できます。

ESM システムの主な要素は、その指標とディメンションです。 ESM は、ディメンション選択に従って集計された指標を含むレポートを生成します。 Adobe Passのイベントが PST タイムゾーンで記録されるので、ESM レポートは PST タイムゾーンでも使用できます。

ESM API は一般には利用できません。  可用性に関するご質問は、Adobe担当者にお問い合わせください。

## プログラマ向け ESM {#esm-for-programmers}

### プログラマーは、次の指標を監視できます。 {#programmers-monitor-metrics}


| *指標名* | *説明* |
|-------------------------|--------------------------|
| authn-attempts | 開始された認証フローの数 |
| authn-successful | クライアントが正常に取得した認証トークンの数 |
| authn-pending | 正常に生成された認証トークンの数（クライアントが実際に取得したかどうかは関係ありません） |
| authn-failed | 外部システムを介して実行された失敗した認証の数。 |
| クライアントレストークン | 正常に発行されたクライアントレストークンの数 |
| クライアントレスの失敗 | クライアントレス API からトークンを受け取ろうとして失敗した回数 |
| authz-attempts | 試行された認証数 |
| authz-successful | 成功した認証の数 |
| authz-failed | アプリケーションレベルでの MVPD による拒否された認証の数 |
| authz-rejected | DoS 攻撃防止の結果、Adobe サービスプロバイダーによって悪意があると見なされて拒否された認証の回数 |
| authz-latency | MVPD のエンドポイントで費やされたミリ秒の合計数 |
| media-tokens | 生成された（再生リクエストの数と同化する）ショートメディアトークンの数 |
| 一意のアカウント | 選択した期間にエンタイトルメント (AuthN / AuthZ) アクションを実行したユニークユーザーの数。 （この指標は、毎日の値がリクエストされた場合にのみ表示されます） </br> これは、個々のデータセンターごとに計算されます。 「dc」ディメンションがリクエストされない場合、この指標は表示されません。 |
| unique-sessions | 選択した期間内にAdobe Pass Authentication サービスに対する認証フロー呼び出しを実行した一意のセッションの数。 （この指標は、毎日の値がリクエストされた場合にのみ表示されます） </br> これは、個々のデータセンターごとに計算されます。 「dc」ディメンションがリクエストされない場合、この指標は表示されません。 |
| count | イベント指向のレポートで使用されるシンプルなカウンター |

</br>

### プログラマーは、上記の指標を次のディメンションでフィルタリングできます。 {#progr-filter-metrics}


| *Dimension名* | *説明* |
|---|---|
| 年 | 4 桁の年 |
| 月 | 月 (1 ～ 12) |
| 日 | 月の日 (1-31) |
| 時間 | 時刻 |
| 分 | 時間の分 |
| media-company | ユーザーの使用権限プロセスを開始した Web サイトを所有するメディア会社 |
| dc | （データセンター）リクエストを受信したホーム地域。 |
| プロキシ | プロキシ MVPD （直接統合の場合は「直接」になります） |
| mvpd | ユーザーに対する権利付与を担当する MVPD |
| requestor-id | 権限付与リクエストの実行に使用する要求者 ID |
| チャネル | リソースフィールドから抽出されたチャネル Web サイト（MRSS ペイロードからチャネル/タイトルとして抽出されるもの。指定されていない場合はチャネル/タイトル、RSS 形式でない場合はリソース値にマッピングされるもの）。 |
| resource-id | 承認リクエストに関連する実際のリソースタイトル (MRSS ペイロードから抽出されます。項目/タイトル（指定されている場合）として抽出されます ) |
| デバイス | デバイスプラットフォーム（PC、モバイル、コンソールなど） |
| eap | 外部システムを介して認証フローが実行される場合の外部認証プロバイダ。 </br> 次の値を指定できます。 </br>  — なし — 認証はAdobe Pass Authentication によって提供されたものです。 </br> - Apple — 認証を提供した外部システムはAppleです。 |
| os ファミリー | デバイス上で動作するオペレーティングシステム |
| browser-family | Adobe Pass Authentication へのアクセスに使用するユーザーエージェント |
| cdt | デバイスプラットフォーム（代替）。現在、クライアントレスで使用されています。 </br>  次の値を指定できます。 </br>  — なし — イベントはクライアントレス SDK から発生しませんでした </br>  — 不明 — Clientless API の deviceType パラメーターはオプションなので、値を含まない呼び出しがあります。 </br>  — クライアントレス API を通じて送信されたその他の値（xbox、appletv、roku など） </br> |
| platform-version | クライアントレス SDK のバージョン。 |
| os-type | デバイス上で動作するオペレーティングシステム（代替） （現在は使用されていません） |
| browser-version | ユーザーエージェントのバージョン |
| sdk-type | 使用したクライアント SDK(Flash、HTML5、Android ネイティブ、iOS、クライアントレスなど ) |
| sdk-version | Adobe Pass Authentication Client SDK のバージョン。 |
| イベント | Adobe Pass Authentication イベント名 |
| 理由 | 失敗の理由 (Adobe Pass Authentication から報告 ) |
| sso-type | 基になる SSO メカニズム (platform/passive/adobe)。 別のアプリケーションで AuthN を再利用して認証トークンが発行されたことを示します |

## MVPD の ESM {#esm-for-mvpds}

### MVPDs は、次の指標を監視できます。

| *指標名* | *説明* |
|---|---|
| authn-attempts | 開始された認証フローの数 |
| authn-successful | クライアントが正常に取得した認証トークンの数 |
| authn-pending | 正常に生成された認証トークンの数（クライアントが実際に取得したかどうかは関係ありません） |
| authn-failed | 外部システムを介して実行された失敗した認証の数。 |
| authz-attempts | 試行された認証数 |
| authz-successful | 成功した認証の数 |
| authz-failed | アプリケーションレベルでの MVPD による拒否された認証の数 |
| authz-rejected | DoS 攻撃防止の結果、Adobe サービスプロバイダーによって悪意があると見なされて拒否された認証の回数 |
| authz-latency | MVPD のエンドポイントで費やされたミリ秒の合計数 |

### MVPDs では、上記の指標を次のディメンションでフィルタリングできます。

| *Dimension名* | *説明* |
|---|---|
| 年 | 4 桁の年 |
| 月 | 月 (1 ～ 12) |
| 日 | 月の日 (1-31) |
| 時間 | 時刻 |
| 分 | 時間の分 |
| requestor-id | 権限付与リクエストの実行に使用する要求者 ID |
| eap | 外部システムを介して認証フローが実行される場合の外部認証プロバイダ。 </br> 次の値を指定できます。 </br>  — なし — 認証はAdobe Pass Authentication によって提供されたものです。 </br> - Apple — 認証を提供した外部システムはAppleです。 |
| cdt | デバイスプラットフォーム（代替）。現在、クライアントレスで使用されています。 </br>  次の値を指定できます。 </br>  — なし — イベントはクライアントレス SDK から発生しませんでした </br>  — 不明 — Clientless API の deviceType パラメーターはオプションなので、値を含まない呼び出しがあります。 </br>  — クライアントレス API を通じて送信されたその他の値（xbox、appletv、roku など） </br> |
| sdk-type | 使用したクライアント SDK(Flash、HTML5、Android ネイティブ、iOS、クライアントレスなど ) |


## 使用例 {#use-cases}

ESM データは、次の使用例で使用できます。

- **監視**  — 業務チームや監視チームは、毎分 API を呼び出すダッシュボードやグラフを作成できます。 表示された情報を使用して、問題が発生した (Adobe Pass Authentication または MVPD で ) 即座に検出できます。

- **デバッグ/品質テスト**  — データはプラットフォーム、デバイス、ブラウザー、OS 別に分類されるので、使用パターンを分析すると、特定の組み合わせ（OSX 上の Safari など）に関する問題を特定できます。

- **Analytics**  — 提供されたデータは、Adobe Analyticsまたは他の分析ツールを使用して収集されるクライアント側のデータを補完/監査するために使用できます。

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
