---
title: Adobe環境について
description: Adobe環境について
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Adobe環境について {#understanding-the-adobe-environments}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

Adobe環境について説明した公式ドキュメントは [&#x200B; 環境の設定と Pre-qual でのテスト &#x200B;](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md) から入手できます。

Adobe環境の概要は次のとおりです。

Adobeには、**事前認定** と **リリース** の 2 つの環境があります。

* 事前認定環境では、リリースする新しいビルドを準備しています。

* 現在のリリースビルドは、リリース環境にあります。

各環境には 2 つのプロファイル（**ステージング** と **実稼動** があります。

* ステージングプロファイルは、MVPD ステージングサーバーに接続します
* 実稼動プロファイルは、MVPD 実稼動プロファイルに接続します。

2 つのプロファイルがある理由は、ステージングプロファイルで新しいパートナーの運用を開始する準備をしており、今後のビルド（事前検証）またはリリース 1 （より安定した）でシステムをテストしたいからです。

パートナーが新しいバージョンをテストする場合は、いくつかの追加手順を行う必要があります。 [&#x200B; 環境の設定と Pre-qual でのテスト &#x200B;](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md) を参照してください。

上記の手順に従うことで、今後のリリースが事前認定環境でテストされることが保証されます。
