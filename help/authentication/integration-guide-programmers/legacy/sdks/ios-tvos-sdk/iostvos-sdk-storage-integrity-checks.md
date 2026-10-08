---
title: iOS/tvOS ストレージの整合性チェック機能
description: iOS/tvOSの整合性チェック機能
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%
---
# （レガシー） iOS/tvOSの整合性チェック機能 {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#Intro}

IOS/tvOS AccessEnabler SDKのバージョン 3.8.3以降では、AccessEnablerの初期化時にストレージの整合性チェックを実行するオプションを使用できます。

このメカニズムを使用するために、APIはAccessEnabler クラスの追加の初期化メソッドで拡張されました。

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## 整合性チェック {#Checks}

ストレージ整合性チェックは、AccessEnabler ストレージの破損が疑われる場合（読み取り/書き込みストレージ操作中に競合状態が発生した場合など）に役立ちます。

AccessEnablerの初期化で実行できるチェックは次のとおりです。
- ストレージの操作性：読み取りおよび書き込み操作の成功を確認します
- 格納された値の整合性：すべての値が有効であり、想定される形式であることを確認します

>[!IMPORTANT]
> 
>いずれかのチェックが失敗した場合、ストレージ内のすべての値がクリアされ、ユーザーがログアウトされ、ユーザーエクスペリエンスが低下する可能性があります。 必要と判断された場合にのみ、ストレージの整合性チェックを使用します。


## デフォルトの動作 {#Default}

デフォルトの初期化メソッドを使用してAccessEnablerを初期化する場合、ストレージ整合性チェックはデフォルトでオフになります。

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

AccessEnablerの初期化で実行するストレージ整合性チェックを明示的に指定するには、次の初期化メソッドを使用します。

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

IntegrityCheckType列挙は、クライアントアプリケーションに公開され、次の値を持ちます。

| 値 | 実行されたチェック | ストレージがクリアされました | 説明 | 推奨ユースケース |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | なし | なし | ストレージの初期化時に整合性チェックが実行されない | SDK フローが期待どおりに機能している場合 |
| INTEGRITY_CHECK_ALL | ストレージの操作性<br/>保存された値の有効性 | オン チェックが失敗しました | 利用可能なすべての整合性チェックは、ストレージの初期化に対して実行されます | SDK ストレージの破損が疑われる場合。<br/> 整合性チェックのいずれかが失敗した場合、ユーザーはログアウトされます |
| INTEGRITY_CHECK_CLEAR | なし | 常に | ストレージの初期化時にストレージがクリアされる | SDK フローを期待どおりに完了できない場合 |
