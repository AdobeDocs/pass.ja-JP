---
title: コンソールアプリログを使用した AccessEnabler iOS/tvOS SDKのデバッグ
description: コンソールアプリログを使用した AccessEnabler iOS/tvOS SDKのデバッグ
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 1%

---

# （レガシー）コンソールアプリログを使用した AccessEnabler iOS/tvOS SDKのデバッグ {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要

このドキュメントでは、コンソール・アプリケーション・ログを使用して AccessEnabler フレームワークをデバッグする際に役立つ詳細情報とともに、AccessEnabler のiOS/tvOSSDKログ・メカニズムの進化を取り上げて説明します。

## ロギング メカニズムの状態

AccessEnabler iOS/tvOS のログ メカニズムの目的は、AccessEnabler フレームワークを使用するアプリケーションが原因で発生する可能性のある問題のトラブルシューティングに役立つメッセージを出力することです。

### AccessEnabler iOS/tvOS 3.5.0 以降

AccessEnabler iOS/tvOS 3.5.0 以降では、ログ機能に次の変更が加えられています。

* AccessEnabler フレームワークでは、Appleの推奨 [OSLog](https://developer.apple.com/documentation/os/oslog) 実装が使用されます。

* AccessEnabler フレームワークでは、サブシステム **com.adobe.pass.AccessEnabler** に基づいてコンソール・アプリケーション・ログをフィルタリングする機能が導入されています。 SDKから送信されるすべてのメッセージは、com.adobe.pass.AccessEnabler の一部です。

* AccessEnabler フレームワークでは、任意（プレフィックス）の **[AccessEnabler]** に基づいてコンソール・アプリケーション・ログをフィルタリングする機能が導入されています。 SDKから送信されるすべてのメッセージには、先頭に [AccessEnabler] が付きます。

* AccessEnabler フレームワークでは、Subsystem または Any （プレフィックス）のいずれか 2 つの条件と組み合わせて、カテゴリ：**debug**、**error** に基づいてコンソール・アプリケーション・ログをフィルタリングする機能が導入されています。

## コンソールアプリログを使用したデバッグ

調査対象の問題に応じて、AccessEnabler フレームワークが発行するログ・メッセージを含めるか除外することができます。したがって、調査時やコンソール・アプリケーション・ログ使用時に役立つ以下の役立つ詳細情報を参照できます。


### AccessEnabler iOS/tvOS 3.5.0 以降

#### 含む {#including}

まず、AccessEnabler フレームワークから出力されるログ・メッセージを確認するには、次の図に示すように **コンソール・アプリのアクション・セクションで** Include Info Messages」と「Include Debug Messages」を選択します。

![](../../../assets/include-info-debug-msg.png)


AccessEnabler iOS/tvOS SDKの機能をデバッグするには、AccessEnabler フレームワーク ログを **参照** して、次の操作を実行します。

* 以下の画像に示すように、「**サブシステム**」オプションを使用して、コンソールアプリを検索します。このオプションの値は com.adobe.pass.AccessEnabler の値と等しくなります。

![](../../../assets/subsys-console-app.png)

* 次を含む **Any** オプションを使用して、コンソールアプリを検索します
  [AccessEnabler] の値を以下の図に示します。

![](../../../assets/any-optn-console-app.png)

上記の 2 つの条件に加えて、**Subsystem** または **Any （プレフィックス** と組み合わせて **Category** オプションを使用し、AccessEnabler iOS/tvOS SDKが発行する **debug** または **error** レベルのメッセージを明示的に検索することもできます。

#### 除外中

他のコンポーネントの機能をより適切にデバッグしたり、AccessEnabler フレームワークのログを **除外** したりするには、次の操作を行います。

* com.adobe.pass.AccessEnabler の値と等しくない **Subsystem** オプションを使用して、コンソールアプリを検索します。
* [AccessEnabler] 値を含まない **Any** オプションを使用して、コンソール・アプリ内を検索します。

## 問題のレポート

Adobe Pass Authentication に問題を報告する際は、次の提案を考慮してください。

* 再現手順を指定してください。
* 問題が発生した OS のバージョンとデバイスのモデルを指定してください。
* この問題が発生している AccessEnabler iOS/tvOS SDKのバージョンを指定してください。
* [ 含む ](#including) セクションに示されている 2 つのオプションのいずれかを使用して、すべての AccessEnabler iOS/tvOS SDK ログ メッセージをキャプチャし、添付してください。
