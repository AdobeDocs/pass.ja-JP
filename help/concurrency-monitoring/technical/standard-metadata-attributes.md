---
title: 標準メタデータ属性
description: 標準メタデータ属性
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1295'
ht-degree: 0%
---
# 標準メタデータ属性 {#std-metadata-attributes}

このページでは、同時視聴数モニタリングサービスが処理できるメタデータ属性と、実装可能なポリシーの基礎として使用できるメタデータ属性の包括的なリストを提供します。 標準のメタデータ属性は、次のように分類できます。

* デザインに含まれる属性（URL パスで必要とされるセッション初期化呼び出しごとに送信）。 これらの値がなければ、有効な呼び出しは実行できません。
* メタデータ属性：セッションの初期化呼び出し中にフォームデータとして渡す必要がある値（バックエンドポリシーに値が必要な場合）。

## 設計に必要な属性 {#attr-req-by-design}

同時実行モニタリング APIは、有効な初期化コールの一部として、クライアントに次の値を強制的に送信させます：[ セッション開始コール ](/help/concurrency-monitoring/technical/restrict-concurr-usage-mult-apps.md#api-calls-descr)。

| フィールド名 | 値の例 | 利用場所 | 次から取得 |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | 認証ヘッダー | 統合時のZendesk チケット |
| mvpdName | Sample_MVPD | URI パス | ユーザーがMVPDを選択した場合の設定エンドポイントからのAdobe Pass認証 |
| accountId | 12345 | URI パス | ユーザーログイン後のAdobe Pass認証upstreamUserID メタデータ [User Metadata upstreamUserID - Adobe Pass認証](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## メタデータ属性 {#metadata-attr}

以下の表のフィールドは、同時視聴数モニタリングで実装されるポリシーを作成するために、プログラマーおよびMVPDで使用できます。

[API v2.0](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)では、これらの属性のいずれかが定義されたポリシーで必要な場合、その属性を持たないセッションの初期化が試行されると、400件の不正なリクエストが発生します。


| エンティティ | 属性名 | データタイプ | 説明 | 外部参照（例：EIDR、OATC） | 値の例 | 検証ルール |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| メディア会社 | programmerName | 文字列 | プログラマーの名前 |                                                   | ProgrammerX |                                                                                   |
| リソース | チャネル | 文字列 | TV チャンネル |                                                   | ChannelY |                                                                                   |
|                 | assetId | 文字列 | このコンテンツに表示される「わかりやすい」タイトルまたは消費者が読みやすいタイトル | [EIDR 2.0 データ フィールド リファレンス ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | ベン=ハー |                                                                                   |
|                 | タイプ | 列挙 | TveItemで表されるコンテンツの一般的なタイプを表す値。 列挙された値には、次のものが含まれます。movie broadcastEpisode nonBroadcastEpisode musicVideo awardsShow clip concert conference newsEvent sportingEvent trailer | [OATC メタデータフィード推奨プラクティス ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | フィールドは、列挙のいずれかの項目に対応している必要があります |
|                 | contentType | 文字列 | このフィールドは、リクエストされたコンテンツがライブかVODかを判断します | 該当なし | ライブ、vod | ライブまたはvod |
|                 | ジャンル | 文字列 | ストリーミングされるコンテンツのジャンル。 一般的なプログラミングタイプを記述します | [OATC メタデータフィード推奨](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} プラクティス | コメディ | 有効なジャンルの種類 |
|                 | 期間 | 数値 | メディア項目の長さ（秒単位） | [OATC メタデータフィード推奨プラクティス ](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | 数列 |
| デバイス/ブラウザー | deviceId | 文字列 | 一意のデバイス ID。 | [ デバイス アトラスのプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | 文字列 | このデバイスのわかりやすい名前。 |                                                   | ジョーズ・iPad |                                                                                   |
|                 | marketingName | 文字列 | デバイスのマーケティング名（または顧客わかりやすい名前） | [ デバイス アトラスのプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | 有効なマーケティング名 |
|                 | mobileDevice | ブーリアン | デバイスが移動中に使用することを意図している場合はTrue | [ デバイス アトラスのプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | true、false | true、false |
|                 | deviceModel | 文字列 | デバイス、ブラウザーまたはその他のコンポーネントのモデル名 | [ デバイス アトラスのプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | タブレット、携帯電話、xbox。 セットトップボックス | 有効なデバイスモデル名 |
|                 | osName | 文字列 | デバイスが実行しているオペレーティングシステム | [ デバイス アトラス - OS定義済みプロパティ値](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android、Windows 10、OS X、Linux、その他メモ：プロパティ値を表示するには、Device Atlasのユーザー名とパスワードでログインする必要があります | 期待される値は、Device Atlas事前定義プロパティの値の1つです |
|                 | browserName | 文字列 | デバイス上のブラウザーの名前またはタイプ | [ デバイス アトラス – ブラウザーの定義済みプロパティ値](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | デバイス上のブラウザーの名前またはタイプ。  注：プロパティ値を表示するには、Device Atlasのユーザー名とパスワードでログインする必要があります | 期待される値は、Device Atlas事前定義プロパティの値の1つです |
|                 | browserVersion | 文字列 | デバイス上のブラウザーのバージョン | [ デバイス アトラスのプロパティ ](https://deviceatlas.com/device-data/properties){target=_blank} | デバイス上のブラウザーのバージョン |                                                                                   |
| アプリケーション | applicationName | 文字列 | アプリケーションの「ユーザーフレンドリー」または消費者が読みやすい名前 | 該当なし | Sample_Application |                                                                                   |
|                 | applicationId | 文字列 | クライアントアプリケーションを一意に識別するアプリケーション ID。 | 該当なし | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | 文字列 | アプリケーションのネイティブプラットフォームは | 該当なし | ios、android |                                                                                   |
|                 | applicationVersion | 文字列 | この値は、分析目的で使用できます | 該当なし | 1.0, 2.0 |                                                                                   |
| 件名 | accountId | 文字列 | 同時実行モニタリング対象のアカウント ID （MVPDの範囲） | 該当なし | test-account |                                                                                   |
|                 | contractType | 文字列 | プレミアム、基本。 顧客は、これをカスタムメタデータとして追加し、独自の領域で使用できます | 該当なし | プレミアム、基本 |                                                                                   |
| ユーザー | name | 文字列 | MVPDの中には、コンテンツを再生する特定のユーザーに関連する情報を提供するものもあります。 | 該当なし |                                                                                                                                                         |                                                                                   |
|                 | hba | ブーリアン | ユーザーが自宅の場所からストリームを開始しようとしているかどうかを識別します | 該当なし | true、false | trueまたはfalse |
| ロケーション | 大陸 | 文字列 | 再生リクエストを送信するdeviceIDの送信元の大陸 | 該当なし | 北米 | 有効な大陸名 |
|                 | 国 | 文字列 | 再生リクエストを送信するdeviceIDの送信元の国 | 該当なし | USA | 有効な国名 |
|                 | 都道府県 | 文字列 | 再生リクエストを送信するdeviceIDの送信元の状態 | 該当なし | CA | 有効な状態名 |
|                 | 市区町村 | 文字列 | 再生リクエストを送信するdeviceIDの送信元の都市 | 該当なし | クパチーノ | 有効な都市名 |
|                 | zipcode | 数値 | 再生リクエストを送信するdeviceIDの送信元のzip コード | 該当なし | 95014 | 有効な郵便番号 |
| ストリーム | streamId | 文字列 | カスタマーコントロールではなく、CM サービスによって生成されます。 は、maxstreams タイプのルールが定義されている場合に暗黙的に使用されます。 | 該当なし | 該当なし | 該当なし |
|                 | streamCDN | 文字列 | ストリームが取得されたCDNを示します | 該当なし | 該当なし | 該当なし |

## ポリシーの作成にメタデータ属性を使用する例 {#examples-metadata-attr}

標準メタデータフィールドは、フィールド値に基づいてサーバーサイドポリシーを定義するために使用できます。

* 特定のフィールド値にのみ適用するようにポリシーを設定できます（例えば、専用のiOS ポリシー：`osType`は`iOS`です）。
* 特定のフィールドの一意の値の数を制限できます。 次に例を示します。
  * x個を超える異なるデバイス：`HAVING DISTINCT COUNT(deviceId) <= 2`
  * 異なる郵便番号がX未満：`HAVING DISTINCT COUNT(zipcode) <= 3`
* フィールド値ごとにアクティブストリームの数を制限できます。 次に例を示します。
  * 1つのデバイスの種類：`GROUP BY deviceType HAVING COUNT(streamId) <= 3`のアクティブ ストリームはX個までです
  * ライブコンテンツのストリームのアクティブストリームはX個までです：`SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

同時視聴数モニタリング チームに連絡して、[Zendesk](mailto:tve-support@adobe.com)でチケットを作成し、実装するポリシーを指定してください。

ポリシーと統合クックブックの例については、次を参照してください。

* [ポリシー決定ポイント](/help/concurrency-monitoring/technical/cm-policy-decision-point.md)
* [API コンソール - Adobe同時視聴数モニタリング](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
