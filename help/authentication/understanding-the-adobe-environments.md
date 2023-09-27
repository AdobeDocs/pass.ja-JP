---
title: Adobe環境について
description: Adobe環境について
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Adobe環境について {#understanding-the-adobe-environments}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

環境を説明する公式ドキュメントについては、Adobe環境に関するを参照してください。 [Pre-qual での環境とテストの設定](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md):

Adobe環境の概要を次の単語で説明します。

Adobeには次の 2 つの環境があります。 **事前認定** および **リリース**.

* 事前認定環境で、リリースされる新しいビルドを準備中です。

* 現在のリリースビルドは、リリース環境上にあります。

各環境には次の 2 つのプロファイルがあります。 **ステージング** および **実稼動**.

* ステージングプロファイルは、MVPDs ステージングサーバーに接続します
* 実稼働プロファイルは、MVPD 実稼働プロファイルに接続します。

2 つのプロファイルを持つ理由は、ステージングプロファイルで新しいパートナーを準備して有効にするためです。また、今後のビルド（事前認定）またはリリース 1（より安定した）でシステムをテストしたいと考えています。

パートナーが新しいバージョンをテストする場合は、追加の手順を実行する必要があります。 詳しくは、 [Pre-qual での環境とテストの設定](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

上記の手順に従うことで、今後のリリースは事前認定環境でテストされることになります。
