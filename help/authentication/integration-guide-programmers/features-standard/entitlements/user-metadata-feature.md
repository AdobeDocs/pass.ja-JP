---
title: ユーザーメタデータ機能
description: ユーザーメタデータ機能
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# ユーザーメタデータ {#user-metadata}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>
</br>

## 概要 {#intro}

ユーザ メタデータ機能を使用すると、プログラマは MVPD によって管理されるユーザ固有のデータにアクセスできます。  ユーザーメタデータタイプには、郵便番号、保護者による制限、ユーザー ID などが含まれます。  *ユーザー* メタデータは、以前に使用可能だった *静的* メタデータ（認証トークン TTL、認証トークン TTL およびデバイス ID）の拡張です。


ユーザーメタデータの重要なポイント：

- 認証および承認フロー中にプログラマのアプリケーションに渡される
- 値はトークンに保存されます
- 異なる MVPD が異なる形式でデータを提供する場合は、値を正規化できます
- 一部のパラメーターは、プログラマーのキー（郵便番号など）を使用して暗号化できます。暗号化証明書の生成については、[ 暗号化用のユーザーメタデータ証明書 ](user-metadata-certificate.md) を参照してください
- 設定を変更すると、特定の値がAdobeで使用可能になります

## ユーザーメタデータの取得 {#obtaining}

ユーザーのメタデータは、AccessEnabler の `getMetadata()` 関数およびクライアントレス API の `/usermetadata` エンドポイントを介してプログラマーが利用できます。  `getMetadata()` の使用とそれに対応するコールバック `setMetadataStatus()` （またはクライアントレス API で使用されるエンドポイントとパラメーター）について詳しくは、Platform API ドキュメントを参照してください。

プログラマーは、取得するメタデータのタイプ（`getMetadata(key)`）のキーを指定することで、メタデータを取得します。

メタデータが次のように返されます。`setMetadataStatus(key, encrypted, data)`

| パラメーター | タイプ | 説明 |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | 文字列 | リクエストされたメタデータのタイプを指定します |
| `encrypted` | ブール値 | 「value」が暗号化されているかどうかを示すブール値フラグ。 これが「true」の場合、「value」は実際の値の JSON web 暗号化された表現になります。 |
| `data` | オブジェクト | メタデータの表現を含む JSON オブジェクト |



データパラメーターの構造、値はタイプによって異なります。

| キー | 値タイプ | サンプル | 説明 |
| --- | --- | --- | --- |
| `zip` | JSON 配列 | \[&quot;77754&quot;, &quot;12345&quot;\] | 郵便番号 |
| `householdID` | JSON 文字列 | 「1o7241p」 | 世帯の識別子。 MVPD がサブアカウントをサポートしていない場合、これは `userID` と同じです |
| `maxRating` | JSON オブジェクト | { MPAA: &quot;NR&quot;, <br> VCHIP: &quot;X&quot;, <br> URL: &quot;http://manage.my/parental&quot; } | ユーザーの保護者による制限の上限 |
| `userID` | JSON 文字列 | 「1o7241p」 | ユーザー識別子。 MVPD がサブアカウントをサポートし、ユーザーがメインアカウントでない場合、`userID` は `householdID` とは異なります。 |
| `channelID` | JSON 配列 | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | ユーザーが閲覧できるチャネルのリスト |
| `is_hoh` | JSON 文字列 | &quot;1&quot; | ユーザーが世帯主かどうかを識別するフラグ |
| `encryptedZip` | JSON 文字列 | &quot;&quot; | Comcast は暗号化された郵便番号を公開します |
| `typeID` | JSON 文字列 | プライマリ | ユーザーアカウントがプライマリ/セカンダリ アカウントかどうかを識別するフラグ |
| `primaryOID` | JSON 文字列 | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | 世帯の識別子。 `typeID` がプライマリの場合、には `userID` の値が含まれます |
| hba_status | ブール値 | 「true」「false」 | 特定の統合に対してホームベースの認証が有効になっているかどうかを示すブール値フラグ |
| 許可ミラーリング | ブール値 | 「true」「false」 | このデバイスで画面のミラーリングが許可されているかどうかを示します |

>[!NOTE]
>
> **注意：** データパラメーターが暗号化されている場合（通常は **zip キー** の場合）、メタデータキーの表現は、配列またはオブジェクトではなく、JSON 文字列になります。


**重要：** プログラマーが使用できる実際のユーザーメタデータは、MVPD の機能によって異なります。  実稼動環境で機密性の高いユーザーメタデータ（郵便番号など）を利用できるようにするには、MVPD との法的契約を締結する必要があります。

</br>


| 名前 | 詳細 | 暗号化が必要 | コメント |
| --- | --- | --- | --- |
| ユーザー ID | MVPD によって提供される | 不可 | この値は、Adobeによってハッシュ化され、メディアトークンおよび sendTrackingData （） コールバックで公開されます。<br><br> この場合のハッシュは、過去の理由に基づいて行われました <br><br> この ID は、世帯 ID またはサブアカウント ID の可能性があります。 これは通常、指定せず、その時点で使用されていたログイン（プライマリ ID またはサブアカウント）に関連付けられた ID のみです |
| アップストリームユーザー ID | MVPD によって提供され、同時実行監視フローにのみ使用されます。 | 不可 | この値は、MVPD およびプログラマーサイトとアプリに同時実行制限を適用する際に使用されます。 <br><br>ID には、同時実行監視ポリシーも含めることができます <br><br> ほとんどの MVPD では、この値はユーザー ID と同じです |
| 世帯ユーザー ID | MVPD によって提供され、主にペアレンタルコントロールフローに使用されます | 不可 | 世帯アカウントとサブアカウントの使用状況をプログラマーが理解できる ID。<br><br> 真のレーティングが利用できない場合は、ペアレンタルコントロールの代用として使用されることがあります（ユーザーが家庭内アカウントでログインしている場合、視聴可能であれば、レーティングされたコンテンツは表示されません） <br><br> 家庭内ユーザー ID、世帯主 ID、世帯主フラグなど、MVPD 間にはさまざまな表現があります。 |
| 世帯主 | 現在の口座が世帯主かどうかを示すフラグ | 不可 | 上記を参照 |
| タイプ ID/プライマリ ID | 世帯アカウント識別子 | 不可 | 世帯主に関する AT&amp;T 固有の指標。<br><br> タイプ ID = ユーザーアカウントがプライマリ/セカンダリ アカウントかどうかを識別するフラグ <br><br>プライマリ OID =世帯 ID。 TypeID がプライマリの場合、には userID の値が格納されます |
| 最大評価 | 現在のアカウントで許可される最大評価 | 不可 | プログラマーが、アカウントに適していないコンテンツを除外できるようにします。<br><br>MPAA または VCHIP の評価あり |
| チャネルライン上がり | ユーザーのパッケージで使用可能なチャネルのリスト | 不可 | 複数のネットワークを集約するポータルからさまざまなチャネルをすばやく許可/削除するために使用される </br></br> *プリフライト認証を使用すると、通常、このユースケースをより柔軟に実行できるので、代わりに使用してください*<br><br>OLCA の仕様により、AuthN 応答の AttributeStatement としてこれを使用できます |
| HBA ステータス | HBA 経由で認証が行われたかどうかを示す | 不可 |     |
| 郵便番号 | ユーザーの請求先の郵便番号 | はい | ブロードキャストやスポーツのイベントに使用 <br><br> 高速なアップデート <br><br> 機密データ用の AuthZ 応答を提供することも可能で、MVPD の法的契約が必要 |
| 暗号化された郵便番号 | ユーザーの請求先の郵便番号（Comcast） | はい | 上記と同様ですが、Comcast で暗号化されています |
| 言語 | ユーザー言語設定 | 不可 | ユーザーの環境設定に従ってメッセージを表示するために使用します |
| ミラーリングを許可 | このデバイスで画面のミラーリングが許可されているかどうかを示します | 不可 |     |



## 使用可能なメタデータ {#available_metadata}

次の表に、Adobe Pass認証エコシステムのユーザーメタデータの現在のステータスを示します。


|     | **Legal **<br><br>**Agreement **<br><br>**Signed （zip のみ）** | **AuthN 上&#x200B;**<br><br>**ユーザー ID** | **郵便番号&#x200B;**<br><br>**AuthN/Z** | **評価&#x200B;**<br><br>**AuthN/Z** | **AuthN/Z の Household **<br><br>**ID** | **AuthN のチャネル ID** | **AuthN の世帯主** | **AuthN のタイプ ID** | **AuthN のプライマリ OID** | 言語 | アップストリームユーザー ID **AuthN 上** | HBA ステータス | OnNet | inHome | AuthZ でのミラーリングを許可 | **備考** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **正式名称** | 該当なし | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | language | upstreamUserID | hba_status | onNet | inHome | 許可ミラーリング | 1. AuthN の場合 – OiosamlMetadataParser は、すべてのパーサーでこの新しい属性が有効になるように変更する必要があります <br>2。  AuthZ の場合 – 認証実装は MVPD 固有なので、一般的な方法はありません |
| **暗号化が必要** | 該当なし | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| **機密** | 該当なし | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |     |
| **AdobeIdP** | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **はい** | **はい** | **はい** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約は必要ありません。有効にすることができます。 |
| **シナコール** | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **プロキシされた MVPD のすべてをカバーしていない法的合意。**   <br> これは Synacor の一般的なサポートで、すべての MVPD にロールアップされていない可能性があります。 |
| 皿 | **いいえ** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | すべての Synacor MVPD と同じリストに upstreamUserID を追加して共有します。 |
| 堆肥 | **いいえ** | **はい** | **いいえ** | **はい（AuthZ のみ）** | **はい（AuthZ のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** |     |
| **AT&amp;T** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| **有線テレビジョン** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| **HTC** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| **プロキシマシロン** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| **プロキシ Clearleap** | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthZ のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約書が署名されました。 |
| ロジャース | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| RCN | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| 憲章 | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| ベライゾン | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** |     |
| Eastlink | **いいえ** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| プロキシ GLDS | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| DTV | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| コックス | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| コジコ | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| Videotron | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい*** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | userID と同じ値の householdID を公開します |
| スペクトル | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **はい** |     |
| **その他すべて&#x200B;**<br><br>**MVPD** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **法的契約はまだありません。機密メタデータは実稼動環境では使用できません。** <br> すべての MVPD の場合、ユーザー ID は追加作業なしで使用できます。 |


新しいタイプが利用可能になり、Adobe Pass認証システムに追加されると、ユーザーメタデータタイプのリストが拡張されます。

## コードサンプル {#code_samples}

- [コードのサンプル 1](#code_sample1)
- [コードサンプル 2 （モック getMetadata）](#code_sample2)


### コードのサンプル 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### コードサンプル 2 （モック getMetadata） {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
