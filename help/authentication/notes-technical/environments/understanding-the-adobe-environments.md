---
title: Adobe環境について
description: Adobe環境について
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%
---
# Adobe環境について {#understanding-the-adobe-environments}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

Adobe環境について説明する公式ドキュメントは、[Pre-qualでの環境の設定とテスト &#x200B;](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)で利用できます。

Adobeの環境は、いくつかの単語で要約されています。

Adobeには、**事前承認**&#x200B;と&#x200B;**リリース**&#x200B;の2つの環境があります。

* プレクオリフィケーション環境で、新しいビルドをリリースする準備を行います。

* 現在のリリースビルドはリリース環境上にあります。

各環境には2つのプロファイルがあります：**ステージング**&#x200B;と&#x200B;**実稼動**。

* ステージングプロファイルは、MVPD ステージングサーバーに接続します
* 実稼動プロファイルは、MVPDの実稼動プロファイルに接続されます。

2つのプロファイルを持つ理由は、ステージングプロファイルで新しいパートナーを公開する準備を行い、次のビルド（事前選定）またはリリースビルド（より安定）でシステムをテストしたいからです。

パートナーが新しいバージョンをテストする場合は、さらに複数の手順を実施する必要があります。 詳しくは、[環境の設定とPre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)でのテストを参照してください。

上記の手順に従うことで、今後のリリースが事前検証環境でテストされることが保証されます。
