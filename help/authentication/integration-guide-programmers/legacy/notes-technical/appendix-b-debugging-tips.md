---
title: 付録B 「デバッグのヒント」
description: 付録B 「デバッグのヒント」
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: b6ba687240799d1889302019613f426259f147ad
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# （レガシー）付録B: デバッグのヒント {#appendix-b-debugging-tips}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 一時データの消去 {#clearing-temporary-data}

Adobe Pass認証は、ブラウザーキャッシュ、LSO キャッシュ、Cookieなどの一時データを保存します。 テスト時にクリーンなスレートを確実に取得するためには、一時データを消去することが重要です。

- [ブラウザーのキャッシュとCookieの消去](#clearing-the-browser-cache-and-cookies)
- [LSO キャッシュのクリア &#x200B;](#clearing-lsos-cache)


## ブラウザーのキャッシュとCookieの消去 {#clearing-the-browser-cache-and-cookies}

ブラウザーは信頼できますが、Firefoxでは「ツール」 -\> 「最近の履歴を消去」 -\> 「消去する時間範囲」で「すべてを」を選択し、「詳細」で「Cookie」と「キャッシュ」を確認します – \> 「今すぐ消去」をクリックします。


## LSO キャッシュのクリア {#clearing-lsos-cache}

[Flash Player ヘルプ &#x200B;](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html)にアクセスします。

「`entitlement.\*`」を選択し、「Web サイトを削除」をクリックします。


## デバッグツール {#tools}

Adobe Pass認証エンジニアは、次のデバッグツールを使用します。

- Firebug - <http://www.getfirebug.com/>
- flashbug （Flash Playerのデバッグバージョンで動作）
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- チャールズ - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>

