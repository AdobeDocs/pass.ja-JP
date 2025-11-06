---
title: 縮退機能
description: 縮退機能
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# 縮退機能 {#degradation-feature}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

ライブスポーツや主要イベントなどのダイナミックな世界では、シームレスな視聴体験を保証することが不可欠です。 これらのイベント中にトラフィックが多くなると、Multichannel Video Programming Distributor （MVPD）の認証および承認エンドポイントが圧迫され、遅延や中断が発生する可能性があります。

Adobe Pass認証では、これらの課題に対応するために、固有のMVPD認証および認証エンドポイントを一時的にバイパスするソリューションである **最適化機能** を使用しています。 この機能は、MVPD システムの負荷が高いために応答時間が低下する可能性があるピークトラフィックイベントで特に役立ちます。

**最適化機能** は、プログラマーにとって重要な保護機能であり、サービスの継続性を確保できます。 主なオーディエンスにはスポーツやニュースのライブチャネルが含まれますが、MVPD エンドポイントによって引き起こされる混乱のリスクを軽減しようとするすべてのプログラマーに対してユーティリティが拡張されます。

>[!IMPORTANT]
>
> Degradation API はプレミアム機能で、Adobeの現在のライセンスが必要です。

最適化ルールを適用すると、プログラマーは自動認証または自動承認を一時的に有効にして、最適化が適用される期間のコンテンツに中断なくアクセスできるようになります。 MVPD との事前に決められた合意に基づいて、プログラマは常にデグレード アクションを開始します。 Adobeは現在、トリガーの低下を直接引き起こすことはありませんが、将来的には、サービスレベル契約（SLA）が確立された場合のプロアクティブな管理が含まれる可能性があります。

この機能は、[ 使用状況モニタリング API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) と組み合わせて使用するように設計されており、MVPD との事前契約に基づいて、Adobe Pass認証は、重要な瞬間にユーザーエクスペリエンス、信頼性、運用制御のバランスを取る強力なツールを提供します。

>[!IMPORTANT]
>
> この機能では、Adobe Pass Authentication サービス自体をバイパスすることはできません。 Adobe Pass認証を使用できない場合、このサービスには、ユーザーアクセスを容易にする組み込みのメカニズムはありません。 その場合、サイトやアプリケーションは、コンテンツ配信を維持するために、独自の代替ルーティングソリューションを実装することを選択できます。

## API アクセスの低下 {#degradation-api-access}

[Degradation API](#degradation-api) にアクセスする前に、Dynamic Client Registration （DCR）プロセスで必要な手順を完了する必要があります。 この必須プロセスにより、Degradation API とのやり取りに必要なアクセストークンが確保されます。

包括的な手順については、[Dynamic Client Registration Overview](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## API の低下 {#degradation-api}

劣化 API は、プログラマーが特定の MVPD の劣化ルールを管理できる RESTful API です。 API は、アクティブな劣化ルールのステータスを取得するだけでなく、アクティブ化、削除する手段も提供します。

Degradation API について詳しくは、次の Zendesk ドキュメント [Adobe Pass認証を参照してください | Degradation API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) ダウンロードするPDF ファイルを探します。

## REST API V2 {#rest-api-v2}

劣化機能を利用するには、TV Everywhere （TVE）アプリケーションとAdobe Pass認証 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) のやり取りを変更するコード更新を実装する必要があります。

これらのアップデートと関連ワークフローの包括的なガイドについては、[ 縮退したアクセスフロー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md) ドキュメントを参照してください。
