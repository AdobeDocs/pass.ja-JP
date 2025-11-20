---
title: 標準メタデータ属性
description: 標準メタデータ属性
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# 標準メタデータ属性 {#std-metadata-attributes}

このページでは、同時実行監視サービスで処理でき、実装可能なポリシーの基礎として使用できるメタデータ属性の完全なリストを示します。 標準のメタデータ属性は、次のように分類できます。

* 設計に含まれる属性（URL パスに必要なので、セッション初期化呼び出しごとに送信）。 これらの値がないと、有効な呼び出しを実行できません。
* メタデータ属性：セッション初期化呼び出し中にフォームデータとして渡す必要のある値（バックエンドポリシーで値が必要な場合）。

## 設計に必要な属性 {#attr-req-by-design}

同時実行性監視 API は、有効な初期化呼び出しの一部として、クライアントに次の値を送信させます。[ セッション開始呼び出し ](/help/concurrency-monitoring/technical/restrict-concurr-usage-mult-apps.md#api-calls-descr)。

| フィールド名 | 値の例 | 使用する場所 | 取得元 |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | 認証ヘッダー | 統合時の Zendesk チケット |
| mvpdName | Sample_MVPD | URI パス | ユーザーがAdobe Passを選択した場合の、config エンドポイントからのMVPD認証 |
| accountId | 12345 | URI パス | ユーザーログイン後のAdobe Pass Authentication upstreamUserID メタデータ [User Metadata upstreamUserID - Adobe Pass Authentication](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## メタデータ属性 {#metadata-attr}

以下の表のフィールドは、プログラマーおよび MVPD が同時実行性監視に実装されるポリシーを作成する際に使用できます。

[API v2.0](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) では、定義されたポリシーでこれらの属性のいずれかが必要な場合、その属性を持たないセッションの init 試行は 400 Bad Request になります。


| Entity | 属性名 | データタイプ | 説明 | 外部参照（例：EIDR、OATC） | 値の例 | 検証ルール |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| メディア会社 | programmerName | string | プログラマーの名前 |                                                   | プログラマー X |                                                                                   |
| Resource | チャネル | string | TV チャンネル |                                                   | ChannelY |                                                                                   |
|                 | assetId | string | このコンテンツに対して提示される「わかりやすい」または消費者が読み取れるタイトル | [EIDR 2.0 データフィールドリファレンス ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | ベンフール |                                                                                   |
|                 | タイプ | 定義済みリスト | TveItem で表されるコンテンツの一般的なタイプを表す値です。 列挙された値は次のとおりです。movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concert conference newsEvent sportingEvent トレーラー | [OATC メタデータフィードの推奨プラクティス ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | フィールドは列挙の項目のいずれかに対応している必要があります |
|                 | contentType | string | このフィールドは、リクエストされたコンテンツがライブかVODかを判断します | 該当なし | ライブ、vod | 生きるまたは vod |
|                 | ジャンル | string | ストリーミングされるコンテンツのジャンル。 一般的なプログラミングタイプを記述します | [OATC メタデータフィード推奨 ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} プラクティス | コメディ | 有効なジャンルの種類 |
|                 | 期間 | 数値 | メディア アイテムの継続時間（秒） | [OATC メタデータフィードの推奨プラクティス ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | 番号順序 |
| デバイス/ブラウザー | deviceId | string | 一意のデバイス ID。 | [Device Atlas のプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | string | このデバイスのわかりやすい名前。 |                                                   | ジョーズiPad |                                                                                   |
|                 | marketingName | string | デバイスのマーケティング名（またはわかりやすい名前） | [Device Atlas のプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | 有効なマーケティング名 |
|                 | mobileDevice | ブール型 | デバイスが移動時に使用される場合は true | [Device Atlas のプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | true、false | true、false |
|                 | deviceModel | string | デバイス、ブラウザー、またはその他のコンポーネントのモデル名 | [Device Atlas のプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | タブレット、スマートフォン、xbox。 セットトップボックス | 有効なデバイスモデル名 |
|                 | osName | string | デバイスが実行しているオペレーティングシステム | [Device Atlas - OS で事前定義されたプロパティ値 ](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android、Windows 10、OS X、Linux、その他注意：プロパティ値を表示するには、Device Atlas にユーザー名とパスワードでログインする必要があります | 想定される値は、Device Atlas の事前定義済みプロパティの値の 1 つです |
|                 | browserName | string | デバイスのブラウザーの名前またはタイプ | [Device Atlas - ブラウザーの定義済みプロパティ値 ](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | デバイス上のブラウザーの名前またはタイプ。  メモ：プロパティ値を表示するには、Device Atlas にユーザー名とパスワードでログインする必要があります | 想定される値は、Device Atlas の事前定義済みプロパティの値の 1 つです |
|                 | browserVersion | string | デバイスのブラウザーバージョン | [Device Atlas のプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | デバイスのブラウザーバージョン |                                                                                   |
| 用途 | applicationName | string | 「ユーザーにわかりやすい」アプリケーションまたは消費者が読み取れる名前 | 該当なし | Sample_Application |                                                                                   |
|                 | applicationId | string | クライアントアプリケーションを一意に識別するアプリケーション ID。 | 該当なし | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | string | アプリケーションのネイティブプラットフォーム | 該当なし | ios、android |                                                                                   |
|                 | applicationVersion | string | この値は、分析目的で使用できます | 該当なし | 1.0、2.0 |                                                                                   |
| 件名 | accountId | string | 同時実行性モニタリングの件名のアカウント ID （MVPDの範囲内） | 該当なし | test-account |                                                                                   |
|                 | contractType | string | プレミアム、ベーシック。 顧客は自由にこれをカスタムメタデータとして追加し、独自のレルム内で使用できます | 該当なし | プレミアム、基本 |                                                                                   |
| ユーザー | name | string | 一部の MVPD は、コンテンツを再生する特定のユーザーに関連する情報を提供します。 | 該当なし |                                                                                                                                                         |                                                                                   |
|                 | hba | ブール型 | ユーザーが自宅の場所からストリームを開始しようとするかどうかを識別します | 該当なし | true、false | true または false |
| 場所 | 大陸 | string | 再生リクエストを送信するデバイス ID の送信元の大陸 | 該当なし | 北米 | 有効な大陸名 |
|                 | 国 | string | 再生リクエストを送信するデバイス ID の発信元の国 | 該当なし | 米国 | 有効な国名 |
|                 | state | string | 再生リクエストを送信するデバイス ID の発信元の状態 | 該当なし | CA | 有効な状態名 |
|                 | 市 | string | 再生リクエストを送信するデバイス ID の送信元の市区町村 | 該当なし | クペルティーノ | 有効な都市名 |
|                 | 郵便番号 | 数値 | 再生リクエストを送信するデバイス ID の送信元の郵便番号 | 該当なし | 95014 | 有効な郵便番号 |
| ストリーム | streamId | string | 顧客コントロールではなく、CM サービスによって生成されます。 maxstreams 型のルールが定義されている場合に暗黙的に使用されます。 | 該当なし | 該当なし | 該当なし |
|                 | streamCDN | string | ストリームの取得元の CDN を示します | 該当なし | 該当なし | 該当なし |

## ポリシー作成のためのメタデータ属性の使用例 {#examples-metadata-attr}

標準のメタデータフィールドを使用して、フィールド値に基づいてサーバーサイドのポリシーを定義できます。

* 特定のフィールド値にのみ適用されるようにポリシーを設定できます（例えば、`osType` が `iOS` の専用のiOS ポリシーなど）
* 特定のフィールドに対して、個別の値の数を制限できます。 次に例を示します。
   * 最大 X 台の個別デバイス数：`HAVING DISTINCT COUNT(deviceId) <= 2`
   * 最大 X 個の個別郵便番号：`HAVING DISTINCT COUNT(zipcode) <= 3`
* フィールド値あたりのアクティブなストリームの数を制限できます。 次に例を示します。
   * 1 つのデバイスタイプに対してアクティブなストリームは X 個以下にする：`GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * ライブコンテンツのストリームのアクティブなストリームは X 個以下にする：`SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

[Zendesk でチケットを作成 ](mailto:tve-support@adobe.com) して同時実行監視チームに連絡し、実装するポリシーを指定します。

ポリシーと統合クックブックの例については、次を参照してください。

* [ポリシーの決定ポイント](/help/concurrency-monitoring/technical/cm-policy-decision-point.md)
* [API コンソール - Adobe同時実行性の監視 ](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
