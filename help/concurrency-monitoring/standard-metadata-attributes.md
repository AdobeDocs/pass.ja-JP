---
title: 標準メタデータ属性
description: 標準メタデータ属性
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 0%

---


# 標準メタデータ属性 {#std-metadata-attributes}

このページでは、同時実行監視サービスが処理でき、実装可能なポリシーの基礎として使用できるメタデータ属性の完全なリストを示します。 標準メタデータ属性は、次のように分類できます。

* デザインに含まれる属性（URL パスで必要とされるので、セッション初期化呼び出しのたびに送信されます）。 これらの値がなければ、有効な呼び出しは実行できません。
* メタデータ属性：セッション初期化呼び出し中にフォームデータとして渡す必要がある値（バックエンドポリシーに値が必要な場合）。

## デザインに必要な属性 {#attr-req-by-design}

同時実行監視 API は、有効な初期化呼び出しの一部としてクライアントに次の値を送信させます。 [セッション開始呼び出し](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| フィールド名 | 値の例 | 使用場所 | 取得元 |
|-------------|---------------------------|--------------------|------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | 認証ヘッダー | 統合時の Zendesk チケット |
| mvpdName | Sample_MVPD | URI パス | ユーザーが MVPD を選択したときのAdobe Pass認証を config エンドポイントから |
| accountId | 12345 | URI パス | ユーザーログイン後のAdobe Pass認証の upstreamUserID メタデータ [ユーザーメタデータ upstreamUserID - Adobe Pass認証](/help/authentication/user-metadata-feature.md) |


## メタデータ属性 {#metadata-attr}

次の表に示すフィールドは、同時実行監視で実装されるポリシーを作成するために、Programmers および MVPDs で使用できます。

を使用 [API v2.0](http://docs.adobeptime.io/cm-api-v2/)定義されたポリシーでこれらの属性のいずれかが必要な場合、その属性を持たないセッションの初期化を試みると、400 Bad Request が発生します。


| エンティティ | 属性名 | データタイプ | 説明 | 外部参照（例： EIDR、OATC） | 値の例 | 検証ルール |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| メディア会社 | programmerName | 文字列 | プログラマーの名前 |                                                   | ProgrammerX |                                                                                   |
| リソース | チャネル | 文字列 | TV チャンネル |                                                   | ChannelY |                                                                                   |
|                 | assetId | 文字列 | このコンテンツに対して表示する、「わかりやすい」または消費者が読み取り可能なタイトル | [EIDR 2.0 データフィールドの参照](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | ベンハー |                                                                                   |
|                 | type | 列挙 | TveItem で表されるコンテンツの一般的なタイプを表す値です。 列挙された値は次のとおりです。 movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concert conference newsEvent sportingEvent trailer | [OATC メタデータフィードの推奨プラクティス](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | フィールドは、列挙内の項目の 1 つに対応している必要があります |
|                 | contentType | 文字列 | このフィールドは、リクエストされたコンテンツがライブか VOD かを決定します | 該当なし | ライブ、vod | live または vod |
|                 | ジャンル | 文字列 | ストリーミングされるコンテンツのジャンル。 一般的なプログラミングタイプを説明します | [OATC メタデータフィードを推奨](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} 練習 | コメディ | 有効なジャンルタイプ |
|                 | duration | 数値 | メディアアイテムの時間（秒） | [OATC メタデータフィードの推奨プラクティス](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | 番号列 |
| デバイス/ブラウザー | deviceId | 文字列 | 一意のデバイス識別子。 | [Device Atlas のプロパティ](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | 文字列 | このデバイスのわかりやすい名前。 |                                                   | ジョーズiPad |                                                                                   |
|                 | marketingName | 文字列 | デバイスのマーケティング名（または顧客にわかりやすい名前） | [Device Atlas のプロパティ](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | 有効なマーケティング名 |
|                 | mobileDevice | ブール型 | デバイスが移動時に使用される場合は true | [Device Atlas のプロパティ](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | 文字列 | デバイス、ブラウザーまたはその他のコンポーネントのモデル名 | [Device Atlas のプロパティ](https://deviceatlas.com/device-data/properties){target=_blank} | タブレット、電話、xbox。 セットトップボックス | 有効なデバイスモデル名 |
|                 | osName | 文字列 | デバイスが動作しているオペレーティングシステム | [Device Atlas - OS で事前定義されたプロパティ値](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android、Windows 10、OS X、Linux、その他の注意：プロパティの値を表示するには、Device Atlas でユーザー名とパスワードを使用してログインする必要があります。 | 期待される値は、Device Atlas で事前定義されたプロパティの値の 1 つです |
|                 | browserName | 文字列 | デバイス上のブラウザーの名前または種類 | [Device Atlas — ブラウザーで事前に定義されたプロパティ値](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | デバイス上のブラウザーの名前またはタイプ。  注意：プロパティの値を表示するには、Device Atlas でユーザー名とパスワードを使用してログインする必要があります | 期待される値は、Device Atlas で事前定義されたプロパティの値の 1 つです |
|                 | browserVersion | 文字列 | デバイス上のブラウザーのバージョン。 | [Device Atlas のプロパティ](https://deviceatlas.com/device-data/properties){target=_blank} | デバイス上のブラウザーのバージョン。 |                                                                                   |
| アプリ | applicationName | 文字列 | ユーザーにわかりやすい、または消費者が読み取り可能なアプリケーションの名前 | 該当なし | Sample_Application |                                                                                   |
|                 | applicationId | 文字列 | クライアントアプリケーションを一意に識別するアプリケーション ID。 | 該当なし | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | 文字列 | アプリケーションのネイティブプラットフォーム | 該当なし | ios, android |                                                                                   |
|                 | applicationVersion | 文字列 | この値は、分析目的で使用できます | 該当なし | 1.0, 2.0 |                                                                                   |
| 件名 | accountId | 文字列 | 同時実行監視サブジェクトのアカウント ID （MVPD のスコープ内） | 該当なし | test-account |                                                                                   |
|                 | contractType | 文字列 | premium、basic。 顧客は、これをカスタムメタデータとして自由に追加し、独自のレルム内で使用できます。 | 該当なし | premium、basic |                                                                                   |
| ユーザー | 名前 | 文字列 | 一部の MVPD は、コンテンツを再生する特定のユーザーに関する情報を提供します。 | 該当なし |                                                                                                                                                         |                                                                                   |
|                 | hba | ブール型 | ユーザーが自宅の場所からストリームを開始しようとするかどうかを識別します。 | 該当なし | true, false | true または false |
| 場所 | 大陸 | 文字列 | 再生リクエストを送信する deviceID の発信元となる大陸 | 該当なし | 北米 | 有効な大陸名 |
|                 | 国 | 文字列 | 再生リクエストを送信する deviceID の送信元の国 | 該当なし | USA | 有効な国名 |
|                 | state | 文字列 | 再生リクエストを送信する deviceID の送信元の状態 | 該当なし | CA | 有効な状態名 |
|                 | 市区町村 | 文字列 | 再生リクエストを送信する deviceID の送信元の市区町村 | 該当なし | Cupertino | 有効な市区町村名 |
|                 | zipcode | 数値 | 再生リクエストを送信する deviceID の送信元の zipcode | 該当なし | 95014 | 有効な zipcode |
| ストリーム | streamId | 文字列 | 顧客管理ではなく、CM サービスによって生成されます。 maxstreams タイプのルールが定義されている場合に暗黙的に使用されます。 | 該当なし | 該当なし | 該当なし |
|                 | streamCDN | 文字列 | ストリームが取得された CDN を示します。 | 該当なし | 該当なし | 該当なし |

## ポリシーの作成にメタデータ属性を使用する例 {#examples-metadata-attr}

標準メタデータフィールドは、フィールド値に基づいてサーバー側ポリシーを定義するために使用できます。

* 特定のフィールド値にのみ適用するようにポリシーを設定できます ( 例えば、専用のiOSポリシーを適用する場合： `osType` 次に該当 `iOS`)
* 特定のフィールドのユニーク値の数を制限できます。 次に例を示します。
   * 最大 X 台の個別デバイス： `HAVING DISTINCT COUNT(deviceId) <= 2`
   * X 個以下の個別郵便番号： `HAVING DISTINCT COUNT(zipcode) <= 3`
* フィールド値あたりのアクティブストリームの数を制限できます。 次に例を示します。
   * 1 つのデバイスタイプに対して最大 X 個のアクティブストリーム： `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * ライブコンテンツのストリームのアクティブストリームは X 以下： `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

次の方法で同時実行監視チームに連絡してください： [zendesk でのチケットの作成](mailto:tve-support@adobe.com) を選択し、実装するポリシーを指定します。

ポリシーおよび統合 cookbook のその他の例については、以下を参照してください。

* [ポリシーの決定ポイント](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API コンソール —Adobeの同時実行監視](http://docs.adobeptime.io/cm-api-v2/)
