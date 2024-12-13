---
title: Xbox 360 および XboxOne クライアントレスでのプログラマーに対するAdobe Pass使用権サービスの有効化
description: Xbox 360 および XboxOne クライアントレスでのプログラマーに対するAdobe Pass使用権サービスの有効化
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# （従来のバージョン） Xbox 360 および XboxOne クライアントレスで、プログラマー向けのAdobe Pass使用権限サービスを有効にする {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。




1. プログラマーは、次の情報を提供して、Adobe Pass認証のクライアントレスソリューションに対して Xbox 360/One を有効にするための Zendesk チケットを作成します。

   1. プラットフォーム：Xbox 360、Xbox One など

   1. 要求者 ID：例：netgeo、CNN など

1. Adobeは X509 証明書を作成し、その最後に秘密鍵とパスワードを設定します。

1. Adobeは、チケットまたはメールでプログラマーに（X509 証明書の）公開証明書を提供します。

1. その後、プログラマーは、Microsoftに登録されたアプリの GDNP ポータルに、その公開証明書をインストールする必要があります。

1. 次に、プログラマーは、Microsoft Xbox Live サービスから、それぞれ XboxOne または 360 用の JWT （Java Web トークン）または STS トークンをリクエストします。このトークンは、手順 3 で提供された X509 公開証明書を使用して暗号化されます。

1. これらは、Xbox デバイス用の一意の deviceId を含むトークンです。 以下のように、「x」パラメーターを使用して、Authorization ヘッダーにトークン（JWT または STS）を含めます。

   1. Xbox 360 の場合、Adobe Passの有料テレビ放送に送信する前に、XSTS トークンを Base64 エンコードする必要があります。
   1. Xbox One の場合、JWT はすでに適切にエンコードされているので、追加のエンコーディングは行われません。

1. Xbox デバイスからのすべての API 呼び出しには、x パラメーターに前述のトークンを含む認証ヘッダーが含まれている必要があります。



>[!NOTE]
>
>特に Xbox には、デジタル署名に関連した独自の要件がいくつかあります。 XBox コンソールのデバイス ID は、XSTS トークンに含まれます。  Xbox 360 の場合、これは暗号化された SAML アサーションです。Xbox One の場合、これは暗号化された JWT です。 XBox コンソールアプリは、XSTS トークン全体をAdobe Passの有料テレビ認証に送信します。 Adobe Passの有料 TV 認証では、公開鍵を使用してトークンを復号化し、トークンを解析して、そのトークンから deviceId を抽出します。

>[!NOTE]
>
>XSTS トークンの長さが長いため、XBox コンソールには技術的な制限があります。つまり、トークンを HTTP GETのパラメーターとしてAdobe Passの有料テレビ認証 API に送信することはできません。 これに対処するために、Adobe Passの有料テレビ認証では、API を呼び出す際に、HTTP ヘッダー「Authorization」の一部として XSTS トークンを送信できます。 XSTS トークンは、Adobe Pass有料テレビ認証からプログラマーに発行された X.509 証明書の公開鍵を使用して暗号化する必要があります。 Adobe Passの有料 TV 認証では、関連する秘密鍵が保存され、その秘密鍵を使用して XSTS トークンが復号化され、deviceId が抽出されます。
