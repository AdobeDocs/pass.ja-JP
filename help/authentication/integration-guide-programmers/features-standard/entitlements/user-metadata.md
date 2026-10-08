---
title: ユーザーメタデータ
description: ユーザーメタデータ
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1936'
ht-degree: 0%
---
# ユーザーメタデータ {#user-metadata}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

ユーザーメタデータは、ユーザー固有の[属性](#attributes)を指します（例：郵便番号、親の評価、ユーザーIDなど）。 Adobe Pass Authentication [REST API V2](#apis)を通じてプログラマーに提供されます。

認証フローが完了すると、ユーザーメタデータが使用できるようになります。ただし、MVPDと特定のメタデータ属性に応じて、認証フロー中に特定のメタデータ属性が更新される場合があります。

ユーザーメタデータは、ユーザーのパーソナライゼーションを強化するために使用できますが、分析にも使用できます。 例えば、プログラマーは、ユーザーの郵便番号を使用して、ローカライズされたニュースや天気の更新を配信したり、保護者の管理を強制したりできます。

Adobe Pass認証は、MVPDが異なる形式でデータを提供する場合、ユーザーメタデータ値を正規化します。 また、特定の属性（郵便番号など）については、プログラマーの証明書を使用して値を[暗号化](#encryption)できます。

Adobe Pass Authenticationを使用すると、プログラマーはMVPD統合で使用可能なユーザーメタデータを確認し、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)を通じて[管理](#management)できます。

## ユーザーメタデータ属性 {#attributes}

次の表に、プログラマーが使用できるユーザーメタデータ属性の一部を示します。

| キー | タイプ | サンプル | 暗号化が必要 | 説明 | 詳細 |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | 文字列 | 「1o7241p」 | いいえ | アカウント ID: | 属性値には、世帯識別子またはサブアカウント識別子を使用できます。 MVPDがサブアカウントをサポートしており、現在のユーザーがプライマリアカウント所有者でない場合、`userID`値は`householdID`とは異なります。 |
| `upstreamUserID` | 文字列 | 「1o7241p」 | いいえ | 同時実行モニタリング用のアカウント ID。 | 属性値を使用すると、MVPDおよびプログラマーサイトとアプリ全体で同時実行の制限を適用できます。 `upstreamUserID`の値は、ほとんどのMVPDの`userID`の値と同じです。 |
| `householdID` | 文字列 | 「1o7241p」 | いいえ | ペアレンタルコントロールのアカウント ID。 | 属性値は、世帯とサブアカウントの使用状況を区別するために使用できます。 真の評価が利用できない場合、家庭用アカウントでログインしていた場合、視聴可能な場合、視聴可能な場合、または視聴可能な場合は、評価されたコンテンツが表示されない場合は、ペアレンタルコントロールの代用として使用できる場合があります。 MVPDには、これを表現する方法に関するバリエーションが多くあります（例：世帯ユーザーID、世帯IDのヘッド、世帯フラグのヘッドなど）。MVPDがサブアカウントをサポートしていない場合、これは`userID`と同じになります。 |
| `primaryOID` | 文字列 | 「uuidd1e19ec9-012c-124f-b520-acaf118d16a0」 | いいえ | アカウント ID: | 属性はAT&amp;Tに固有です。`primaryOID`値は、`typeID`値が「プライマリ」に設定されている場合の`userID`値と同じです。 |
| `typeID` | 文字列 | 「プライマリ」 | いいえ | 現在のユーザーがプライマリまたはセカンダリの口座名義人かどうかを示す属性です。 | 属性はAT&amp;Tに固有です。`primaryOID`値は、`typeID`値が「プライマリ」に設定されている場合の`userID`値と同じです。 |
| `is_hoh` | 文字列 | &quot;1&quot; | いいえ | 現在のユーザーが世帯主かどうかを示す属性です。 | この属性はSynacorに固有です。 |
| `hba_status` | ブーリアン | 「true」 | いいえ | 現在のユーザーがHBAで認証されたかどうかを示す属性。 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | ブーリアン | 「true」 | いいえ | 現在のデバイスが画面をミラーリングできるかどうかを示す属性。 | 属性はSpectrumに固有です。 |
| `zip` | 配列 | \[&quot;77754&quot;, &quot;12345&quot;\] | はい | ユーザーの郵便番号。 | 属性値は、ローカライズされたニュース、天気の更新、スポーツイベントを配信するために使用できます。 `zip`値は、MVPDとの法的契約が必要な機密データを表します。 暗号化すると、`zip` キーの表現は`Array`ではなく`String`になります。 |
| `encryptedZip` | 文字列 | &quot;&quot; | はい | ユーザーの暗号化された郵便番号。 | 属性はComcastに固有です。 |
| `channelID` | 配列 | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | いいえ | ユーザーが表示権限を持つチャネルのリスト。 | 属性値を使用すると、複数のネットワークを集約するポータルからさまざまなチャネルをフィルタリングできます。 このユーザーメタデータ属性の代わりに[事前承認API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)を使用して、ユーザーが利用できないチャネルを除外することをお勧めします。 |
| `maxRating` | オブジェクト | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | いいえ | 現在のユーザーに対する親の最大評価。 | 属性値は、「MPAA」または「VCHIP」の評価に基づいて、現在のユーザーに適していないコンテンツをフィルタリングするために使用できます。 |
| `language` | 文字列 | 「英語」 | いいえ | 言語設定。 | 属性値を使用すると、ユーザーの言語設定に従ってメッセージを表示できます。 |

プログラマーが使用できるユーザーメタデータ属性は、MVPDの内容によって異なります。 次の表に、様々なMVPDで使用可能な属性を示します。

|                         | **契約書に署名しました（zipのみ）** | **AuthN**&#x200B;のユーザーID | **AuthN**&#x200B;のアップストリームユーザーID | **AuthN/Z**&#x200B;の世帯ID | **AuthN**&#x200B;のプライマリ OID | **AuthN**&#x200B;のタイプ ID | **AuthN**&#x200B;の世帯責任者 | **HBA ステータス** | **AuthZでのミラーリングを許可** | **AuthN/Z**&#x200B;の郵便番号 | **AuthN**&#x200B;のチャネル ID | **AuthN/Zの評価** | **言語** | **onNet** | **inHome** | **メモ** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **正式名** | なし | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **暗号化が必要** | なし | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **はい** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| **機密性** | なし | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **はい** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Adobe IdP | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **はい** | **はい** | **はい** | **No** | **No** | **はい（AuthNのみ）** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | 法的な合意は必要ありません。 |
| Synacor | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **はい** | **No** | **No** | **はい（AuthNのみ）** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | プロキシ化されたすべてのMVPDを対象としていない法的契約書。 これはSynacorの一般的なサポートであり、すべてのMVPDにロールアップされていない可能性があります。 |
| ディッシュ | **No** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | すべてのSynacor MVPDと同じリストに加えて`upstreamUserID`が共有されます。 |
| Comcast | **No** | **はい** | **はい** | **はい（AuthZのみ）** | **No** | **No** | **No** | **はい** | **No** | **No** | **No** | **はい（AuthZのみ）** | **No** | **No** | **No** |                                                                                                                                           |
| AT&amp;T | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **はい** | **はい** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | 法的文書が署名されました。 |
| DTV | **はい** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| COX | **No** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Cablevision | **はい** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **はい** | **No** | **No** | **No** | **No** | 法的文書が署名されました。 |
| Spectrum | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** |                                                                                                                                           |
| Charter | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** |                                                                                                                                           |
| Verizon | **No** | **はい** | **はい** | **No** | **No** | **No** | **No** | **はい** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| HTC | **No** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **はい** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| ロジャース | **No** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| RCN | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** |                                                                                                                                           |
| Eastlink | **No** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** |                                                                                                                                           |
| コジェコ | **No** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Videotron | **No** | **はい** | **はい** | **はい*** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | `userID`と同じ値の`householdID`が公開されます。 |
| Proxy Massilon | **はい** | **はい** | **はい** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** | 法的文書が署名されました。 |
| Proxy Clearleap | **はい** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **はい（AuthZのみ）** | **はい** | **No** | **No** | 法的文書が署名されました。 |
| プロキシ GLDS | **No** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **はい（AuthNのみ）** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| その他のMVPD | **No** | **はい** | **はい** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | まだ法的契約書がありません。本番環境では機密メタデータを使用できません。 すべてのMVPDに対して、`userID`は追加作業なしで使用できます。 |

>[!IMPORTANT]
>
> 機密性の高いユーザーメタデータ（郵便番号など）が利用可能になる前に、契約書をMVPDで署名する必要があります。

## ユーザーメタデータの暗号化 {#encryption}

ユーザーのメタデータ属性を暗号化および復号化するには、プログラマーが証明書（公開鍵/秘密鍵のペア）を生成し、[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)を通じて証明書を[自己設定](#management)するか、公開鍵をAdobe Pass Authentication担当者と共有する必要があります。

証明書が生成され、正しく設定されていることを確認するには、次の手順に従います。

1. OpenSSL ツールキット（http://www.openssl.org）をダウンロードしてインストールします。

1. 証明書署名要求（CSR）を生成します。

   * キーペアを生成します。 コマンドターミナルで次のコマンドを実行します。

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSRを生成します。 コマンドターミナルで次のコマンドを実行します。

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     秘密鍵のパスワードを入力するよう求められます。

   * 秘密鍵とパスワードのバックアップコピーを作成します。 CSRの例：

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. CSRを認証局（CA）に送信します（例：Verisign）。

1. CA は.p7b形式の証明書を送信します（PKCS#7、Cryptographic Message Syntax Standard）。

1. .p7b証明書をデプロイします。 秘密鍵を使用してPKCS#7 （.p7b）ファイルをPKCS#12 （PFX ファイル、Personal Information Exchange Syntax Standard）に変換し、PEM ファイル（連結された証明書コンテナファイル）を生成します。

   * PKCS#7 ファイルを一時PEM ファイルに変換します。 コマンドラインで次のコマンドを実行します。

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 一時PEM ファイルをPFX ファイルに変換します。 コマンドラインで次のコマンドを実行します。

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 一時PEM ファイルを最終的なPEM ファイルに変換します。 コマンドラインで次のコマンドを実行します。

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. PEM ファイルを使用して、[Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)経由で証明書を[設定](#management)するか、PEM ファイルをAdobe Pass Authentication担当者に送信します。

   * [Adobe Pass TVE ダッシュボード &#x200B;](https://experience.adobe.com/#/pass/authentication)を使用して証明書を管理する方法について詳しくは、次の節を参照してください。

   * Adobe Pass Authenticationは、プライマリ証明書とバックアップ証明書の両方をサポートしています。 何らかの形でプライマリ証明書が漏洩した場合は、プライマリ証明書を取り消して、セカンダリ証明書に切り替えることができます。 これにより、顧客への影響を最小限に抑えながら、証明書間でスムーズに移行できます。

## ユーザーメタデータ管理 {#management}

>[!IMPORTANT]
>
> Adobe Pass TVE ダッシュボードにアクセスできない場合は、[Zendesk](https://adobeprimetime.zendesk.com)からチケットを作成し、テクニカルアカウントマネージャー（TAM）に適切な変更を依頼してください。

Adobe Pass TVE ダッシュボードは、Adobe Pass認証のお客様（プログラマー）が設定とデータを管理するためのツールです。 このセルフサービスダッシュボードを使用すると、[Adobe Pass TVE ダッシュボードユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)のドキュメントに記載されている様々な機能を利用できます。

MVPDで使用可能なユーザーメタデータ属性を確認および管理するには、[TVE Dashboard User Guide for Integrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata) ドキュメントの手順に従います。

ユーザーのメタデータ属性の暗号化に使用される証明書を確認および管理するには、[TVE Dashboard プログラマー向けユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates)または[TVE Dashboard チャンネル向けユーザーガイド &#x200B;](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates)の手順に従います。

## REST API V2 {#rest-api-v2}

ユーザーメタデータ属性は、次のAPIを使用して取得できます。

* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定のmvpdのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

ユーザーメタデータ属性の構造については、上記のAPIの&#x200B;**応答**&#x200B;および&#x200B;**サンプル**&#x200B;の節を参照してください。

>[!IMPORTANT]
>
> 認証フローが完了すると、ユーザーメタデータが使用可能になります。そのため、クライアントアプリケーションは、プロファイル情報に既に含まれているため、[&#x200B; ユーザーメタデータ &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)情報を取得するために別のエンドポイントをクエリする必要はありません。

上記のAPIを統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本的なプロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

特定のメタデータ属性は、MVPDと特定のメタデータ属性に応じて、認証フロー中に更新される場合があります。 その結果、クライアントアプリケーションは、最新のユーザーメタデータを取得するために、上記のAPIを再度クエリする必要がある場合があります。

>[!MORELIKETHIS]
>
> [認証フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
