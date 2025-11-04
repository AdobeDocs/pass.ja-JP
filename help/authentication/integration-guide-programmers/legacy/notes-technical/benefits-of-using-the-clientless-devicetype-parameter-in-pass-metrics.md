---
title: Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット
description: Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# （レガシー）Adobe Pass認証指標でクライアントレス deviceType パラメーターを使用するメリット {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

## コンテキスト

オプションですが、クライアントレス API のパラメーター `deviceType` が存在する場合は、[&#x200B; 使用権限サービスのモニタリング &#x200B;](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md) で公開されるAdobe Pass認証指標で使用されます。

Adobe Pass認証指標の `deviceType` パラメーターとその **メリット** との関連は最初は述べていなかったことを考慮すると、このテクニカルノートの範囲は、それらに関する詳細を追加することです。

## 説明

`deviceType` パラメーターは、最初のバージョン以降 Clientless API に存在していましたが、Adobe Pass認証指標への影響が新しいリリースで追加されました。



>[!IMPORTANT]
>
>パラメーター `deviceType` が正しく設定されている場合、権利付与サービスのモニタリングに次の **特典** が含まれます。クライアントレスを使用する際に、[&#x200B; デバイスタイプごとに分類 &#x200B;](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) される指標が提供されるので、Roku、AppleTV、Xbox など、様々なタイプの分析を実行できます。


使用権限サービスの監視 API について詳しくは、ESM 2.0 で使用可能な [&#x200B; ドリルダウンツリー &#x200B;](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#esm_dimensions) （リソース）を参照してください。

>[!NOTE]
>
>このテクニカルノートの内容は、[Clientless API](#clientless_device_type) にも追加されました。




## 実装

Adobe Pass認証指標の利点を最大限に活用するために、現在使用されており、正しい [&#x200B; ークフローを設定する必要がある &#x200B;](#web_srvs_summary) クライアントレス API`deviceType` は 2 種類あります。

1. `regcode` を必須パラメーターとして持ち、`deviceType` の作成時に設定された `regcode` パラメーターを次の API 呼び出しで使用する API。
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/](#reg_serv)

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
