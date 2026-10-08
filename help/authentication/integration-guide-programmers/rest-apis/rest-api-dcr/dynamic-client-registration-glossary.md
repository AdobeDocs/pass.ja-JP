---
title: 動的クライアント登録（DCR）用語集
description: 動的クライアント登録（DCR）用語集
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%
---
# 動的クライアント登録（DCR）用語集 {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このドキュメントでは、Adobe Pass Authentication Dynamic Client Registration （DCR）の統合時に使用される用語の定義について説明します。

>[!MORELIKETHIS]
> 
> * [REST API v2用語集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## 用語集 {#glossary-terms}

### A {#a}

#### アクセストークン {#access-token}

アクセストークンは、保護されたAPIへのアクセスを確実にするための[Dynamic Client Registration （DCR） ](#dcr) プロセスの結果として、Adobe Pass Authenticationによって生成されたトークンです。

### C {#c}

#### クライアント資格情報 {#client-credentials}

クライアント資格情報は、[動的クライアント登録（DCR） ](#dcr) プロセス中に生成される一意の値のセットであり、[ アクセストークン ](#access-token)の取得に使用されます。

#### カスタムスキーム {#custom-scheme}

カスタムスキームは、[Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) アプリケーションを参照する一意の値で、Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)から生成およびダウンロードでき、iOS デバイスで実行中のアプリケーションの最終リダイレクトとして使用されます。

### D {#d}

#### DCR {#dcr}

動的クライアント登録（DCR）は、[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)によって定義された認証メカニズムであり、[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)によって記述されているOAuth 2.0認証フレームワークに基づいています。

DCRは、保護されたAPIへのアクセスをさらに有効にできるAdobe Pass認証サービスとして[ プログラマー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)に配信されます。

詳しくは、[動的クライアント登録の概要](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)のドキュメントを参照してください。

### R {#r}

#### 登録済みアプリ {#registered-application}

登録アプリケーションは、[Dynamic Client Registration （DCR） ](#dcr) プロセスを進める必要がある[Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) アプリケーションに関する情報を格納するAdobe Pass Authentication conceptです。

### S {#s}

#### ソフトウェア声明 {#software-statement}

ソフトウェアステートメントは、Adobe Pass [TVE ダッシュボード ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)からダウンロードできるJSON Web トークン （JWT）で、[Dynamic Client Registration （DCR） ](#dcr) プロセスの一部として使用することを目的としています。
