---
title: ユーザメタデータ機能
description: ユーザメタデータ機能
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# ユーザーメタデータ {#user-metadata}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>
</br>

## はじめに {#intro}

ユーザーメタデータ機能を使用すると、プログラマーは、MVPD によって管理される様々な種類のユーザー固有のデータにアクセスできます。  ユーザーメタデータタイプには、郵便番号、親の規制、ユーザー ID などが含まれます。  *ユーザー* メタデータは、以前に使用可能だった *静的* メタデータ（認証トークン TTL、認証トークン TTL、デバイス ID）。


ユーザーメタデータのキーポイント：

- 認証および承認フロー中にプログラマーのアプリケーションに渡される
- 値はトークンに保存されます
- 異なる MVPD が異なる形式のデータを提供する場合、値を正規化できます
- 一部のパラメーターは、プログラマーのキー（郵便番号など）を使用して暗号化できます
- 特定の値は、設定の変更を通じてAdobeが使用できるようになります。

## ユーザーメタデータの取得 {#obtaining}

ユーザーメタデータは、AccessEnabler を通じてプログラマーが使用できます `getMetadata()` 関数と `/usermetadata` エンドポイント（Clientless API 内）  の使用について詳しくは、プラットフォーム API のドキュメントを参照してください。 `getMetadata()` と、対応するコールバック `setMetadataStatus()` （または、クライアントレス API で使用されるエンドポイントとパラメーター）。

プログラマーは、取得するメタデータのタイプのキーを指定することで、メタデータを取得します。 `getMetadata(key)`.

メタデータは次のように返されます。 `setMetadataStatus(key, encrypted, data)`:

| パラメーター | タイプ | 説明 |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | 文字列 | 要求されるメタデータのタイプを指定します |
| `encrypted` | Boolean | 「値」が暗号化されているかどうかを示す boolean フラグ。 これが「true」の場合、「value」は、実際には実際の値の JSON Web 暗号化された表現になります。 |
| `data` | オブジェクト | メタデータの表現を格納する JSON オブジェクト |



データパラメーターの構造と値は、タイプによって異なります。

| キー | 値のタイプ | サンプル | 説明 |
| --- | --- | --- | --- |
| `zip` | JSON 配列 | \[&quot;77754&quot;, &quot;12345&quot;\] | 郵便番号 |
| `householdID` | JSON 文字列 | &quot;1o7241p&quot; | 世帯識別子。 MVPD がサブアカウントをサポートしない場合、これは `userID` |
| `maxRating` | JSON オブジェクト | { MPAA: &quot;NR&quot;, <br>  VCHIP: &quot;X&quot;,  <br>  URL: &quot;http://manage.my/parental&quot; } | ユーザーの親の最大評価 |
| `userID` | JSON 文字列 | &quot;1o7241p&quot; | ユーザー識別子。 MVPD がサブアカウントをサポートし、ユーザーがメインアカウントではない場合、 `userID` 次とは異なる `householdID`. |
| `channelID` | JSON 配列 | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | ユーザーが表示する権限を持つチャネルのリスト |
| `is_hoh` | JSON 文字列 | &quot;1&quot; | ユーザーが世帯主かどうかを識別するフラグ |
| `encryptedZip` | JSON 文字列 | &quot;&quot; | Comcast は暗号化された郵便番号を公開します |
| `typeID` | JSON 文字列 | プライマリ | ユーザーアカウントがプライマリ/セカンダリアカウントであるかどうかを示すフラグ |
| `primaryOID` | JSON 文字列 | &quot;uuid1e19ec9-012c-124f-b520-acaf118d16a0&quot; | 世帯識別子。 次の場合 `typeID` がプライマリの場合、に `userID` |
| hba_status | Boolean | &quot;true&quot; &quot;false&quot; | 特定の統合に対してホームベース認証が有効になっているかどうかを示すブール型フラグ |
| allowMirroring | Boolean | &quot;true&quot; &quot;false&quot; | このデバイスで画面のミラーリングを許可するかどうかを示します |

>[!NOTE]
>
> **注意：** データパラメーターが暗号化されている場合、通常は **zip キー**&#x200B;の場合、メタデータキーの表現は、配列やオブジェクトの代わりに JSON 文字列になります。


**重要：** プログラマーが利用できる実際のユーザメタデータは、MVPD が利用できるものによって異なります。  実稼働環境で機密性の高いユーザーメタデータ（zipcode など）を利用するには、事前に MVPDs との間で法的契約に署名する必要があります。

</br>


| 名前 | 詳細 | 暗号化が必要 | コメント |
| --- | --- | --- | --- |
| ユーザー ID | MVPD によって提供される | いいえ | この値は、Adobeによってハッシュ化され、メディアトークンと sendTrackingData() コールバックで公開される値です。<br><br>この場合のハッシュ化は、歴史的な理由で行われました<br><br>この ID は、世帯 ID またはサブアカウント ID にすることができます。 これは通常、指定されていないので、その時点で使用されていたログインに関連付けられた ID のみです（プライマリアカウントまたはサブアカウントを指定できます）。 |
| アップストリームユーザー ID | MVPD によって提供され、同時実行監視フローにのみ使用されます。 | いいえ | この値は、MVPD、プログラマーのサイトおよびアプリに対して同時実行制限を適用する場合に使用されます。 <br><br>この ID には、同時実行監視ポリシーも含めることができます<br><br>ほとんどの MVPD では、この値はユーザー ID と等しくなります。 |
| 世帯ユーザー ID | 主に親の制御フローに使用される MVPD によって提供されます | いいえ | プログラマーが世帯とサブアカウントの使用状況を把握できるようにする ID。<br><br>真の評価を利用できない場合は、親の制限の代わりに使用される場合があります（ユーザーが閲覧可能な世帯アカウントでログインした場合、評価されたコンテンツは表示されません）。<br><br>この表現方法に関して、MVPD 間で多くのバリエーションがあります。世帯ユーザー ID、世帯 ID の長、世帯フラグの長などです。 |
| 世帯主 | 現在のアカウントが世帯主であるかどうかを示すフラグ | いいえ | 上記を参照 |
| タイプ ID /プライマリID | 世帯アカウント識別子 | いいえ | 世帯主向けの AT&amp;T 固有の指標。<br><br>タイプ ID =ユーザーアカウントがプライマリ/セカンダリアカウントであるかどうかを示すフラグ<br><br>プライマリOID =世帯識別子。 TypeID がプライマリの場合、には userID の値が含まれます |
| 最大評価 | 現在のアカウントで許可されている最大評価 | いいえ | プログラマーがアカウントに適さないコンテンツを除外できるようにします。<br><br>MPAA または VCHIP の評価を持つ |
| チャネルラインアップ | ユーザーのパッケージで使用可能なチャネルのリスト | いいえ | 複数のネットワークを集約するポータルから、様々なチャネルを迅速に許可/削除するために使用します。</br></br> *プリフライト認証を使用すると、通常はより柔軟にこの使用が可能になるので、代わりにを使用する必要があります* <br><br>OLCA の仕様では、AuthN 応答で AttributeStatement としてこれを使用できます |
| HBA ステータス | HBA 経由で認証が行われたかどうかを示す | いいえ |     |
| 郵便番号 | ユーザーの請求郵便番号 | はい | 放送またはスポーツのイベントに使用される<br><br>AuthZ 応答も提供され、高速更新が可能です。<br><br>機密データ、MVPD の法的契約が必要 |
| 暗号化された郵便番号 | ユーザーの請求郵便番号 (Comcast) | はい | Comcast で暗号化されている |
| 言語 | ユーザーの言語設定 | いいえ | ユーザーの環境設定に従ってメッセージを表示するために使用します |
| ミラーリングを許可 | このデバイスで画面のミラーリングを許可するかどうかを示します | いいえ |     |



## 使用可能なメタデータ {#available_metadata}

次の表に、Adobe Pass Authentication エコシステムのユーザーメタデータの現在の状態を示します。


|     | **法的事項&#x200B;**<br><br>**契約&#x200B;**<br><br>**署名済み（zip のみ）** | **ユーザー ID **<br><br>**AuthN で** | **Zipcode **<br><br>**AuthN/Z で** | **評価&#x200B;**<br><br>**AuthN/Z で** | **Household **<br><br>**AuthN/Z の ID** | **AuthN のチャネル ID** | **AuthN の世帯主** | **AuthN でのタイプ ID** | **AuthN のプライマリOID** | 言語 | アップストリームユーザー ID **AuthN で** | HBA ステータス | OnNet | inHome | AuthZ でのミラーリングを許可 | **メモ** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **正式名** | 該当なし | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | 言語 | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. AuthN の場合 — すべてのパーサーがこの新しい属性を有効にできるように、OiosamlMetadataParser を変更する必要があります。 <br>2.  AuthZ の場合 — 認証実装は MVPD 固有なので、汎用的な方法はありません。 |
| **暗号化が必要です** | 該当なし | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| **機密** | 該当なし | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |     |
| **AdobeIdP** | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **はい** | **はい** | **はい** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 法的契約は不要で、有効にできます。 |
| **Synacor** | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **すべてのプロキシ MVPD を対象としない法的契約。**   <br>これは Synacor の一般的なサポートです。すべての MVPD にロールアップされていない可能性があります。 |
| 皿 | **いいえ** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | すべての Synacor MVPDs と同じリストを共有し、upstreamUserID を共有します。 |
| Comcast | **いいえ** | **はい** | **いいえ** | **はい（AuthZ のみ）** | **はい（AuthZ のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** |     |
| **AT&amp;T** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 契約に署名しました。 |
| **Cablevision** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 契約に署名しました。 |
| **HTC** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| **Proxy Massilon** | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 契約に署名しました。 |
| **プロキシ Clearleap** | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthZ のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | 契約に署名しました。 |
| ロジャース | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| RCN | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| 憲章 | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| Verizon | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **いいえ** |     |
| Eastlink | **いいえ** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| プロキシ GLDS | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| DTV | **はい** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| COX | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| コゲコ | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** |     |
| Videotron | **いいえ** | **はい** | **はい（AuthN のみ）** | **いいえ** | **はい*** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | userID と同じ値を持つ householdID を公開します |
| スペクトル | **はい** | **はい** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **はい（AuthN のみ）** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **はい** | **いいえ** | **いいえ** | **はい** |     |
| **その他すべて&#x200B;**<br><br>**MVPDs** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **はい** | **いいえ** | **いいえ** | **いいえ** | **いいえ** | **法的契約はまだありません。機密メタデータは実稼動環境では使用できません。**  <br>すべての MVPD に対して、追加作業を行わずにユーザー ID を使用できます。 |


新しいタイプが使用可能になり、Adobe Pass Authentication システムに追加されると、ユーザーメタデータタイプのリストが展開されます。

## コードサンプル {#code_samples}

- [コードサンプル 1](#code_sample1)
- [コードサンプル 2 （モック getMetadata）](#code_sample2)


### コードサンプル 1 {#code_sample1}

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
