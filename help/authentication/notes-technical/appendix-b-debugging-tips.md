---
title: 付録 B 「デバッグのヒント」
description: 付録 B 「デバッグのヒント」
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# 付録 B: デバッグのヒント {#appendix-b-debugging-tips}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。


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


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
