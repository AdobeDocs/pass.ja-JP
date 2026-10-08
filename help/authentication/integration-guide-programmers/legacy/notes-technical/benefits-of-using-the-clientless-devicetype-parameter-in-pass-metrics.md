---
title: Adobe Pass Authentication metricsでClientless deviceType パラメーターを使用するメリット
description: Adobe Pass Authentication metricsでClientless deviceType パラメーターを使用するメリット
exl-id: a5004887-d5fa-468e-971b-10806519175b
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%
---
# （レガシー） Adobe Pass Authentication metricsでClientless deviceType パラメーターを使用するメリット {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>

## コンテキスト

オプションですが、クライアントレス APIのパラメーター`deviceType`が存在する場合は、[使用権限サービスモニタリング ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)を通じて公開されているAdobe Pass認証メトリックで使用されます。

Adobe Pass認証指標に関する`deviceType` パラメーターとその&#x200B;**benefits**&#x200B;の間の接続が最初に記載されていなかったことを考えると、このテクニカルノートの範囲は、それらのパラメーターに関する詳細情報を追加することです。

## 説明

`deviceType` パラメーターは、最初のバージョン以降Clientless APIに存在しましたが、Adobe Pass認証メトリックに対する影響は、より新しいリリースで追加されました。



>[!IMPORTANT]
>
>パラメーター`deviceType`が正しく設定されている場合は、使用権限サービス監視で次の&#x200B;**benefit**&#x200B;が使用されます。クライアントレスを使用する場合は、デバイスの種類](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)ごとに[分割される指標が提供されるため、Roku、AppleTV、Xboxなどでさまざまな種類の分析を実行できます。


使用権限サービス監視APIについて詳しくは、[ ドリルダウンツリー](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree)を参照してください。このツリーには、ESM 2.0で使用可能な[ ディメンション ](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) （リソース）が示されています。

>[!NOTE]
>
>このテクニカルノートの内容は、[ クライアントレス API](#clientless_device_type)にも追加されました。




## 導入

Adobe Pass認証メトリックを最大限に活用するには、現在使用されている[ クライアントレス API](#web_srvs_summary)の2種類があり、正しい`deviceType`を設定する必要があります。

1. 必須パラメーターとして`regcode`を持ち、次のAPI呼び出しで`regcode`を作成するときに設定された`deviceType` パラメーターを使用するAPI:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. オプションのパラメーターとして`deviceType`を持つAPI:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [<span class="s1">\&lt;SP\_FQDN\>/api/v1/tokens/authn</span>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

`deviceType` パラメーターを使用し、すべてのAPIに正しいクライアントレスデバイスタイプを渡すことをお勧めします。
