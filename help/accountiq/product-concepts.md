---
title: アカウント IQ の用語集
description: 製品の用語。
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# 製品の概念と用語集 {#glossary}

## D2C と TV Everywhere に共通する用語

以下の製品用語とその定義は、すべてのものに共通です。 [アカウント IQ のバージョン](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

現在のセグメントの共有スコアを、超低、低、中、高および超高の共有範囲カテゴリに分割するグラフを持つダッシュボードパネル。

### [!UICONTROL Action] {#action-def}

関連する直接的または間接的な出来事 [操作](#operation-def) 関連する操作セグメントの特性（共有スコアや使用中のデバイス数など）に影響する情報。

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

現在のセグメントの共有スコアを超低、低、中、高、超高の共有範囲カテゴリと、セグメントのストリーミングの合計量に対する各カテゴリの割合に分割するグラフを持つダッシュボードパネル。

### [!UICONTROL AuthN] {#authn-def}

認証の試行回数です。 認証試行とは、ユーザーが D2C サービスまたは MVPD を使用してログインを試みるプロセスです。 TV Everywhere ユーザーの場合、ユーザーは選択した MVPD にリダイレクトされ、MVPD に自分自身を識別します（通常はユーザー名とパスワード）。

### [!UICONTROL AuthN OK] {#authn-ok-def}

成功した認証の数。 認証が成功するのは、D2C サービスまたは MVPD によってユーザーの ID が確認された場合です。 Tv Everywhere ユーザーの場合、これによりユーザーはプログラマーアプリまたはサイトにリダイレクトされます。

### [!UICONTROL Cluster] {#cluster-def}

クラスターは、場所とデバイスのコレクションです。 クラスターは、デバイス間で共通の場所を見つけることで作成されます。 共通の場所で認識されたデバイスは、同じクラスターに属すると見なされます。 2 台のデバイスは、共通の場所を持たない場合でも同じクラスタ内に存在できますが、他のデバイスの場所を介して接続できます。

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

静的デバイスを持たないクラスター。

#### [!UICONTROL Static cluster] {#static-cluster-def}

1 つ以上の静的デバイスを持つクラスター。

### [!UICONTROL Concurrency] {#consurrency-def}

同時は、同時に再生される 2 つ（またはそれ以上）のストリームまたは非常に近い時間で再生されるストリームによって定義され、通常の速度で走行することによってそれらの間の間隔を正当化することはできません。
同時使用量は、2 つの異なるクラスター間の最大速度（マイル/時間）を使用して計算されます。 ユーザーが 124 マイル未満の距離で 124m/h を超える速度を持つ場合、または 124 マイルを超える距離で 400m/h を超える速度を持つ場合、同時使用と見なされます。 距離は、異なるクラスタの位置間で計算されます。 同じクラスター内で同時使用が許可されています。

### [!UICONTROL Device] {#device-def}

アップストリーミングコンテンツを再生できるデジタルビデオハードウェア製品を提供する。 例えば、スマートフォン、ノートパソコン、デスクトップコンピューター、ゲーム機、スマートテレビなどです。

### [!UICONTROL Evaluation period] {#evaluation-period-def}

評価期間は、操作に関連付けられたアクションの開始から、アクションの終了または測定までの時間です。

### [!UICONTROL Geographical Span] {#geographical-span-def}

一連の位置における最も遠いポイント間の距離。

### [!UICONTROL Granularity] {#granularity-def}

時間間隔を参照する、期間のサイズ（1 週間または 1 か月など）。

### [!UICONTROL IP] {#ip-def}

インターネット サービス プロバイダによってデバイスに割り当てられたインターネット プロトコル アドレス。 例えば、ケーブルサービスプロバイダーやセルサービスプロバイダーなどです。

### [!UICONTROL Location] {#location-def}

地球上のユニークなポイント。 これは、1000m x 1000m （1 平方 km）の精度を持つ特定の再生リクエストのジオロケーションとも呼ばれます。

### [!UICONTROL Media Company] {#media-company-def}

Media Company は、メディアネットワークのグループを所有する会社です。

### [!UICONTROL Metric] {#metric}

指標は、購読者のアカウントの属性（MVPD、ストリーミングするコンテンツのプログラマーとチャネル、使用するデバイスの数など）です。

### [!UICONTROL Mobile device] {#mobile-device-def}

機動性の高い装置を提供する。 例えば、携帯電話、タブレットなどです。

### 操作 {#operation-def}

操作は、特定の効果を追跡するために作成されたレコードです [アクション](#action-def) 関連付けられたセグメントで。 アクションの例としては、セグメントで識別されるアカウントに対して許可される同時ストリーム数の制限があります。

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

ユーザーがパスワード共有の大きさを理解し、それに対して緊急に対処できるようにするための値です。

### [!UICONTROL Play Request] {#play-requests-def}

ストリーム開始に相当します。 このイベントは、ユーザーのストリーミングコンテンツの開始をマークします。

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

共有アカウントからの使用状況とも呼ばれ、各アカウントによって行われた再生リクエストの数に基づいて計算された値を、各アカウントの共有確率で重み付けした値です。 これは、共有アカウントのリスクインデックスによる使用状況とも呼ばれます。

### [!UICONTROL Segment] {#segmet-def}

セグメントは、選択した指標で指定されたユーザー定義の条件を満たすアカウントのセットです。 例えば、「地域 A、B、C、D、E のユーザーで、デバイスが 3 台以上あるもの」などです。

### [!UICONTROL Sharing level] {#sharing-level-def}

リスク指標 – 取引先企業または共有取引先企業のリスク指標とも呼ばれ、選択した期間内に少なくとも 1 回ストリーミングした現在のセグメント内のすべての取引先企業について計算された共有確率の平均に基づいて計算された値です。

### [!UICONTROL Static device] {#static-device-def}

モビリティが非常に低いデバイス。 例えば、ゲーム機、セットトップボックス、テレビなどです。

### [!UICONTROL Time interval] {#time-interval-def}

期間とも呼ばれ、ユーザーインターフェイスおよび開始から終了までのテーブルに表示される再生リクエストアクティビティを含む期間です。

### [!UICONTROL Trend] {#trend-def}

現在の期間と前の期間の関連指標における差異の割合（合計再生リクエストの割合など）。

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

特定の期間中に少なくとも 1 回ストリーミングした一意のアカウントの数。

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

1 か月に 37 回以上の再生リクエスト。

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

再生リクエスト数が月に 9 件未満です。

#### [!UICONTROL Regular user] {#regular-user-def}

1 か月あたり 9～37 件の再生リクエスト。

### [!UICONTROL Usage Pattern] {#usage-patern-def}

ソーシャルグループまたは行動（例：小家族、旅行者または通勤者、ソーシャル共有など）の観点からアカウントのユーザーを最もよく特徴付けるアカウントに適用される有限のカテゴリラベルのセットの 1 つ。

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

ビデオカテゴリは、ビデオの特性を満たす特定のラベルです。 例えば、ソースの地域、コンテンツタイプ（VOD、ライブなど）、イベント、特定のラベル（チームなど）などです。

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

ビデオカテゴリは、ストリームに関連付けられたプログラマー、チャネル、および MVPD の組み合わせによって定義されます。

### [!UICONTROL Zip Code] {#zip-code-def}

米国内の場所に関連付けられた米国の郵便番号
<!--calculated metrics-->


## TV Everywhere 固有の用語

### [!UICONTROL AuthZ] {#authz-def}

Authorization または認証リクエスト数。 許可リクエストとは、プログラマーがAdobeを通じて MVPD に許可をリクエストし、ユーザーがリクエストしたコンテンツのストリーミングを開始するプロセスです。 MVPD は、通常、ユーザーの MVPD サブスクリプションに関連付けられたコンテンツ権限に基づいてリクエストを許可します（例えば、コンテンツに関連付けられたチャネルがユーザーのサブスクリプションに含まれているかどうか）。 一部の認証リクエストのレスポンスはAdobeによってキャッシュされるため、Adobeは MVPD にリクエストを渡すことなく即座に応答できます。

### [!UICONTROL AuthZ OK] {#authz-ok-def}

成功した認証の数。

### [!UICONTROL Channel] {#channel-def}

チャネル（プロパティとも呼ばれます）は、テーマに関連したビデオコンテンツのソースです。 従来は、MVPD からの個別の、数値でアドレス可能な連続ビデオフィードを表していました。 チャネルは、購読者がセットトップボックス（STB）を通じて使用できるコンテンツのアクセス可能なチャネルに直接マッピングされます。

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

選択した期間に、すべてのプログラマーおよび MVPD にわたるリスク指標（勘定科目、使用状況、全体）ごとに計算された値。

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

アカウントの評価が、選択したセグメントのプログラマーで直接発生したイベントに制限される、共有分析の一種。  通常は、すべてのアカウントイベントが評価されるので、共有をより正確に推定できます。  一部の MVPD データは、分離モード分析のみを可能にする方法で構成されています。

### [!UICONTROL MVPD] {#mvpd-def}

MVPD は、ディストリビューターとも呼ばれ、Media Company のビデオコンテンツのアグリゲーター、リセラー、ディストリビューターです。

### [!UICONTROL Programmer] {#programmer-def}

プログラマーは、ネットワークとも呼ばれ、1 つ以上のチャネルを所有および管理する大企業（企業）の子会社です。

### [!UICONTROL requestorID] {#requestorid-def}

メディア会社が MVPD に対して自分自身または子会社を識別するために使用する ID。  プログラマーの実装に応じて、これはメディア会社、プログラマー、チャネルのいずれかにマッピングできます。 従来、これはチャネルにマッピングされていました。  MML （March Madness Live）などの疑似チャネルの作成や、MVPD 駆動のデータ制限に対処するための技術的な動きにより、requestorID はメディア会社との関係が深まり始めています。

### [!UICONTROL resourceID] {#resource-id-def}

エンドユーザーがリクエストしたコンテンツ。 従来、この識別子は、ユーザーがリクエストしたコンテンツに関連付けられたチャネルを識別していました。  システムの機能強化により、ID で特定のプログラム（特定の評価など）を表すことができるため、ID は引き続き関連するチャネルを識別します。


