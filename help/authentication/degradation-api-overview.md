---
title: 低下 API の概要
description: 低下 API の概要
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 95c3b1cbce4a591ce387ae3b242721e50ba2ddb1
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# 低下 API の概要 {#degradation-api-overview}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> Degradation API を使用する前に、次の前提条件が満たされていることを確認してください。
>
> * [ クライアント資格情報の取得 ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API ドキュメントの説明に従って、クライアント資格情報を取得します。
> * [ アクセストークンの取得 ](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API ドキュメントの説明に従って、アクセストークンを取得します。
>
> 登録されたアプリケーションを作成してソフトウェアのステートメントをダウンロードする方法について詳しくは、[ 動的クライアント登録の概要 ](./dcr-api/dynamic-client-registration-overview.md) ドキュメントを参照してください。

## 一般情報 {#general_info}

>[!NOTE]
>
>この API は一般公開されていません。 可用性の更新については、Adobe担当者にお問い合わせください。

この機能は、特定の MVPD 認証および認可エンドポイントを一時的にバイパスする機能を統合の 3 つの主要なパーティ（プログラマ、MVPD、およびAdobe）に提供します。 通常はプログラマがそのようなアクションを開始しますが、誰が劣化イベントをトリガーしているかに関係なく、そのアクションは影響を受ける MVPD との事前に合意された取り決めに依存します。

この機能の主なユースケースは、ライブスポーツや大きなイベントの際に発生します。 このような高トラフィックのシナリオでは、特定の MVPD エンドポイントの負荷が高くなりすぎて、ユーザーの応答時間が非常に長くなる可能性があります。 このような状況で良好なユーザーエクスペリエンスを維持するために、プログラマーは、ユーザーを一時的に自動認証/自動承認できる劣化規則をトリガーするか、使用可能な MVPD 一覧から削除して MVPD を無効にすることを決定する場合があります。

劣化ルールは一定期間のみ適用されます。 この機能の主な顧客はスポーツチャネルとライブニュースチャネルですが、MVPD サービスは時々減少するため、プログラマーはこの機能にアクセスしたいと考えるかもしれません。

低下に関する注記：

 – この機能は、使用状況の監視 API と共に使用するように設計されています。この API は、MVPD あたりの認証と承認の数、平均認証待ち時間、および完全なサービス概要に必要なその他の指標に関するリアルタイム情報を提供します。
 – この機能では、Adobe Pass Authentication サービスのバイパスはできません。 Adobe Pass認証がダウンしている場合、サービス内には、ユーザーがコンテンツを表示できるメカニズムはありません。 ただし、サイトやアプリは、自分でAdobe Pass認証を迂回することができます。
 – 現在、Adobeは直接トリガーを低下させません。この判断は、常に MVPD でこのような条件に同意した特定のプログラマーが行う必要があります。 今後、MVPD と契約（Adobe Pass対策）を結ぶことができれば、SLA認証が積極的に劣化ルールをトリガーする可能性があります。

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
