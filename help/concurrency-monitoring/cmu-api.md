---
title: API の概要
description: API の概要
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '2054'
ht-degree: 0%

---


# 同時実行監視使用 API {#cmu-api-usage}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## API の概要 {#api-overview}

同時実行監視使用状況 (CMU) は WOLAP （Web ベース）として実装されています [オンライン分析処理](http://en.wikipedia.org/wiki/Online_analytical_processing)) プロジェクトです。 CMU は、Data Warehouse をベースとする、ビジネスでレポートされる汎用の Web API です。 通常の OLAP 操作を RESTfully で実行できる HTTP クエリ言語として機能します。


>[!NOTE]
>
>CMU API は一般には利用できません。 可用性に関するご質問は、Adobe担当者にお問い合わせください。

CMU API は、基になる OLAP キューブの階層ビューを提供します。 各リソース ([ディメンション](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) ディメンション階層で、URL パスセグメントとしてマッピングされている場合は、で（集計された）レポートが生成されます。 [指標](/help/authentication/entitlement-service-monitoring-overview.md#programmers-can-monitor-the-following-metrics) 現在の選択範囲の。 各リソースは、親リソース（ロールアップ用）とそのサブリソース（ドリルダウン用）を指します。 スライスとダイシングは、クエリー文字列パラメータを使用して、特定の値または範囲に対する寸法を固定することで実現されます。

REST API は、ディメンションパス、提供されたフィルター、選択した指標に従って、リクエストで指定された時間間隔（指定されていない場合はデフォルト値にフォールバック）で使用可能なデータを提供します。 時間範囲は、時間ディメンション（年、月、日、時間、分、秒）を含まないレポートには適用されません。

エンドポイント URL のルートパスは、1 つのレコード内の全体的な集計指標と、使用可能なドリルダウンオプションへのリンクを返します。 API バージョンは、エンドポイント URI パスの末尾のセグメントとしてマッピングされます。 例： https://mgmt.auth.adobe.com/cmu/*v2* は、クライアントが WOLAP バージョン 2 にアクセスすることを意味します。

使用可能な URL パスは、応答に含まれるリンクを通じて検出できます。 有効な URL パスは、（事前に）集計された指標を保持する、基礎となるドリルダウン・ツリー内のパスをマッピングするために保持されます。 /dimension1/dimension2/dimension3 という形式のパスは、これら 3 つのディメンションの事前集計を反映します（SQL 句 GROUP BY dimension1、dimension2、dimension3 と同じ）。 このような事前集計が存在せず、システムがその場で計算できない場合、API は 404（見つかりません）応答を返します。

### ドリルダウンツリー {#drill-down-tree}

次のドリルダウンツリーは、CMU 2.0 で使用可能な寸法（リソース）を示しています。

**Dimensionは CM テナントが利用できます**

![](assets/new_breakdown.png)

A `GET` から `https://mgmt.auth.adobe.com/cmu/v2` API エンドポイントは、次を含む表現を返します。

* 使用可能なルートドリルダウンパスへのリンク：

  ```html
  <link rel="drill-down" href="/cmu/v2/dimensionA"/>
  <link rel="drill-down" href="/cmu/v2/dimensionB"/>
  ```

* すべての指標の概要（デフォルトの間隔での集計値）。クエリー文字列パラメーターが指定されていないので、以下を参照してください。

ドリルダウンパス（ステップバイステップ）に従う： /dimensionA/year/month/day/dimensionX は、次の応答を取得します。

* 「dimensionY」および「dimensionZ」ドリルダウンオプションへのリンク
* dimensionX の各値の日別の集計を含むレポート

### **フィルター**

日付/時間ディメンションを除き、現在の投影（ディメンションパス）に使用できるディメンションは、名前をクエリー文字列パラメーターとして使用してフィルタリングできます。

次のフィルターオプションを使用できます。

* **次と等しい** フィルターは、クエリー文字列内の特定の値にディメンション名を設定することで提供されます。
* **IN** フィルターは、異なる値を持つ同じ dimension-name パラメーターを複数回追加することで指定できます。 dimension=value1&amp;dimension=value2
* **等しくない** フィルターは「!」を使用する必要があります ディメンション名の後に記号を付けると、「!」が生成されます。=&#39; &quot;operator&quot;: dimension!=値
* **次に含まれない** フィルターには「!」が必要です=&#39;演算子を複数回使用します。set: dimension!=値 1&amp;ディメンション！=値 2&amp;...


クエリー文字列内のディメンション名には特別な使用法もあります。ディメンション名が値のないクエリー文字列パラメーターとして使用される場合、API はそのディメンションをレポートに含む投影を返すように指示します。

CMU クエリの例：

| URL | 同等の SQL |
|:---|:---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * from projection WHERE dimension1 = &#39;value1&#39; GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1=value1&amp;dimension1=value2 | SELECT * from projection WHERE dimension1 IN (&#39;value1&#39;, &#39;value2&#39;) GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=value1 | SELECT * from projection WHERE dimension1 &lt;> &#39;value1&#39; GROUP BY dimension1, dimension2, dimension3 |
| /dimension1/dimension2/dimension3?dimension1!=値 1&amp;ディメンション 2!=value2 | SELECT * from projection WHERE dimension1 NOT IN (&#39;value1&#39;, &#39;value2&#39;) GROUP BY dimension1, dimension2, dimension3 |
| 次の直接パスがないと仮定します。 /dimension1/dimension3 があり、パスは/dimension1/dimension2/dimension3 です。  </br></br> /dimension1?dimension3 | SELECT * from projection GROUP BY dimension1,dimension3 |

>[!NOTE]
>
>これらのフィルタリングテクニックは、日付/時間ディメンションに対しては機能しません。 日付/時間ディメンションをフィルタリングする唯一の方法は、開始および終了クエリー文字列パラメーター（以下で説明）を必要な値に設定することです。

次のクエリー文字列パラメーターは、API 用に予約された意味を持ちます（したがって、ディメンション名として使用できません。そうでない場合、そのようなディメンションではフィルタリングは使用できません）。

CMU API 予約済みクエリー文字列パラメーター：

| パラメーター | オプション | 説明 | デフォルト値 | 例 |
|:--------------|:--------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|
| access_token | はい | IMS OAuth 保護が有効になっている場合、IMS トークンは、標準の認証ベアラートークンまたはクエリ文字列パラメーターとして渡すことができます。 | なし | access_token=XXXXX |
| dimension-name | はい | 任意のディメンション名（現在の URL パスまたは有効なサブパスに含まれる名前）。値は等号フィルターとして扱われます。 値を指定しない場合、指定したディメンションが現在のパスに含まれていない場合や現在のパスに隣接している場合でも、指定したディメンションが出力に強制的に含まれます | なし | someDimension=someValue&amp;someOtherDimension |
| 終了 | はい | ミリ秒単位のレポートの終了時間 | サーバーの現在の時刻 | end=2012-07-30 |
| 形式 | はい | コンテンツのネゴシエーションに使用されます ( 同じ効果ですが、パス「拡張」よりも低い優先順位です（以下を参照）。 | なし：コンテンツネゴシエーションは他の戦略を試みます | format=json |
| 制限 | はい | 返す行の最大数 | リクエストで制限が指定されていない場合に、サーバーが自己リンクで報告するデフォルト値 | limit=1500 |
| 指標 | はい | 返される指標名のコンマ区切りリスト。これは、使用可能な指標のサブセットをフィルタリング（ペイロードサイズを小さくする）する場合と、要求された指標を含む投影（デフォルトの最適投影ではなく）を返す API の強制の両方に使用します。 | このパラメーターが指定されていない場合、現在の投影で使用できるすべての指標が返されます。 | metrics=m1,m2 |
| 開始 | はい | レポートの開始時間 (ISO8601)。プレフィックスのみが指定されている場合、サーバーは残りの部分に入力します。例えば、start=2012 は start=2012-01-01となります。:00:00:00 | サーバーからセルフリンクでレポートされました。サーバーは、選択した時間の精度に基づいて適切なデフォルトを提供しようとします。 | start=2012-07-15 |


現在使用可能な HTTP メソッドはGETのみです。 OPTIONS/HEAD方法のサポートは、今後のバージョンで提供される可能性があります。



## CMU API ステータスコード {#cmu-api-status-codes}

| ステータスコード | 理由フレーズ | 説明 |
|:-----------|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200 | OK | 応答には、「ロールアップ」リンクと「ドリルダウン」リンク（該当する場合）が含まれます。 レポートは、リソースの属性（ネストされた「レポート」要素またはプロパティ）としてレンダリングされます。 |
| 400 | 無効なリクエスト | 応答本文には、リクエストの問題を説明するテキストメッセージが含まれます。  400 Bad Request ステータスには、応答本文（プレーン/テキストメディアタイプ）に説明テキストが付属し、クライアントエラーに関する有用な情報を提供します。 無効な日付形式やフィルターが存在しないディメンションに適用されるなどの簡単なシナリオに加えて、大量のデータを即座に返したり集計したりする必要があるクエリに対する応答も拒否されます。 |
| 401 | 未認証 | ユーザーを認証するために適切な OAuth ヘッダーを含まないリクエストが原因です |
| 403 | Forbidden | 現在のセキュリティコンテキストで要求が許可されていないことを示します。これは、ユーザーが認証済みで、要求された情報へのアクセスが許可されていない場合に発生します |
| 404 | 見つかりません | 無効な URL パスが要求で指定された場合に発生します。 クライアントが 200 件の応答を提供する「ドリルダウン」/「ロールアップ」リンクに従う場合、この問題は発生しないでください |
| 405 | 許可されていないメソッド | リクエストでサポートされていないメソッドが使用されたことを示すシグナル。 現在はGETメソッドのみがサポートされていますが、今後のバージョンではHEADやOPTIONSが可能になる可能性があります |
| 406 | 許容不可 | サポートされていないメディアタイプがクライアントによってリクエストされたことを示すシグナル |
| 500 | 内部サーバーエラー | 「こんなことは起こらない」 |
| 503 | サービス利用不可 | アプリケーション内またはその依存関係内でエラーを示します。 |

## データ形式 {#data-formats}

データは次の形式で利用できます。

* JSON （デフォルト）
* XML
* CSV
* HTML（デモ用）


次のコンテンツネゴシエーション戦略は、クライアントが使用できます（優先順位はリスト内の位置によって決まります。まず最初に行います）。

1. URL パスの最後のセグメントに追加された「ファイル拡張子」( 例：/cmu/v2/tenant/year/month/day.xml)。 URL にクエリー文字列が含まれる場合、拡張機能は疑問符の前に付く必要があります。 `/cmu/v2/tenant/year/month/day.csv?mvpd=SomeMVPD`
1. 形式クエリー文字列パラメーター：例： `/cmu/report?format=json`
1. 標準の HTTP Accept ヘッダー：例： `Accept: application/xml`

「extension」とクエリパラメーターの両方で、次の値がサポートされます。

* xml
* json
* csv
* html

いずれかの戦略でメディアタイプが指定されていない場合、API はデフォルトで JSON コンテンツを生成します。

## ハイパーテキストアプリケーション言語 (HAL) {#hypertext-app-lang}

JSON および XML の場合、ペイロードは、以下に説明するように HAL としてエンコードされます。 `http://stateless.co/hal_specification.html`.

実際のレポート（「report」と呼ばれるネストされたタグ/プロパティ）は、次のようにエンコードされ、選択した/適用可能なすべてのディメンションと指標を含むレコードの実際のリストで構成されます。

### JSON {#json}

```js
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML {#xml}

```xml
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report>
```

XML 形式および JSON 形式の場合、レコード内のフィールド（ディメンションおよび指標）の順序は指定されませんが、一貫しています（順序はすべてのレコードで同じです）。 ただし、クライアントはレコード内のフィールドの特定の順序に依存しないでください。

リソースリンク（JSON の「自己」リリースと、XML の「href」リソース属性）には、現在のパスと、インラインレポートに使用されるクエリ文字列が含まれます。 クエリ文字列は、暗黙的および明示的なパラメーターをすべて表示し、ペイロードは、使用された時間間隔、暗黙的なフィルター（存在する場合）などを明示的に示します。 リソース内の残りのリンクには、現在のデータを掘り下げるために実行できる、使用可能なすべてのセグメントが含まれます。 ロールアップ用のリンクも提供され、親パス（存在する場合）を指します。 The `href` ドリルダウン/ロールアップリンクの値には、URL パスのみが含まれます（クエリー文字列は含まれないので、必要に応じてクライアントによって追加される必要があります）。 現在のリソースで使用（または暗黙）されるすべてのクエリー文字列パラメーターが、「ロールアップ」リンクや「ドリルダウン」リンクに適用されるわけではありません（例えば、フィルターはサブリソースやスーパーリソースには適用されません）。

例 ( クライアントと呼ばれる指標が 1 つあり、 `year/month/day/...`):

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.xml`

  ```xml
  <resource href="/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
    <links>
      <link rel="roll-up" href="/cmu/v2/year"/>
      <link rel="drill-down" href="/cmu/v2/year/month/day"/>
    </links>
    <report>
      <record month="6" year="2012" clients="205"/>
      <record month="7" year="2012" clients="466"/>
    </report>
  </resource>
  ```

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.json`

  ```js
  {
    "_links" : {
      "self" : {
        "href" : "/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
      },
      "roll-up" : {
        "href" : "/cmu/v2/year"
      },
      "drill-down" : {
        "href" : "/cmu/v2/year/month/day"
      }
    },
    "report" : [ {
      "month" : "6",
      "year" : "2012",
      "clients" : "205"
    }, {
      "month" : "7",
      "year" : "2012",
      "clients" : "466"
    } ]
  }
  ```

### CSV {#csv}

CSV データ形式では、リンクや他のメタデータ（ヘッダー行を除く）はインラインで提供されません。代わりに、選択メタデータはファイル名で提供され、次のパターンに従います。

```html
report__<start-date>_<end-date>_<filter-values,...>.csv
```

CSV にはヘッダー行が含まれ、その後の行としてレポートデータが含まれます。 ヘッダー行には、すべてのディメンションに続くすべての指標が含まれます。 レポートデータの並べ替え順は、ディメンションの順序に反映されます。 したがって、データが D1 で並べ替えられ、次に D2 で並べ替えられる場合、CSV ヘッダーは次のようになります。 `D1, D2, ...metrics....`

ヘッダー行のフィールドの順序は、テーブルデータの並べ替え順を反映しています。

例： https://mgmt.auth.adobe.com/cmu/v2/year/month.csvは、という名前のファイルを生成します。 ```report__2012-07-20_2012-08-20_1000.csv``` 次の内容を含む：

| 年 | 月 | クライアント |
|:----:|:-----:|:-------:|
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## データの鮮度 {#data-freshness}

リクエストには Last-Modified ヘッダーが含まれていますが、 **DOES NOT** 本文のレポートが最後に更新された時間を反映します。 一般レポートは、次のルールに従って、定期的に計算されています。

* 時間の精度が **年** または **月**&#x200B;に設定されている場合、レポートは 2 日ごとに更新されます
* 時間の精度が **日**&#x200B;を設定しない場合、レポートは 3 時間ごとに更新されます
* 時間の精度が **時間**&#x200B;を指定しない場合、レポートは 1 時間ごとに更新されます
* 時間の精度が **分**&#x200B;を指定しない場合、レポートは毎分更新されます

The **アクティビティレベル** および **同時実行レベル** レポートは、時間の精度に関係なく、毎日更新されます。

## GZIP 圧縮 {#gzip-compression}

Adobeでは、CMU レポートを取得するクライアントで gzip のサポートを有効にすることを強くお勧めします。 これをおこなうと、応答のサイズが大幅に小さくなり、応答時間が短縮されます。 （CMU データの圧縮率は 20～30 の範囲にあります）。

クライアントで gzip 圧縮を有効にするには、Accept-Encoding：ヘッダーを次のように設定します。

```
Accept-Encoding: gzip, deflate
```

## 関連情報 {#related-information}

* [CMU の概要](/help/concurrency-monitoring/cm-usage-reports.md)
