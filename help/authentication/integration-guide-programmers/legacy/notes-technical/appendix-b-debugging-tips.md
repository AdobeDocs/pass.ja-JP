---
title: 付録 B 「デバッグのヒント」
description: 付録 B 「デバッグのヒント」
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# （レガシー）付録 B：デバッグのヒント {#appendix-b-debugging-tips}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 一時データの消去 {#clearing-temporary-data}

Adobe Pass Authentication には、ブラウザーのキャッシュ、LSO のキャッシュ、Cookie などの一時データが格納されます。 テスト時に明確な状態を確保するために、一時データのクリアは重要です。

- [ブラウザーのキャッシュと Cookie のクリア](#clearing-the-browser-cache-and-cookies)
- [LSO キャッシュのクリア ](#clearing-lsos-cache)


## ブラウザーのキャッシュと Cookie のクリア {#clearing-the-browser-cache-and-cookies}

ブラウザーに依存しますが、Firefox では「ツール」 – \> 「最近の履歴をクリア」 – \> 「クリアする時間範囲」で「すべて」を選択し、「詳細」で「Cookie」と「キャッシュ」 – \> を確認して、「今すぐクリア」をクリックします。


## LSO キャッシュのクリア {#clearing-lsos-cache}

[Flash Player ヘルプ ](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html) にアクセスします。

（テスト対象に応じて） ```entitlement.\*``` を選択し、[ ウェブサイトを削除 ] をクリックします。


## デバッグツール {#tools}

Adobe Pass認証エンジニアは、次のデバッグツールを使用します。

- Firebug - <http://www.getfirebug.com/>
- Flashbug （Flash Player のデバッグバージョンで動作）
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>
