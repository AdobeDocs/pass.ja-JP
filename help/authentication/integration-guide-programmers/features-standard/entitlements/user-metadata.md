---
title: ユーザーメタデータ
description: ユーザーメタデータ
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '1902'
ht-degree: 0%

---

# ユーザーメタデータ {#user-metadata}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

ユーザーメタデータとは、ユーザー固有の [ 属性 ](#attributes) 例：郵便番号、保護者による制限、ユーザー ID など）を指し、MVPD によって維持管理され、Adobe Pass認証 [REST API V2](#apis) を通じてプログラマーに提供されます。

ユーザーメタデータは、認証フローが完了すると使用可能になりますが、MVPDと問題の特定のメタデータ属性に応じて、特定のメタデータ属性が認証フロー中に更新される場合があります。

ユーザーメタデータはユーザーのパーソナライゼーションを強化するために使用できますが、分析にも使用できます。 例えば、プログラマーはユーザーの郵便番号を使用して、ローカライズされたニュースや天気の最新情報を配信したり、保護者による制限を実施したりできます。

MVPD が異なる形式でデータを提供する場合、Adobe Pass認証はユーザーメタデータ値を正規化します。 また、特定の属性（郵便番号など）については、プログラマーの証明書を使用して値を [ 暗号化 ](#encryption) できます。

Adobe Pass認証を使用すると、プログラマーは、[Adobe Pass TVE ダッシュボード ](#management) を通じて、MVPD統合で利用可能になったユーザーメタデータを確認し [ 管理 ](https://experience.adobe.com/#/pass/authentication) できます。

## ユーザーメタデータ属性 {#attributes}

次の表に、プログラマーが使用できるユーザーメタデータ属性の一部を示します。

| キー | タイプ | サンプル | 暗号化が必要 | 説明 | 詳細 |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | 文字列 | 「1o7241p」 | 不可 | アカウント識別子。 | 属性値は、世帯の識別子またはサブアカウントの識別子にすることができます。 MVPDがサブアカウントをサポートし、現在のユーザーがプライマリアカウントの所有者でない場合、`userID` の値は `householdID` とは異なります。 |
| `upstreamUserID` | 文字列 | 「1o7241p」 | 不可 | 同時実行監視のアカウント識別子。 | 属性値を使用すると、MVPDやプログラマーサイトおよびアプリに同時実行制限を適用できます。 `upstreamUserID` の値は、ほとんどの MVPD の `userID` の値と同じです。 |
| `householdID` | 文字列 | 「1o7241p」 | 不可 | 保護者による制限のアカウント識別子。 | 属性値は、世帯とサブアカウントの使用状況を区別するために使用できます。 場合によっては、真の評価が利用できない場合は、ペアレンタルコントロールの代用として使用することができます。ユーザーが世帯アカウントでログインした場合、彼らは見ることができます。そうしないと、評価されたコンテンツは表示されません。 これが表現される方法（世帯のユーザー ID、世帯の ID の長、世帯のフラグの長さなど）については、MVPD 間で多くのバリエーションがあり、MVPDがサブアカウントをサポートしていない場合、これは `userID` と同じになります。 |
| `primaryOID` | 文字列 | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | 不可 | アカウント識別子。 | この属性は AT&amp;T に固有です。`primaryOID` の値は、`userID` の値が「プライマリ」に設定されている場合の `typeID` の値と同じです。 |
| `typeID` | 文字列 | &quot;プライマリ&quot; | 不可 | 現在のユーザーがプライマリまたはセカンダリのアカウント所有者かどうかを示す属性。 | この属性は AT&amp;T に固有です。`primaryOID` の値は、`userID` の値が「プライマリ」に設定されている場合の `typeID` の値と同じです。 |
| `is_hoh` | 文字列 | &quot;1&quot; | 不可 | 現在のユーザーが世帯主かどうかを示す属性。 | 属性は Synacor に固有です。 |
| `hba_status` | ブール値 | &quot;true&quot; | 不可 | 現在のユーザーが HBA 経由で認証されているかどうかを示す属性。 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | ブール値 | &quot;true&quot; | 不可 | 現在のデバイスが画面をミラー化できるかどうかを示す属性。 | この属性は Spectrum に固有です。 |
| `zip` | 配列 | \[&quot;77754&quot;, &quot;12345&quot;\] | はい | ユーザーの郵便番号。 | この属性値は、ローカライズされたニュース、天気の最新情報、スポーツイベントの配信に使用できます。 `zip` 値は、MVPDとの法的契約を必要とする機密データを表します。 暗号化された場合、`zip` キーの表現は `String` ではなく `Array` になります。 |
| `encryptedZip` | 文字列 | &quot;&quot; | はい | ユーザーの暗号化された郵便番号。 | 属性は Comcast に固有です。 |
| `channelID` | 配列 | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | 不可 | ユーザーが閲覧できるチャネルのリスト。 | この属性値は、複数のネットワークを集約するポータルから各種チャネルをフィルタリングするために使用できます。 ユーザーが使用できないチャネルを除外するには、このユーザーメタデータ属性の代わりに [Preauthorize API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) を使用することをお勧めします。 |
| `maxRating` | オブジェクト | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | 不可 | 現在のユーザーの保護者による制限の上限。 | この属性値は、「MPAA」または「VCHIP」のレーティングに基づいて、現在のユーザーには適さないコンテンツをフィルタリングするために使用できます。 |
| `language` | 文字列 | &quot;English&quot; | 不可 | 言語設定。 | この属性値は、ユーザーの言語の環境設定に従ってメッセージを表示するために使用できます。 |

プログラマーが使用できるユーザーメタデータ属性は、MVPDが提供する内容によって異なります。 次の表に、様々な MVPD で使用できる属性を示します。

|                         | **署名済みの法的契約書（zip のみ）** | **AuthN のユーザー ID** | **AuthN のアップストリームユーザー ID** | **AuthN/Z の世帯 ID** | **AuthN のプライマリ OID** | **AuthN のタイプ ID** | **AuthN の世帯主** | **HBA ステータス** | **AuthZ でのミラーリングを許可** | **AuthN/Z の郵便番号** | **AuthN のチャネル ID** | **AuthN/Z の評価** | **言語** | **onNet** | **inHome** | **備考** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **正式名称** | 該当なし | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **暗号化が必要** | 該当なし | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| **機密** | 該当なし | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| Adobe IdP | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **はい** | **はい** | **はい** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | 法的合意は不要です。 |
| シナコール | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | プロキシ設定された MVPD のすべてをカバーしていない法的契約。 これは Synacor の一般的なサポートであり、すべての MVPD にロールアップされない可能性があります。 |
| 皿 | **いいえ** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | すべての Synacor MVPD と同じリストに加えて、`upstreamUserID` を共有します。 |
| 堆肥 | **いいえ** | **はい** | **はい** | **はい（AuthZ のみ）** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthZ のみ）** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| AT&amp;T | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| DTV | **はい** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| コックス | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| 有線情報 | **はい** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| スペクトル | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| 憲章 | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| ベライゾン | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| HTC | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| ロジャース | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| RCN | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| Eastlink | **いいえ** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| コジコ | **いいえ** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| Videotron | **いいえ** | **はい** | **はい** | **はい*** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | `householdID` と同じ値を持つ `userID` を公開します。 |
| プロキシマシロン | **はい** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| Proxy Clearleap | **はい** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthZ のみ）** | **はい** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| プロキシ GLDS | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |                                                                                                                                           |
| その他の MVPD | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約はまだありません。機密メタデータは実稼動環境では使用できません。 すべての MVPD では、`userID` は追加作業なしで使用できます。 |

>[!IMPORTANT]
>
> ユーザーの機密メタデータ（郵便番号など）を利用するには、事前に MVPD との間で法的契約を締結する必要があります。

## ユーザーメタデータの暗号化 {#encryption}

ユーザーメタデータの属性を暗号化および復号化するには、プログラマーが証明書（公開鍵と秘密鍵のペア）を生成し、{2[Adobe Pass TVE ダッシュボードを使用して証明書を ](#management) 自己設定 [ するか、公開鍵をAdobe Pass認証担当者と共有する必要があります。](https://experience.adobe.com/#/pass/authentication)

証明書が正しく生成および設定されていることを確認するには、次の手順に従います。

1. OpenSSL ツールキット（http://www.openssl.org）をダウンロードしてインストールします。

1. 証明書署名リクエスト（CSR）を生成します。

   * キーペアを生成します。 コマンドターミナルで次を実行します。

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSR を生成します。 コマンドターミナルで次を実行します。

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     秘密鍵のパスワードを入力するよう求められます。

   * 秘密鍵とパスワードのバックアップコピーを作成します。 サンプル CSR:

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

1. CSR を認証局（CA）（例：Verisign）に送信します。

1. CA から.p7b 形式（PKCS#7, Cryptographic Message Syntax Standard）で証明書が送信されます。

1. .p7b 証明書をデプロイします。 秘密鍵を使用して PKCS#7 （.p7b）ファイルを PKCS#12 （PFX ファイル、Personal Information Exchange Syntax Standard）に変換し、PEM ファイル（連結証明書コンテナファイル）を生成します。

   * PKCS#7 ファイルを一時 PEM ファイルに変換します。 コマンドラインで次を実行します。

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 一時 PEM ファイルを PFX ファイルに変換します。 コマンドラインで次を実行します。

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 一時 PEM ファイルを最終 PEM ファイルに変換します。 コマンドラインで次を実行します。

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. PEM ファイルを使用して、[Adobe Pass TVE Dashboard を介して証明書を ](#management) 設定 [ するか ](https://experience.adobe.com/#/pass/authentication)PEM ファイルをAdobe Pass認証担当者に送信します。

   * [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) を使用して証明書を管理する方法の詳細については、次の節を参照してください。

   * Adobe Pass認証は、プライマリ証明書とバックアップ証明書の両方をサポートしています。 プライマリ証明書が侵害された場合は、その証明書を失効させて、セカンダリ証明書に切り替えることができます。 これにより、顧客への影響を最小限に抑えながら、証明書間をスムーズに移行できます。

## ユーザーメタデータの管理 {#management}

>[!IMPORTANT]
>
> Adobe Pass TVE ダッシュボードにアクセスできない場合は、[Zendesk](https://adobeprimetime.zendesk.com) からチケットを作成し、テクニカルアカウントマネージャー（TAM）に適切な変更を依頼します。

Adobe Pass TVE ダッシュボードは、Adobe Pass認証のお客様（プログラマー）が設定とデータを管理するためのツールです。 このセルフサービスダッシュボードにより、[Adobe Pass TVE ダッシュボードユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md) ドキュメントに記載されている様々な機能が有効になります。

MVPDで使用できるユーザーメタデータ属性を確認および管理するには、[TVE Dashboard User Guide for Integrations](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata) ドキュメントの手順に従います。

ユーザーメタデータ属性の暗号化に使用する証明書を確認および管理するには、プログラマー用の [TVE ダッシュボードユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) またはチャネル用の [TVE ダッシュボードユーザーガイド ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates) のドキュメントの手順に従います。

## REST API V2 {#rest-api-v2}

ユーザーメタデータ属性は、次の API を使用して取得できます。

* [プロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定の mvpd のプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定のコードのプロファイルの取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

ユーザーメタデータ属性の構造については、上記の API の **Response** および **Samples** の節を参照してください。

>[!IMPORTANT]
>
> ユーザーメタデータは認証フローが完了すると使用できるようになります。したがって、[ ユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) 情報は既にプロファイル情報に含まれているため、クライアントアプリケーションで個別のエンドポイントをクエリして取得する必要はありません。

上記の API を統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [セカンダリアプリケーション内で実行される基本プロファイルフロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

MVPDと特定のメタデータ属性に応じて、特定のメタデータ属性を認証フロー中に更新できます。 その結果、最新のユーザーメタデータを取得するには、クライアントアプリケーションが上記の API を再度クエリする必要が生じる場合があります。

>[!MORELIKETHIS]
>
> [ 認証フェーズに関するよくある質問 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
