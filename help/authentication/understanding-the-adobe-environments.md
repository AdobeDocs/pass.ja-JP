---
title: Adobe環境について
description: Adobe環境について
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: c6afb9b080ffe36344d7a3d658450e9be767be61
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Adobe環境について {#understanding-the-adobe-environments}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

Adobe環境を説明した公式ドキュメントが利用可能です [環境の設定とプリクォールでのテスト](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

Adobe環境の概要は次のとおりです。

Adobeには次の 2 つの環境があります。 **事前認定** および **リリース**.

* 事前認定環境では、リリースする新しいビルドを準備しています。

* 現在のリリースビルドは、リリース環境にあります。

各環境には次の 2 つのプロファイルがあります。 **ステージング** および **実稼動**.

* ステージングプロファイルは、MVPD ステージングサーバーに接続します
* 実稼動プロファイルは、MVPD 実稼動プロファイルに接続します。

2 つのプロファイルがある理由は、ステージングプロファイルで新しいパートナーの運用を開始する準備をしており、今後のビルド（事前検証）またはリリース 1 （より安定した）でシステムをテストしたいからです。

パートナーが新しいバージョンをテストする場合は、いくつかの追加手順を行う必要があります。 参照、 [環境の設定とプリクォールでのテスト](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

上記の手順に従うことで、今後のリリースが事前認定環境でテストされることが保証されます。
