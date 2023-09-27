---
title: アカウント IQ の用語集
description: 製品用語集。
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 0%

---

# 製品の概念と用語集 {#glossary}

以下の製品用語とその定義を参照してください。

## [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

現在のセグメント共有スコアを、[ 最低 ]、[ 低 ]、[ 中 ]、[ 高 ]、[ 非常に高い ] の共有範囲カテゴリに分割したグラフ付きのダッシュボードパネル。

## [!UICONTROL Action] {#action-def}

関連する直接または間接のイベント [操作](#operation-def) 関連する操作セグメント（コホート）の特性（共有スコアや使用中のデバイスの数など）に影響を与えます。

## [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

現在のセグメントでスコアを共有している範囲のカテゴリ（非常に低い、低い、中程度、高い、非常に高い）に分割したグラフを含むダッシュボードパネル。また、セグメントのストリーミングの合計量に対する各カテゴリの割合も表示されます。

## [!UICONTROL AuthN] {#authn-def}

認証、または認証試行の回数。 認証の試行とは、現在有効な認証状態を持たないユーザーが、選択された MVPD にリダイレクトされるプロセスです。MVPD では、ユーザー名とパスワードを使用して、自身を MVPD に識別します。

## [!UICONTROL AuthN OK] {#authn-ok-def}

成功した認証の数。 ユーザーが MVPD によって確認され、その結果、ユーザーがプログラマーアプリまたはサイトにリダイレクトされると、認証が成功します。

## [!UICONTROL AuthZ] {#authz-def}

認証、または認証リクエストの数。 認証要求とは、プログラマがAdobeを通じて MVPD に対する許可を要求し、ユーザの要求コンテンツのストリーミングを開始する処理です。 通常、MVPD は、ユーザーの MVPD サブスクリプションに関連付けられたコンテンツ権限（例えば、コンテンツに関連付けられたチャネルがユーザーのサブスクリプション内にあるかどうか）に基づいてリクエストを許可します。 一部の認証リクエスト応答はAdobeによってキャッシュされ、Adobeは MVPD にリクエストを渡すことなく、即座に応答できます。

## [!UICONTROL AuthZ OK] {#authz-ok-def}

成功した認証の数。

## [!UICONTROL Channel] {#channel-def}

チャネルは、プロパティとも呼ばれ、主題に関連したビデオコンテンツのソースです。 従来は、MVPD からの個別の、数値でアドレス可能な連続ビデオフィードを表していました。 このチャネルは、セットトップボックス (STB) を通じて、購読者が利用できるコンテンツのアクセス可能なチャネルに直接マッピングされます。

## [!UICONTROL Cluster] {#cluster-def}

クラスターは、場所とデバイスの集まりです。 クラスタは、デバイス間の共通の場所を見つけることで作成されます。 共通の場所で認識されたデバイスは、同じクラスタに属していると見なされます。 共通の場所がない場合でも、2 台のデバイスを同じクラスタ内に配置できますが、他のデバイスの場所を介して接続できます。

### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

静的デバイスを持たないクラスター。

### [!UICONTROL Static cluster] {#static-cluster-def}

1 つ以上の静的デバイスを持つクラスター。

## [!UICONTROL Concurrency] {#consurrency-def}

同時実行は、同時に 2 つ（または複数）のストリームで定義されます。そのため、同時に再生されたストリームと非常に近いストリームとの間の間隔は、通常の速度での移動によって正当化されません。
同時使用量は、2 つの異なるクラスタ間の最大速度（マイル/時間）を使用して計算されます。 124 マイル未満の距離で 124 m/h を超える速度を持つ場合、または 124 マイルを超える距離で 400 m/h を超える速度を持つ場合、ユーザーは同時使用率を持つと見なされます。 距離は、異なるクラスタからの位置間で計算されます。 同じクラスターで同時に使用できます。

## [!UICONTROL Device] {#device-def}

TV Everywhere コンテンツを再生でき、Adobe Passでサポートされるデジタルビデオハードウェア製品。 例えば、スマートフォン、ノートパソコンとデスクトップコンピュータ、ゲーム機、スマートテレビなどです。

## [!UICONTROL Geographical Span] {#geographical-span-def}

一連の位置の最も遠い点間の距離。

## [!UICONTROL Granularity] {#granularity-def}

期間を基準とした、期間のサイズ（1 週間、1 か月など）。

## [!UICONTROL Industry Average Index] {#industry-avg-index-def}

選択した期間内のすべてのプログラマーおよび MVPD の各リスクインデックス（アカウント、使用状況、全体）に対して計算された値。

## [!UICONTROL IP] {#ip-def}

インターネットサービスプロバイダによってデバイスに割り当てられたインターネットプロトコルアドレス。 例えば、ケーブルサービスプロバイダ、セルサービスプロバイダなどです。

## [!UICONTROL Isolation Mode] {#isolation-mode-def}

共有分析の一種で、アカウントの評価は、選択したセグメントのプログラマーに直接発生したイベントに限定されます。  通常、すべてのアカウントイベントが評価され、より正確に共有を推定できます。  一部の MVPD データは、分離モード分析のみを可能にするように構造化されています。

## [!UICONTROL Location] {#location-def}

地球上のユニークな点です。 また、Pass データを使用して検出された特定の再生リクエストの位置情報とも呼ばれ、精度は 1000mx1000m（1 平方 km）です。

<!-- ### Home location {#home-location-def}

the precision is 0.01 ~ 2000mx2000m (4 square km) - at this moment the home location is detected using an ML algo, based on data provided by two mvpds. The probability is ~ 80%. We are not using the zip code for the majority of the users. Currently, this information is not used in assessing the sharing probability. -->

## [!UICONTROL Media Company] {#media-company-def}

メディア企業は、メディアネットワークのグループを所有する会社です。

## [!UICONTROL Metric] {#metric}

指標は、購読者のアカウントの属性（例えば、MVPD、ストリーミングするコンテンツのプログラマーやチャネル、使用するデバイスの数）です。

## [!UICONTROL Mobile device] {#mobile-device-def}

高いモビリティを持つデバイス。 例えば、携帯電話、タブレットなどです。

## [!UICONTROL MVPD] {#mvpd-def}

MVPD は、ディストリビューターとも呼ばれ、メディア企業のビデオコンテンツの集約、販売店、販売代理店です。

## 操作 {#operation-def}

操作は、特定の [アクション](#action-def) 関連付けられたセグメント上にある。 アクションの例としては、セグメントで識別されるアカウントで許可される同時ストリームの数に制限を設けることができます。

## [!UICONTROL Overall sharing score] {#overall-sharing-score}

ユーザーがプログラマープロパティまたは MVPD サブスクライバーでのパスワード共有の規模を理解し、それに作用する緊急感を与えるのに役立つ値。

<!--**Aggregated Risk Index**
Also known as Risk Index and Sharing Risk Index, it is a value that helps users understand the magnitude of password sharing on Programmer properties or by MVPD subscribers and provides them a sense of urgency to act upon it.-->

<!--**Risk Index - Overall**
A value computed as an average of "Risk Index - Accounts" and the "Risk Index - Usage". Overall Sharing Risk Index-->

## [!UICONTROL Play Request] {#play-requests-def}

ストリーム開始を記録および保護するためにメディアトークンをリクエストするために、クライアントアプリまたはAdobeがメディアに対しておこなったリクエストです。

## [!UICONTROL Programmer] {#programmer-def}

プログラマー (Network) は、1 つ以上のチャネルを所有および管理する大企業（企業）の子会社である会社です。

## [!UICONTROL requestorID] {#requestorid-def}

メディア会社が MVPD に対して自身または子会社を識別するために使用する ID。  プログラマの実装に応じて、これはメディア会社、プログラマ、またはチャネルにマッピングできます。  最も一般的メディア会社が MVPD に対する自身または子会社の識別に使用する ID。  プログラマの実装に応じて、これはメディア会社、プログラマ、またはチャネルにマッピングできます。  従来、これはチャネルにマッピングされていました。  MML(March Madness Live) のような擬似チャネルの作成や、MVPD 主導のデータ制限への技術的な対応に伴い、 requestorID はメディア会社との関連性が高まり始めています。

## [!UICONTROL resourceID] {#resource-id-def}

エンドユーザーがリクエストしたコンテンツ。  従来、このチャネルは、ユーザーがリクエストしたコンテンツに関連付けられたチャネルを識別していました。  システムが強化されたので、この ID は特定のプログラム（例：特定の評価を持つ）を表すことができ、ID は引き続き関連するチャネルを識別します。

## [!UICONTROL Risk Index - Usage] {#risk-index-usage}

「共有アカウントからの使用状況」とも呼ばれ、各アカウントの共有の確率で重み付けされた、各アカウントによる再生リクエストの数に基づいて計算される値です。 共有アカウントのリスクインデックスによる使用とも呼ばれます。

## [!UICONTROL Segment] {#segmet-def}

セグメントとは、選択した指標で指定されたユーザー定義の条件を満たす一連のアカウントです（例えば、「MVPD A、B、C、D、E のユーザーで、チャネル X、Y、Z を監視しているユーザー」）。

## [!UICONTROL Sharing level] {#sharing-level-def}

リスクインデックス — アカウントまたは共有アカウントのリスクインデックスとも呼ばれ、選択した期間に選択したプログラマーチャネルの 1 つからストリーミングされた選択した MVPD のセット内の各アカウントで計算された共有確率の平均に基づいて計算される値です。

## [!UICONTROL Static device] {#static-device-def}

モビリティが非常に低いデバイス。 例えば、ゲームコンソール、セットトップボックス、テレビセットなどです。

## [!UICONTROL Time frame] {#time-frame-def}

期間またはタイムスロットとも呼ばれ、ユーザーインターフェイスおよびテーブルで表される再生リクエストアクティビティを含む時間のウィンドウで、開始から終了までを指します。

## [!UICONTROL Top MVPDs in segment] {#top-mvpds-def}

共有レベル、共有アカウントからの使用状況、または全体的な共有スコアのいずれかによる測定として、選択したセグメント内の上位（最大 10）の MVPD です。

## [!UICONTROL Trend] {#trend-def}

現在の期間と前の期間の、関連する指標の差異（合計再生リクエストに対する割合など）。

## [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

特定の期間、Adobe Passが関与するプログラマー TV Everywhere アプリまたはサイトとやり取りした特定の期間のユニーク MVPD アカウントの数。  このインタラクションには、プログラマーアプリまたはサイト上で、Adobe Passサービスを呼び出すアクティビティが含まれます。 例えば、authN または authZ の状態の確認、認証、認証を行います。 プログラマーがAdobeTempPass （無料プレビュー）を使用しているデバイスがセグメントに含まれている場合、ユニーク購読者の合計数には、ユニークデバイスの数も含まれます。

## [!UICONTROL Usage] {#usage-defs}

### [!UICONTROL Avid user] {#avid-user-def}

1 ヶ月に 37 を超える再生リクエスト。

### [!UICONTROL Infrequent user] {#infrequent-users-def}

1 ヶ月あたり 9 件未満の再生リクエスト。

### [!UICONTROL Regular user] {#regular-user-def}

1 か月に 9 ～ 37 回の再生リクエスト。

## [!UICONTROL Usage Pattern] {#usage-patern-def}

アカウントに適用される有限のカテゴリラベルの 1 つで、ソーシャルグループや行動（例えば、小家族、旅行者や通勤者、ソーシャルシェアなど）に関して、アカウントのユーザーを最も特徴付けるものです。

## [!UICONTROL Zip Code] {#zip-code-def}

米国内の場所に関連付けられた米国の郵便番号
