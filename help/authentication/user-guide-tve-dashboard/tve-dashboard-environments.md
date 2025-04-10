---
title: TVE ダッシュボード環境
description: TVE ダッシュボードの様々な環境の使用と操作について説明します。
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 環境 {#environments}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

TVE ダッシュボードは、Adobe Pass認証内の特定の目的を満たすようにカスタマイズされた、様々な環境を提供します。 主な環境は次の 2 つです。

* **前提条件**：事前認定環境は、実稼動環境へのデプロイメント前に新しいビルドを準備およびテストするためのテスト基盤として機能します。

* **リリース**：リリース環境は、実稼動用に完成およびテストされたビルドをホストします。

各環境内には、次の 2 つの異なるプロファイルがあります。

* **ステージング**：ステージングプロファイルは、運用開始前に統合のテストと検証をおこなうために MVPD のステージングサーバーに接続します。

* **実稼動**：実稼動プロファイルは、実際の実稼動アクティビティに対する MVPD の実稼動プロファイルに接続します。

## ユースケース

TVE ダッシュボードの環境は、アプリケーションのライフサイクルを通じて様々なユースケースに対応します。 これらの環境を使用すると、次のことができます。

### 前提条件のステージング

* MVPD のステージングエンドポイントを使用して、Adobe Pass Authentication Server の新しい未リリースの機能を検証します。
* 主にAdobe Pass認証製品チームが、新しい MVPD 統合を追加して検証するために使用します。

### 初期生成

* MVPD の実稼動エンドポイントを使用して、Adobe Pass Authentication Server のリリースされていない新しい機能または設定を検証します。
* MVPD の実稼動エンドポイントを使用して、チャネルごとに新しいアプリケーションバージョンを検証します。
* 実稼動環境にプッシュする前に、設定の変更をすべて検証します。

### リリースステージング

* MVPD のステージングエンドポイントを使用して、チャネルごとに新しいアプリケーションバージョンを検証します。
* この環境内でパフォーマンステストまたは処理能力テストを実施します。

### 実稼動をリリース

* は、すべてのエンドユーザーが一般に利用できる最新のAdobe Pass リリースビルドのライブ環境を表します。
* コードと設定の安定性を維持し、エンドユーザーのアプリケーションの設定変更を直ちに反映します。

## 環境の切り替え {#switch-environments}

Adobe Pass Authentication TVE Dashboard 環境を切り替えるには、次の手順に従います。

1. プログラマーの資格情報を使用してログインします。

1. 左側のパネルの上部にある「**環境**」ドロップダウンメニューから、必要なステージング環境または実稼動環境を選択します。

   ![TVE ダッシュボード環境ドロップダウン ](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Adobe Pass認証 TVE ダッシュボード環境ドロップダウンメニュー*

>[!NOTE]
>
> 設定は、設定に基づいて環境ごとに異なる場合があります。
