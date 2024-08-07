---
title: Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット
description: Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

## コンテキスト

オプションですが、クライアントレス API のパラメーター `deviceType` が存在する場合は、[ 使用権限サービスのモニタリング ](/help/authentication/entitlement-service-monitoring-overview.md) で公開されるAdobe Pass認証指標で使用されます。

Adobe Pass認証指標の `deviceType` パラメーターとその **メリット** との関連は最初は述べていなかったことを考慮すると、このテクニカルノートの範囲は、それらに関する詳細を追加することです。

## 説明

`deviceType` パラメーターは、最初のバージョン以降 Clientless API に存在していましたが、Adobe Pass認証指標への影響が新しいリリースで追加されました。



>[!IMPORTANT]
>
>パラメーター `deviceType` が正しく設定されている場合、権利付与サービスのモニタリングに次の **特典** が含まれます。クライアントレスを使用する際に、[ デバイスタイプごとに分類 ](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) される指標が提供されるので、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できます。


使用権限サービスの監視 API について詳しくは、[ ドリルダウンツリー ](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) を参照してください。このツリーには、ESM 2.0 で使用可能な [ ディメンション ](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) （リソース）が示されています。

>[!NOTE]
>
>このテクニカルノートの内容は、[Clientless API](#clientless_device_type) にも追加されました。




## 実装

Adobe Pass認証指標の利点を最大限に活用するために、現在使用されており、正しい `deviceType` ークフローを設定する必要がある [ クライアントレス API](#web_srvs_summary) は 2 種類あります。

1. `regcode` を必須パラメーターとして持ち、`regcode` の作成時に設定された `deviceType` パラメーターを次の API 呼び出しで使用する API。
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. `deviceType` をオプションパラメーターとして持つ API:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

`deviceType` パラメーターを使用して、すべての API に対して正しいクライアントレスデバイスタイプを渡すことをお勧めします。
