---
title: Adobe シングルサインオンサービス
description: 複数のデバイスやアプリケーションをまたいでシームレスな認証を可能にするAdobe Pass SSO サービスについて説明します。
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Adobe シングルサインオンサービス {#sso-service}

このドキュメントでは、Adobe シングルサインオンサービスのユースケース、エンドポイントおよび API について説明します。

**最新リビジョン - 1.0.0**

## 範囲 {#scope}

Adobe Pass SSO サービスは、複数のデバイスやアプリケーション間でシームレスな認証を可能にし、セキュリティとコンプライアンスの標準を維持しながら、統一されたユーザーエクスペリエンスを提供します。 このサービスは、今日のマルチデバイスストリーミングの状況において、クロスプラットフォーム認証に対する高まるニーズに対応しています。

## 概要 {#overview}

### 概要 {#what-it-is}

ユーザーが 1 回認証を行い、情報に基づいた方法で他のデバイスへの認証の転送を管理できる、包括的なシングルサインオンソリューション

### 認証の現在の課題 {#current-challenges}

* ユーザーは、購読を持つすべてのストリーミングサービスで認証する必要があります
* ユーザーは、デバイスまたはアプリケーションごとに個別に認証する必要があります
* 一部のプラットフォームでは、認証フローでパスワードを入力することが困難なため、放棄の割合が増える場合があります

### Adobe Pass SSO サービスの機会 {#opportunity}

* 独自の認証を必要とするストリーミングサービスの数の増加
* シームレスなクロスデバイスエクスペリエンスに対する需要の増加
* 世帯あたりのストリーミングデバイス数の増加
* 世帯ごとに統合された認証が必要

### ビジネス上のメリット {#business-benefits}

#### コンテンツプロバイダー {#content-providers}

* **ユーザーエンゲージメントの向上** - シームレスなエクスペリエンスにより、セッション時間が長くなります
* **摩擦の軽減** – 認証の障壁を低くするとコンテンツ消費が増加
* **リテンションの向上** - ユーザーエクスペリエンスの向上によりチャーンが減ります
* **コストの削減** – 認証の問題に関するサポートチケットが少なくなる

#### エンドユーザー {#end-users}

* **シームレスなエクスペリエンス** – 一度認証すれば、どこからでもアクセス可能
* **時間の節約** - ログインプロセスが繰り返されない
* **デバイスの柔軟性** - デバイス間を中断することなく切り替える
* **一貫したエクスペリエンス** – すべてのプラットフォームで統一された認証

#### IdPs （MVPD、Telcos など） {#idps}

* MVPD は、認証済み SSO を使用して追加のデバイスを通知できます
* **ユーザー満足度の向上** – 認証体験の向上
* **サポート負荷の軽減** – 認証関連のサポートコールを削減
* **競争上の優位性** – 競合他社よりも優れたユーザーエクスペリエンス

## ユースケース {#use-cases}

### ID マッピング {#identity-mapping}

このサービスは、D2C と TVE アカウントを同じアプリ内でリンクする機能を構築します。

より多くのストリーミングサービスが、サードパーティ（MVPD/バーチャルMVPD/電話会社など）に販売されるバンドルを構築しています。 ユーザーは、同じアプリで複数のアカウントを処理する必要が生じる場合があります。 シームレスな認証エクスペリエンスを作成するには、これらのアカウントをブリッジして、ログインエクスペリエンスを簡素化する必要があります。

ユーザーは D2C アカウントで認証し、別のアカウントで ONCE 認証します（例： MVPD）。 当社のサービスでは、これらのアカウントがリンクされます。つまり、今後、異なるデバイスでの後続の認証は D2C アカウントでのみ行うことができます。

### クロスデバイス SSO {#cross-device-sso}

テレビに接続されたデバイスでの認証は、携帯電話での認証よりも面倒な場合があります。 良いユーザーエクスペリエンスは、電話で認証し、その認証をスマートテレビに渡すことです。

## 主要コンポーネント {#key-components}

* **サービストークン API** - シングルサインオンメカニズムを安全に管理するキーコンポーネント
* **List API** - アプリケーションは、ユーザーがエコシステム内のデバイスのリストを理解するのに役立ちます
* **Link API** - アプリケーションを使用すると、ユーザーがエコシステムにデバイスを追加できます
* **Unlink API** - アプリケーションを使用すると、ユーザーがエコシステムでデバイスを削除できます

## 詳細なユースケース {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

このユースケースでは、1 つのデバイス上の D2C と TVE （MVPD）資格情報の間でリンクを設定し、そのリンクされたプロファイルを同じアプリケーション内の他のデバイスで活用できます。

![D2C-TVE SSO フロー &#x200B;](../../../assets/sso_service_d2c_1.png)

![&#x200B; クロスデバイス SSO フロー &#x200B;](../../../assets/sso_service_d2c_2.png)

### クロスデバイス SSO {#cross-device-sso-detailed}

このユースケースを使用すると、あるデバイスで既に認証されたユーザーが、他のデバイスにインストールされている同じ D2C または TVE アプリケーション（認証および承認用にAdobe Pass REST V2 を実装するために必要なアプリケーション）で認証済みプロファイルを再利用できます。

## D2C-TVE アプリケーションにAdobe SSO サービスを統合する方法 {#integration}

### 手順 1 – 共通の識別子の取得 {#step-1}

Adobe SSO サービスを統合するには、アプリケーション実装で X-SSO-ID の共通の識別情報 SSO 属性として使用する一意で永続的な ID を確立する必要があります。 これは、D2C サービスでユーザーを認証することで取得でき、この認証に関する属性を保持します。

### 手順 2 - サービストークンの取得 {#step-2}

POST /serviceToken エンドポイントの X-SSO-ID で共通の識別子を使用すると、次のペイロードを持つ署名済み JWT が取得されます。

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

サービストークンには、サービストークンが有効な間隔を示す「iat」（発行日）と「exp」（有効期限）があります。 有効期限が切れた場合、期限切れのサービストークンと共にGET /serviceToken エンドポイントを使用して、新しい JWT を取得できます。

### 手順 3 - Adobe Pass REST API V2 を使用した TVE MVPDでの認証 {#step-3}

Adobe Passの認証は、サービストークン [REST API V2 - シングルサインオンサービストークンフロー &#x200B;](https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows) を使用して実装する必要があります。

### 手順 4 – 別のデバイスをリンクする {#step-4}

/link API を使用すると、認証済みアプリケーションから、別のデバイスで使用するリンクを作成できます

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

「コード」は、ユーザーがセカンダリデバイス上の未認証のアプリケーションで発生させる 6 桁のシーケンスの短いリンクコードです

### 手順 5 - シングルサインオン認証の取得 {#step-5}

別のデバイスでは、ユーザーがコードを導入すると、アプリケーションは次の操作を実行できます。

* d2C サービスからの ID の取得
* rest V2 API を使用したAdobe PassからのMVPD プロファイルの取得

MVPD プロファイルは、最初の認証が取得された期間中、SSO を通じて有効になります。

MVPDがプロファイルを無効にした場合、またはユーザーがMVPDからログアウトした場合、Adobe Pass REST API V2 を使用するアプリケーションにはプロファイルレコードがなくなり、ユーザーはMVPDでもう一度認証する必要があります。

### 手順 6 – 他のデバイスでのシングルサインオンの管理 {#step-6}

アプリケーションは、/list API を使用して、同じ共通識別子でリンクされている他のすべてのデバイスに関する情報を取得できます。

/unlink API を使用してデバイスのリンクを解除することで、デバイスをいつでもシングル サインオンから削除できます。

## API {#apis}

### サービストークン API {#service-token-api}

#### 説明 {#service-token-description}

サービストークン API を使用すると、複数のアプリケーションまたはデバイス間でのシングルサインオン（SSO）機能を有効にするサービストークンをリクエストおよび管理できます。 これらのサービストークンは、認証済みプロファイル（SSO プロファイル）を識別し、SSO 接続を確立および維持するために不可欠です。

>[!WARNING]
>
>サービストークンには、機密性の高い認証情報が含まれています。 アプリケーションは、これらのトークンを安全に処理し、信頼できない関係者に公開しないようにする必要があります。

サービストークン API には、次の 2 つの主要なエンドポイントが用意されています。

* **POST /api/{serviceProvider}/serviceToken** – 新しく作成された JWS サービストークンを取得します
* **GET /api/{serviceProvider}/serviceToken** – 既存の JWS サービストークンを更新します

Adobe Pass Authentication Services のエラーが原因でサービストークン API リクエストに対応できなかった場合、API 応答の一部に追加のエラー情報が含まれます。

#### POST - serviceToken {#post-service-token}

##### リクエスト {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>トークンがリクエストされているサービスプロバイダーの識別子。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>
         デバイス識別子ペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> ヘッダードキュメントを参照してください。
         <br/><br/>
         この識別子は、X-SSO-ID が指定されていない場合のデフォルトの SSO 識別子として使用されます。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         <a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a> ヘッダーのドキュメントで指定されているデバイス情報。
         <br/><br/>
         <b> 強くお勧めします </b> アプリケーションのデバイスプラットフォームで有効な値を明示的に指定できる場合に使用します。
         <br/><br/>
         Adobe Pass認証バックエンドは、明示的に設定された値を、暗黙的に抽出された値と結合します。 指定しない場合、デフォルトの抽出値が使用されます。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         このリクエストを既存の認証済みプロファイルに関連付けるリンクコード。 指定された場合、応答にはリンクコードを生成したプロファイルを含む SSO 用のサービストークンが含まれます。
         <br/><br/>
         これは、通常、セカンダリアプリケーションまたはデバイスが、プライマリアプリケーションまたはデバイスから認証済みプロファイルに接続する場合に使用されます。
      </td>
      <td>x-sso-id が指定されていない場合は必須</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         アプリケーションが SSO のベースとしてリクエストする共通の識別子。
         <br/><br/>
         指定された場合、この識別子は、デバイスやアプリケーションをまたいで共通の SSO プロファイルを確立するために使用されます。
      </td>
      <td>x-sso-link が指定されていない場合は必須</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承諾</td>
      <td>
         クライアントアプリケーションによって受け入れられるメディアタイプ。
         <br/><br/>
         指定する場合は、application/json にする必要があります。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>optional</td>
   </tr>
</table>

##### 応答 {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>201</td>
      <td>作成日時</td>
      <td>
        サービストークンが正常に生成され、応答本文に返されました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバー側で問題が発生しました。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
</table>

###### 成功 {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>201</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>HTTP ステータス（「作成済み」など）</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>サービストークンを含む、Base64 でエンコードされた JSON web 署名（JWS）。 このトークンは、後続の API 呼び出しで使用して、認証済みプロファイルを識別し、SSO 機能を有効にすることができます。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>エポックミリ秒、またはエラー時に 0</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>エポックミリ秒、またはエラー時に 0</td>
      <td><i>必須</i></td>
   </tr>
</table>

###### エラー {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、401、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples-post-service-token}

### 1.新しいサービストークンをリクエストする（SSO ID を含む）

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### 2.新しいサービストークンをリクエストする（SSO リンクを使用）

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### リクエスト {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>トークンがリクエストされているサービスプロバイダーの識別子。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         以前に取得した、更新が必要なサービストークン。
         <br/><br/>
         更新を行うには、このトークンが有効であるか、最近有効期限が切れている必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承諾</td>
      <td>
         クライアントアプリケーションによって受け入れられるメディアタイプ。
         <br/><br/>
         指定する場合は、application/json にする必要があります。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>optional</td>
   </tr>
</table>

##### 応答 {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        サービストークンが正常に更新され、応答本文に返されました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンまたはサービストークンが無効です。クライアントは新しいアクセストークンまたはサービストークンを取得して、もう一度試してください。 詳しくは、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバー側で問題が発生しました。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
</table>

###### 成功 {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>200</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>HTTP ステータス（例：「OK」）</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>更新されたサービストークンを含む、Base64 でエンコードされた JSON web 署名（JWS）。 このトークンは、後続の API 呼び出しで使用して、認証済みプロファイルを識別し、SSO 機能を有効にすることができます。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>エポックミリ秒、またはエラー時に 0</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>エポックミリ秒、またはエラー時に 0</td>
      <td><i>必須</i></td>
   </tr>
</table>

###### エラー {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、401、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples-get-service-token}

### &#x200B;1. サービストークンを更新するリクエスト

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### リンク API {#link-api}

#### 説明 {#link-description}

Link API を使用すると、複数のアプリケーションまたはデバイス間でのシングルサインオン（SSO）を有効にできるリンクコード（または QR コード）をリクエストできます。 このリンクコードを使用すると、ユーザーは既存の認証済みプロファイル（SSO プロファイル）に新しいアプリケーションまたはデバイスを接続し、アプリケーションやデバイス間でシームレスな SSO エクスペリエンスを提供できます。

Link API では、AD-Service-Token ヘッダーに有効なサービストークンを指定する必要があります。

生成されたリンクコードは、通常、プライマリアプリケーションまたはデバイスのユーザーに表示され、SSO 接続を確立するためにセカンダリアプリケーションまたはデバイスに入力されます。 リンクコードの有効期間は限られており（通常 5～30 分）、1 回限りの使用を意図しています。

Adobe Pass Authentication Services エラーが原因でリンク API リクエストにサービスを提供できなかった場合は、その他のエラー情報がリンク API 応答の結果に含まれます。

#### 投稿 – リンク {#post-link}

##### リクエスト {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>トークンがリクエストされているサービスプロバイダーの識別子。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>デバイス識別子ペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> ヘッダードキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         サービストークンの生成については、サービストークン API ドキュメントを参照してください。
         <br/><br/>
         このサービストークンは、リンクコードが生成される認証済みプロファイルを識別します。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承諾</td>
      <td>
         クライアントアプリケーションによって受け入れられるメディアタイプ。
         <br/><br/>
         指定する場合は、application/json にする必要があります。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>optional</td>
   </tr>
</table>

##### 応答 {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>201</td>
      <td>作成日時</td>
      <td>
        リンクコードが正常に生成され、応答本文に返されました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバー側で問題が発生しました。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
</table>

###### 成功 {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>201</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>HTTP ステータス（「作成済み」など）</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">リンク</td>
      <td>SSO 接続を確立するためにセカンダリ・アプリケーションまたはデバイスで使用できる短い数字または英数字のコード。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>リンクコードが有効になる際のタイムスタンプ （エポックからのミリ秒）。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>リンクコードの有効期限が切れる際のタイムスタンプ（エポックからのミリ秒）。</td>
      <td><i>必須</i></td>
   </tr>
</table>

###### エラー {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、401、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples-post-link}

### 1.既存の認証済みプロファイルのリンクコードをリクエストする

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Unlink API {#unlink-api}

#### 説明 {#unlink-description}

Unlink API を使用すると、認証済みプロファイル（SSO プロファイル）から 1 つまたは複数のデバイスの削除をリクエストできます。 この API を使用すると、ユーザーは SSO 設定からデバイスを切断し、認証済みプロファイルにアクセスできるデバイスを制御できます。

>[!WARNING]
>
>Unlink API では、AD-Service-Token ヘッダーに有効なサービストークンを指定する必要があります。

Adobe Pass Authentication Services エラーが原因で Unlink API リクエストに対応できなかった場合は、その他のエラー情報が Unlink API レスポンスの結果に含まれます。

#### 投稿 – リンク解除 {#post-unlink}

##### リクエスト {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>サービスプロバイダーの識別子。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文パラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">デバイス</td>
      <td>
         リンク解除するデバイス識別子の配列。
         <br/><br/>
         例：<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         送信するリソースに使用できるメディアタイプ。
         <br/><br/>
         application/json にする必要があります。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>デバイス識別子ペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> ヘッダードキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         サービストークンの生成については、サービストークン API ドキュメントを参照してください。
         <br/><br/>
         このサービストークンは、デバイスのリンクが解除される認証済みプロファイルを識別します。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承諾</td>
      <td>
         クライアントアプリケーションによって受け入れられるメディアタイプ。
         <br/><br/>
         指定する場合は、application/json にする必要があります。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>optional</td>
   </tr>
</table>

##### 応答 {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        要求されたデバイスは、SSO 設定から正常にリンク解除されました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>許可されていないメソッド</td>
      <td>
        HTTP メソッドが無効です。クライアントは、リクエストされたリソースに許可されている HTTP メソッドを使用し、再試行する必要があります。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバー側で問題が発生しました。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
</table>

###### 成功 {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>200</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>操作結果の情報：「OK」</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         正常にリンク解除されたデバイスのリスト。
         <br/><br/>
         例：<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>必須</i></td>
   </tr>
</table>

###### エラー {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、401、405、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples-post-unlink}

### &#x200B;1. SSO プロファイルからデバイスのリンクを解除するリクエスト

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### 2.部分的に成功したデバイスのリンク解除リクエスト

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### リスト API {#list-api}

#### 説明 {#list-description}

List API は、認証済みプロファイル（SSO プロファイル）に現在接続されているデバイスのリストを取得するために使用できます。 この API を使用すると、ユーザーとアプリケーションは、SSO 設定の一部であるデバイスを表示し、複数のデバイスをまたいで認証済みエクスペリエンスの表示と管理を行うことができます。

>[!WARNING]
>
>List API では、AD-Service-Token ヘッダーで有効なサービストークンを指定する必要があります。

List API は、ユーザーが自分のデバイスを認識するのに役立つデバイス識別子とメタデータなど、認証済みプロファイル（SSO プロファイル）内の各デバイスに関する詳細を返します。 この情報は、ユーザーが SSO 設定に残すデバイスに関して十分な情報に基づいた決定を下すために使用できます。

Adobe Pass Authentication Services のエラーが原因で List API リクエストに対応できなかった場合は、List API 応答の結果にエラー情報が含まれます。

#### GET - リスト {#get-list}

##### リクエスト {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">パス</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">メソッド</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">パスパラメーター</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>サービスプロバイダーの識別子。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">認証</td>
      <td>ベアラートークンペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Authorization</a> ヘッダーのドキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>デバイス識別子ペイロードの生成については、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> ヘッダードキュメントを参照してください。</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         サービストークンの生成については、サービストークン API ドキュメントを参照してください。
         <br/><br/>
         このサービストークンは、デバイスリストを取得する認証済みプロファイルを識別します。
      </td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">承諾</td>
      <td>
         クライアントアプリケーションによって受け入れられるメディアタイプ。
         <br/><br/>
         指定する場合は、application/json にする必要があります。
      </td>
      <td>optional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>クライアントアプリケーションのユーザーエージェント。</td>
      <td>optional</td>
   </tr>
</table>

##### 応答 {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">コード</th>
      <th style="background-color: #EFF2F7;">テキスト</th>
      <th style="background-color: #EFF2F7;">説明</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        SSO 設定内のデバイスのリストは正常に取得され、応答本文に返されました。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>リクエストが正しくありません</td>
      <td>
        リクエストが無効です。クライアントはリクエストを修正して再試行する必要があります。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未認証</td>
      <td>
        アクセストークンが無効です。クライアントは新しいアクセストークンを取得して、再試行する必要があります。 詳しくは、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview"> 動的クライアント登録の概要 </a> ドキュメントを参照してください。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>許可されていないメソッド</td>
      <td>
        HTTP メソッドが無効です。クライアントは、リクエストされたリソースに許可されている HTTP メソッドを使用し、再試行する必要があります。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部サーバーエラー</td>
      <td>
        サーバー側で問題が発生しました。 応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従ったエラー情報が含まれている場合があります。
      </td>
   </tr>
</table>

###### 成功 {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>200</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">デバイス</td>
      <td>
         キーと値のペアのマップを含む JSON。
         <br/><br/>
         <b>Key:</b> deviceId - <a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a> ヘッダードキュメントに記載されているデバイス識別子ペイロード
         <br/><br/>
         <b>Value:</b> attributes - デバイスメタデータ属性のマップを含む JSON。以下が含まれます。
         <ul>
            <li>デバイスタイプ</li>
            <li>platform</li>
            <li>ユーザーエージェント</li>
            <li>デバイスの識別に役立つその他の関連メタデータ</li>
         </ul>
         属性の値は単純な値（文字列、整数、ブール値など）にすることができます
      </td>
      <td><i>必須</i></td>
   </tr>
</table>

###### エラー {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">ヘッダー</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ステータス</td>
      <td>400、401、405、500</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必須</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">本文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>応答本文には、<a href="https://experienceleague.adobe.com/ja/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes"> 拡張エラーコード </a> ドキュメントに従った追加のエラー情報が提供される場合があります。</td>
      <td><i>必須</i></td>
   </tr>
</table>

## サンプル {#samples-get-list}

### &#x200B;1. SSO プロファイル内のデバイスの一覧表示を要求する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. デバイスがリンクされていないデバイスの一覧表示を要求する

>[!BEGINTABS]

>[!TAB  リクエスト ]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB  応答 ]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## エラーコード {#error-codes}

### エラー応答構造 {#error-structure}

すべてのエラー応答には、次のフィールドが含まれます。

| フィールド | タイプ | 説明 |
|:---|:---|:---|
| ステータス | 整数 | HTTP ステータスコード（400、401、500 など） |
| コード | 文字列 | 機械読み取り可能なエラーコード |
| メッセージ | 文字列 | 人間が読み取れる説明 |
| アクション | 文字列 | 推奨されるクライアント側のアクション |
| helpUrl | 文字列 | ドキュメントリンク |
| trace | 文字列 | 関連付けの一意のリクエスト ID （UUID） |

**エラー応答の例**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=ja",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**成功応答の例**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>成功応答にはエラーフィールドは含まれません。 エラー応答には、成功フィールドは含まれません。

### エラーコードカタログ {#error-catalog}

#### 一般的なエラー

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| token_invalid | 400 | 指定したトークンは無効です | get_new_token |
| token_expired | 401 | トークンの有効期限が切れています | get_new_token |
| header_missing | 400 | 必須ヘッダーがありません | check_heads |
| 未認証 | 401 | 未認証のアクセス | なし |
| internal_error | 500 | 内部エラーが発生しました | なし |

#### 検証エラー

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| request_null | 400 | リクエストオブジェクトを null にすることはできません | なし |

#### POST ServiceToken エラー

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| header_missing | 400 | POST リクエストには、x-sso-id または x-sso-link ヘッダーのいずれかが必要です | check_heads |
| header_missing | 400 | POST リクエストには AP デバイス識別子ヘッダーが必要です | check_heads |

#### GET ServiceToken エラー

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| header_missing | 400 | GET リクエストには AD-Service-Token ヘッダーが必要です | check_heads |
| header_invalid | 401 | AD-Service-Token の JWT 署名が無効です | get_new_token |
| header_invalid | 401 | JWT 署名の検証エラー | get_new_token |
| header_invalid | 401 | AD-Service-Token に JWT 件名（サブ）がないか、空です | get_new_token |
| header_invalid | 401 | JWT 件名の抽出中にエラーが発生しました | get_new_token |

#### リンク検証エラー

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| header_missing | 401 | リンクリクエストには AD-Service-Token ヘッダーが必要です | check_heads |
| header_invalid | 401 | AD-Service-Token の JWT 署名が無効です | get_new_token |
| header_invalid | 401 | JWT 署名の検証エラー | get_new_token |

#### 検証エラーのリンクを解除

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| header_missing | 401 | リンク解除リクエストには AD-Service-Token ヘッダーが必要です | check_heads |
| header_invalid | 401 | AD-Service-Token の JWT 署名が無効です | get_new_token |
| header_invalid | 401 | JWT 署名の検証エラー | get_new_token |
| header_invalid | 401 | AD-Service-Token に JWT 件名（サブ）がないか、空です | get_new_token |
| request_invalid | 400 | デバイスの一覧を Null または空にすることはできません | check_request_body |

#### リスト検証エラー

| コード | ステータス | メッセージ | アクション |
|:---|:---|:---|:---|
| header_missing | 401 | リストリクエストには AD-Service-Token ヘッダーが必要です | check_heads |
| header_invalid | 401 | AD-Service-Token の JWT 署名が無効です | get_new_token |
| header_invalid | 401 | JWT 署名の検証エラー | get_new_token |
| header_invalid | 401 | AD-Service-Token に JWT 件名（サブ）がないか、空です | get_new_token |
| header_invalid | 401 | JWT 件名の抽出中にエラーが発生しました | get_new_token |

