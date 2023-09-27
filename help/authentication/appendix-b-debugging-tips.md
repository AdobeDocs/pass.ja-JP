---
title: 付録 B「デバッグのヒント」
description: 付録 B「デバッグのヒント」
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 付録 B：デバッグのヒント {#appendix-b-debugging-tips}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


## 一時データをクリア中 {#clearing-temporary-data}

Adobe Pass認証は、一時データ（ブラウザーキャッシュ、LSO キャッシュ、cookie など）を保存します。 テスト時に新しいスレートが得られるように、一時データをクリアすることが重要です。

- [ブラウザーのキャッシュと cookie のクリア](#clearing-the-browser-cache-and-cookies)
- [LSO キャッシュのクリア](#clearing-lsos-cache)


## ブラウザーのキャッシュと cookie のクリア {#clearing-the-browser-cache-and-cookies}

ブラウザーは信頼できますが、Firefox では「ツール」 —\>「最近の履歴を消去」 —\>「クリアする時間範囲：」で「すべて」を選択し、「詳細」で「Cookie」と「キャッシュ」 —\>「今すぐクリア」をクリックします。


## LSO キャッシュのクリア {#clearing-lsos-cache}

次にアクセス： [Flash Playerヘルプ](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

を選択します。 ```entitlement.\*``` （テストの内容に応じて）、「web サイトを削除」をクリックします。


## デバッグツール {#tools}

Adobe Pass認証エンジニアは、次のデバッグツールを使用します。

- Firebug - <http://www.getfirebug.com/>
- Flashbug（Flash Player のデバッグバージョンで動作）
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
