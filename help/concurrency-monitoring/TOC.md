---
product: adobe primetime
feature: Concurrency Monitoring
audience: end-user
user-guide-title: Adobe Pass 同時実行モニタリング
user-guide-description: 複数のアプリケーションでの同時使用に関する制限を定義し、適用する方法を説明します。
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# Adobe Pass Concurrency Monitoring ヘルプ {#cm}

- [CMの概要](cm-home.md)
- はじめに {#getting-started}
   - [同時視聴数モニタリングの概要](getting-started/getting-started-overview.md)
   - [主要な概念](getting-started/key-concepts.md)
- 統合ガイド {#integration-guide}
   - [用語集](cm-glossary.md)
   - API リファレンス {#api-reference}
      - [概要](api/api-reference-overview.md)
      - [API エンドポイント](api/api-endpoints.md)
      - [API リファレンス](api/cm-api-overview.md)
   - [同時実行モニタリング使用状況API](reports/cmu-api.md)
   - [API アクセス](reports/cmu-api-access.md)
   - [同時実行モニタリング使用状況レポートの例](reports/cm-usage-reports-examples.md)
- ユースケースと実装 {#use-cases-implementation}
   - [ユースケースの概要](use-cases/cm-use-cases.md)
   - [LIFOとFIFO戦略](use-cases/lifo-fifo-strategies.md)
   - [セッションの競合の処理](use-cases/handling-409-errors.md)
   - [実装モデル](technical/implementation-models.md)
   - [シングルテナントポリシー複数アプリ](technical/single-tenant-policy-mult-app.md)
   - [複数のアプリの同時使用を制限](technical/restrict-concurr-usage-mult-apps.md)
- [CM使用状況レポート](reports/cm-usage-reports.md)
- テクニカルノート {#tech-notes}
   - [標準メタデータ属性](technical/standard-metadata-attributes.md)
   - [カスタムメタデータ](technical/custom-metadata.md)
   - [ポリシー決定ポイント](technical/cm-policy-decision-point.md)
   - [ポリシー情報ポイント](technical/policy-info-pt-versionone.md)
   - [スロットリング](technical/throttling-mechanism.md)
   - [同時視聴数モニタリングでVODとライブコンテンツを区別する方法](technical/vod-live-dist.md)
   - [データ保持ポリシー](technical/data-retention-policy.md)
- リリース {#cm-release-notes}
   - [同時実行モニタリング - 3.6.2 リリースノート](releases/rn-cm-services-362.md)
   - [同時実行モニタリング - 3.6.1 リリースノート](releases/rn-cm-services-361.md)
   - [同時実行モニタリング - 3.6.0 リリースノート](releases/rn-cm-services-360.md)
   - [同時実行モニタリング - 3.5.1 リリースノート](releases/rn-cm-services-351.md)
   - [同時実行モニタリング - 3.5.0 リリースノート](releases/rn-cm-services-350.md)
   - [同時実行モニタリング - 3.4.4 リリースノート](releases/rn-cm-services-344.md)
   - [同時実行モニタリング - 3.4.3 リリースノート](releases/rn-cm-services-343.md)
   - [同時実行モニタリング - 3.4.2 リリースノート](releases/rn-cm-services-342.md)
   - [同時実行モニタリング - 3.4.0 リリースノート](releases/rn-cm-services-340.md)
   - [同時実行モニタリング - 3.3.7 リリースノート](releases/rn-cm-services-337.md)
   - [同時実行モニタリング - 3.3.6 リリースノート](releases/rn-cm-services-336.md)
   - [同時実行モニタリング - 3.3.2 リリースノート](releases/rn-cm-services-332.md)
   - [同時実行モニタリング - 3.3.1 リリースノート](releases/rn-cm-services-331.md)
   - [同時実行モニタリング - 3.3.0 リリースノート](releases/rn-cm-services-330.md)
   - [同時実行モニタリング - 3.2.3 リリースノート](releases/rn-cm-services-323.md)
   - [同時実行モニタリング - 3.2.2 リリースノート](releases/rn-cm-services-322.md)
   - [同時実行モニタリング - 3.2.1 リリースノート](releases/rn-cm-services-321.md)
   - [同時実行モニタリング - 3.2 リリースノート](releases/rn-cm-services-320.md)
   - [同時実行モニタリング - 3.1 リリースノート](releases/rn-cm-services-31.md)
   - [同時実行モニタリング - 3.0 リリースノート](releases/rn-cm-services-30.md)
   - [同時実行モニタリング - 2.9.6 リリースノート](releases/rn-cm-296.md)
   - [同時実行モニタリング - 2.9 リリースノート](releases/rn-cm-29.md)
   - [同時実行モニタリング - 2.8.2 リリースノート](releases/rn-cm-282.md)
   - [同時実行モニタリング - 2.7.2 リリースノート](releases/rn-cm-272.md)
   - [同時実行モニタリング - 2.6.0 リリースノート](releases/rn-cm-260.md)
   - [同時実行モニタリング - 2.5.0 リリースノート](releases/rn-cm-250.md)
   - [同時実行モニタリング - 2.3.2 リリースノート](releases/rn-cm-232.md)
   - [同時実行モニタリング - 2.2.2 リリースノート](releases/rn-cm-222.md)
- [サポート手順](support/cm-escalation-procedures.md)
