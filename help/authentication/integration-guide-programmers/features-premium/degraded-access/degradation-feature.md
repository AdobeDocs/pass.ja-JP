---
title: デグラデーション機能
description: デグラデーション機能
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%
---
# デグラデーション機能 {#degradation-feature}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

ライブスポーツや大規模なイベントが開催されるダイナミックな世界では、シームレスな視聴者体験を保証することが不可欠です。 これらのイベント中にトラフィックが多いと、MVPD（Multichannel Video Programming Distributor）の認証および認証エンドポイントが過負荷になり、遅延や中断につながる可能性があります。

Adobe Pass Authenticationは、特定のMVPD認証と認証エンドポイントを一時的にバイパスできるソリューションである&#x200B;**Degradation Feature**&#x200B;を使用して、これらの課題に対処します。 この機能は、MVPDシステムへの負荷が大きいため、応答時間が低下する可能性があるピークトラフィックイベント時に特に役立ちます。

**デグラデーション機能**&#x200B;は、プログラマーにとって重要なセーフガードとなり、サービスの継続性を確保できます。 主なオーディエンスにはライブスポーツやニュースチャンネルが含まれていますが、その有用性はMVPDエンドポイントによる中断のリスクを軽減しようとしているプログラマーにも及んでいます。

>[!IMPORTANT]
>
> Degradation APIはプレミアム機能であり、Adobeからの現在のライセンスが必要です。

劣化ルールを適用することで、プログラマーは自動認証または自動認証を一時的に有効にし、劣化が適用される期間コンテンツへの中断のないアクセスを保証できます。 劣化アクションは、MVPDとの事前に設定された契約書に基づいて、プログラマによって常に開始されます。 Adobeは現在、製品劣化を直接トリガーしていませんが、サービスレベル契約（SLA）が締結された場合、将来の機能に先見的な管理が含まれる場合があります。

この機能は、[使用状況モニタリング API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)と共に使用するように設計されており、MVPDとの事前契約に基づいて、Adobe Pass Authenticationは、重要な瞬間にユーザーエクスペリエンス、信頼性、運用制御のバランスを取るための強力なツールを提供します。

>[!IMPORTANT]
>
> この機能では、Adobe Pass認証サービス自体をバイパスすることはできません。 Adobe Pass認証が利用できない場合、このサービスにはユーザーアクセスを容易にする組み込みメカニズムはありません。 そのような場合、サイトやアプリケーションは、コンテンツ配信を維持するために、独自の代替ルーティングソリューションを実装することを選択できます。

## 劣化API アクセス {#degradation-api-access}

[Degradation API](#degradation-api)にアクセスする前に、Dynamic Client Registration （DCR） プロセスで必要な手順を完了する必要があります。 この必須プロセスにより、Degradation APIを操作するために必要なアクセストークンが確保されます。

包括的な手順については、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)のドキュメントを参照してください。

## 劣化API {#degradation-api}

Degradation APIは、プログラマーが特定のMVPDの劣化ルールを管理できるようにするRESTful APIです。 APIは、アクティブ化、削除、およびアクティブな劣化ルールのステータスの取得を行うための手段を提供します。

Degradation APIについて詳しくは、次のZendesk ドキュメント [Adobe Pass Authentication | Degradation API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3)を参照し、ダウンロードするPDF ファイルを探します。

## REST API V2 {#rest-api-v2}

デグラデーション機能を利用するには、コードの更新を実装して、TV Everywhere （TVE） アプリケーションがAdobe Pass Authentication [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)とどのように連携するかを変更する必要があります。

これらの更新と関連ワークフローに関する包括的なガイドについては、[&#x200B; デグレードされたアクセス フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)のドキュメントを参照してください。
