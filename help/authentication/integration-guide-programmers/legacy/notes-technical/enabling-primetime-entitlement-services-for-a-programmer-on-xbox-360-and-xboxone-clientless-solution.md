---
title: Xbox 360およびXboxOne クライアントレスでのプログラマーのAdobe Pass使用権限サービスの有効化
description: Xbox 360およびXboxOne クライアントレスでのプログラマーのAdobe Pass使用権限サービスの有効化
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 0%
---
# （レガシー） Xbox 360およびXboxOne クライアントレスでのプログラマーに対するAdobe Passの使用権限サービスの有効化 {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。


1. プログラマーは、次の情報を提供することで、Xbox 360/One for Adobe Pass Authentication クライアントレスソリューションを有効にするためのZendesk チケットを作成します。

   1. プラットフォーム：例：Xbox 360、Xbox One

   1. 依頼者ID: netgeo、CNNなど

1. AdobeはX509証明書を作成し、秘密鍵とパスワードを最後に設定します。

1. Adobeは、チケットまたは電子メールでプログラマーに公開証明書（X509証明書）を提供します。

1. その後、プログラマーは、Microsoftに登録されたアプリのGDNP ポータルにその公開証明書をインストールする必要があります。

1. その後、プログラマーはXboxOneまたは360のJWT （Java Web Token）またはSTS TokenをMicrosoft Xbox Live サービスからそれぞれリクエストします。これは、手順3で提供されるX509公開証明書を使用して暗号化されます。

1. これらは、Xbox デバイスの一意のdeviceIdを含むトークンです。 以下のように&#39;x&#39; パラメーターを使用して、認証ヘッダーにトークン（JWTまたはSTS）を含めます。

   1. Xbox 360の場合、XSTS トークンはAdobe Pass有料テレビ認証に送信する前にBase64 エンコードされている必要があります。
   1. Xbox Oneの場合、JWTは既に適切にエンコードされているため、追加のエンコードは行われません。

1. Xbox デバイスからのすべてのAPI呼び出しには、上記のトークンがx パラメーターに含まれている認証ヘッダーが含まれている必要があります。



>[!NOTE]
>
>特にXboxには、デジタル署名に関連する独自の要件がいくつかあります。 XBox コンソールのデバイス IDは、XSTS トークンに含まれています。  Xbox 360の場合、これは暗号化されたSAML アサーションです。Xbox Oneの場合、これは暗号化されたJWTです。 XBox コンソールアプリは、XSTS トークン全体をAdobe Pass有料テレビ認証に送信します。 Adobe Pass pay-TV認証は、公開鍵を使用してトークンを復号化し、トークンを解析して、そこからdeviceIdを抽出します。

>[!NOTE]
>
>XSTS トークンの長さが大きいため、XBox コンソールには技術的な制限があります。Adobe Passの有料テレビ認証APIにHTTP GET パラメーターとしてトークンを送信することはできません。 これに対処するため、Adobe Passの有料テレビ認証では、APIを呼び出す際に、HTTP ヘッダー「認証」の一部としてXSTS トークンを送信できます。 XSTS トークンは、Adobe Pass有料テレビ認証からプログラマーに発行されたX.509証明書の公開鍵を使用して暗号化する必要があります。 Adobe Pass有料テレビ認証は、関連する秘密鍵を保存し、XSTS トークンを復号化し、そこからdeviceIdを抽出するために使用します。
