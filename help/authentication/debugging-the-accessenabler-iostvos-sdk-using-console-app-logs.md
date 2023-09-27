---
title: コンソールアプリログを使用した AccessEnabler iOS/tvOS SDK のデバッグ
description: コンソールアプリログを使用した AccessEnabler iOS/tvOS SDK のデバッグ
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# コンソールアプリログを使用した AccessEnabler iOS/tvOS SDK のデバッグ {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


## 概要

このドキュメントの範囲は、AccessEnabler のiOS/tvOS SDK のログメカニズムの進化を、コンソールアプリログを使用した AccessEnabler フレームワークのデバッグに役立つ詳細と共にキャプチャし、提示することです。

## ログメカニズムの状態

AccessEnabler iOS/tvOS のログメカニズムの目的は、AccessEnabler フレームワークを使用するアプリケーションで発生する可能性のある問題のトラブルシューティングに役立つメッセージを表示することです。

### AccessEnabler iOS/tvOS 3.5.0 以降

AccessEnabler iOS/tvOS 3.5.0 バージョン以降、ログメカニズムには次の変更が加えられました。

* AccessEnabler フレームワークでは、Appleを推奨 [OSLog](https://developer.apple.com/documentation/os/oslog) 実装。

* AccessEnabler フレームワークには、サブシステムに基づいてコンソールアプリケーションのログをフィルタリングする機能が導入されています。 **com.adobe.pass.AccessEnabler**. SDK から送信されるすべてのメッセージは、com.adobe.pass.AccessEnabler に含まれています。

* AccessEnabler フレームワークには、任意（プレフィックス）に基づいてコンソールアプリケーションのログをフィルタリングする機能が導入されています。 **[AccessEnabler]**. SDK から送信されるすべてのメッセージには、「 」というプレフィックスが付きます。 [AccessEnabler].

* AccessEnabler フレームワークには、カテゴリに基づいてコンソールアプリケーションのログをフィルタリングする機能が導入されています。 **デバッグ**, **エラー** 上記の 2 つの条件のいずれかと組み合わせて、 Subsystem または Any （プレフィックス）を指定します。

## コンソールアプリログを使用したデバッグ

調査された問題に応じて、AccessEnabler フレームワークから発行されるログメッセージを含めたり除外したりできます。以下に、調査中やコンソールアプリログを使用する際に役立つ便利な詳細を示します。


### AccessEnabler iOS/tvOS 3.5.0 以降

#### 次を含む {#including}

まず、AccessEnabler フレームワークから発行されたログ・メッセージを表示するために、まず、 **必須** コンソールアプリの「アクション」セクションで、「情報メッセージを含める」と「デバッグメッセージを含める」を選択します（下図を参照）。

![](assets/include-info-debug-msg.png)


AccessEnabler iOS/tvOS SDK の機能をデバッグするには、 **参照** AccessEnabler フレームワークのログでは、次の操作を実行できます。

* 次を使用してコンソールアプリで検索 **サブシステム** 以下の図のように、 com.adobe.pass.AccessEnabler の値に等しいオプション。

![](assets/subsys-console-app.png)

* 次を使用してコンソールアプリで検索 **任意** オプション (
  [AccessEnabler] の値は、以下の画像のようになります。

![](assets/any-optn-console-app.png)

上記の 2 つの条件と共に、 **カテゴリ** ～に伴う選択肢 **サブシステム** または **任意（プレフィックス）** 明示的に検索するには **デバッグ** または **エラー** AccessEnabler iOS/tvOS SDK が発行するメッセージのレベルを示します。

#### 除外

他のコンポーネントの機能をより適切にデバッグできるようにするため、および **除外** AccessEnabler フレームワークのログでは、次の操作を実行できます。

* 次を使用してコンソールアプリで検索 **サブシステム** com.adobe.pass.AccessEnabler の値と等しくないオプション。
* 次を使用してコンソールアプリで検索 **任意** オプション ( [AccessEnabler] の値です。

## 問題のレポート

Adobe Pass Authentication に関する問題を報告する際は、次の推奨事項を考慮してください。

* 再生手順を指定してください。
* 問題が発生した OS のバージョンとデバイスのモデルを指定してください。
* 問題が発生している AccessEnabler iOS/tvOS SDK のバージョンを指定してください。
* すべての AccessEnabler iOS/tvOS SDK ログメッセージを、 [次を含む](#including) 」セクションに入力します。
