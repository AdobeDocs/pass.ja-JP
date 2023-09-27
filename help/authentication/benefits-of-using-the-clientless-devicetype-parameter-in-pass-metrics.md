---
title: Adobe Pass認証指標で Clientless deviceType パラメーターを使用するメリット
description: Adobe Pass認証指標で Clientless deviceType パラメーターを使用するメリット
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Adobe Pass認証指標で Clientless deviceType パラメーターを使用するメリット {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

## コンテキスト

オプションですが、 `deviceType` クライアントレス API から（存在する場合）、を通じて公開されるAdobe Pass認証指標で使用されます。 [エンタイトルメントサービスの監視](/help/authentication/entitlement-service-monitoring-overview.md).

この点を考えると `deviceType` パラメーターとその **メリット** Adobe Pass認証指標は最初は記載されていませんでしたが、このテクニカルノートの範囲は、それらに関する詳細情報を追加することです。

## 説明

The `deviceType` 最初のバージョン以降、Clientless API にパラメーターが存在していましたが、Adobe Pass認証指標に対する影響は、より新しいリリースで追加されました。



>[!IMPORTANT]
>
>パラメーターが `deviceType` が正しく設定されている場合、次の値が含まれます。 **利益** エンタイトルメントサービスの監視： [デバイスタイプ別に分類](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) クライアントレスを使用する場合に、Roku、AppleTV、Xbox など、様々な種類の分析を実行できます。


Entitlement Service Monitoring API について詳しくは、 [ドリルダウン・ツリー](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) これが示す [寸法](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) （リソース）ESM 2.0 で使用可能。

>[!NOTE]
>
>このテクニカルノートの内容も [クライアントレス API](#clientless_device_type).




## 実装

Adobe Pass認証指標のメリットを最大限に活用するには、次の 2 種類があります。 [クライアントレス API](#web_srvs_summary) 現在使用中で、正しい `deviceType` 設定：

1. を持つ API `regcode` を必須パラメーターとして指定し、 `deviceType` 作成時に設定されたパラメーター `regcode`を次の API 呼び出しで使用します。
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. を持つ API `deviceType` をオプションのパラメーターとして指定します。
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

以下を推奨します。 `deviceType` パラメーターを渡し、すべての API に対して適切なクライアントレスデバイスタイプを渡します。
