---
title: Adobe Pass Concurrency Monitoring 2.6.0 リリースノート
description: Adobe Pass Concurrency Monitoring 2.6.0 リリースノート
exl-id: f24980e3-ffe8-4b5e-8adc-ae443baed40f
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%
---
# Adobe Pass Concurrency Monitoring 2.6.0 リリースノート {#cm-260}


このページでは、このリリースの新機能、変更点、既知の問題について説明します。



## リリース日：2016年11月10日（PT）



## 新機能

このリリースでは、現在のストリームを開始できるように、既存のストリームを終了する機能が追加されました（つまり、kill ストリーム）。



**リモート終了**

* 409 Conflict応答では、アドバイスの「conflicts」フィールド内にリストされている各セッションには、terminicode属性が含まれます。
* 競合するセッションのリストを表示するプロンプトが表示され、殺すセッションを選択できます
* リモートセッションは、セッションの起動中に（選択した終了コードを値として） X-Terminate リクエストヘッダーを渡すことによってのみ強制終了できます。
* 410 Gone応答に対して、現在の応答を殺したセッションを示すために、新しいタイプの「アドバイス」が定義されました。


詳しくは、更新されたドキュメントを参照してください。



>[!NOTE]
>
>アクティブなセッションのリストに使用されるセッション定義が更新され、アプリケーション IDではなく、アプリケーション名とデバイス名が含まれるようになりました。




## バグ修正 {#bug-fixes}

サーバー応答の重複ヘッダーを削除しました（修正には、CORS ヘッダーと日付1の両方が含まれます）。




## 既知の問題 {#known-issues}

該当なし
