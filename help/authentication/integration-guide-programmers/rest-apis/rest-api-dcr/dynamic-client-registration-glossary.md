---
title: 動的クライアント登録（DCR）の用語集
description: 動的クライアント登録（DCR）の用語集
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
source-git-commit: ae02f53afc58b7d31f57bcc1e4dd1328f12abc3e
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 3%

---

# 動的クライアント登録（DCR）の用語集 {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

このドキュメントでは、Adobe Pass Authentication Dynamic Client Registration （DCR）を統合する際に使用する用語の定義について説明します。

>[!MORELIKETHIS]
> 
> * [REST API v2 用語集 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## 用語集の用語 {#glossary-terms}

### A {#a}

#### アクセストークン {#access-token}

アクセストークンは、保護された API へのアクセスを確実にすることを目的とした [Dynamic Client Registration （DCR） ](#dcr) プロセスの結果として、Adobe Pass Authentication によって生成されるトークンです。

### C {#c}

#### クライアント資格情報 {#client-credentials}

クライアント資格情報は、[Dynamic Client Registration （DCR） ](#dcr) プロセス中に生成される一意の値のセットであり、[ アクセストークン ](#access-token) の取得に使用されることを目的としています。

#### カスタムスキーム {#custom-scheme}

カスタムスキームは、Adobe Pass[TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) から生成およびダウンロードできる [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) アプリケーションを参照する一意の値で、iOS デバイスで動作するアプリケーションでの最終リダイレクトとして使用されます。

### 日 {#d}

#### DCR {#dcr}

Dynamic Client Registration （DCR）は、[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) で定義されている認証メカニズムであり、[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) で説明されている OAuth 2.0 認証フレームワークに基づいています。

DCR は、保護された API へのアクセスをさらに可能にするAdobe Pass認証サービスとして [ プログラマー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) に配信されます。

詳しくは、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

### R {#r}

#### 登録アプリケーション {#registered-application}

登録済みアプリケーションは、[Dynamic Client Registration （DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) プロセスを続行する必要がある [ プログラマー ](#dcr) アプリケーションに関する情報を格納する、Adobe Pass認証コンセプトです。

### S {#s}

#### ソフトウェア明細書 {#software-statement}

このソフトウェアステートメントは、Adobe Pass[TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) からダウンロードできる JSON web トークン（JWT）であり、[Dynamic Client Registration （DCR） ](#dcr) プロセスの一部として使用することを目的としています。
