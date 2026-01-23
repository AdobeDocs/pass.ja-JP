---
title: iOS/tvOS ストレージの整合性チェックメカニズム
description: iOS/tvOS の整合性チェックメカニズム
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 0%

---

# （従来の）iOS/tvOS の整合性チェックメカニズム {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#Intro}

iOS/tvOS AccessEnabler SDKのバージョン 3.8.3 以降では、AccessEnabler の初期化でストレージの整合性チェックを実行するオプションが使用可能です。

このメカニズムを使用するために、AccessEnabler クラス用の追加の初期化メソッドを使用して API が拡張されました。

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## 整合性チェック {#Checks}

ストレージの整合性チェックは、AccessEnabler ストレージの破損が疑われる場合（ストレージの読み取り/書き込みオペレーション中に競合状態が発生する場合など）に役立ちます。

AccessEnabler の初期化時には、次のチェックを実行できます。
- ストレージの操作性：読み取り/書き込み処理の成功を確認
- 格納された値の整合性：すべての値が有効で、期待される形式であることをチェックします

>[!IMPORTANT]
> 
>いずれかのチェックが失敗した場合、ストレージ内のすべての値がクリアされ、ユーザーがログアウトされるので、ユーザーエクスペリエンスが悪くなる可能性があります。 ストレージの整合性チェックは、必要な場合にのみ使用してください。


## デフォルトの動作 {#Default}

デフォルトの初期化方法を使用して AccessEnabler を初期化すると、ストレージ整合性チェックがデフォルトでオフになります。

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

AccessEnabler の初期化時に実行するストレージ整合性チェックを明示的に指定するには、次の初期化方法を使用します。

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

IntegrityCheckType 列挙は、クライアントアプリケーションに公開され、次の値が含まれています。

| 値 | 実行されたチェック | ストレージがクリアされました | 説明 | 推奨されるユースケース |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | なし | なし | ストレージの初期化時に整合性チェックが実行されない | SDK フローが期待どおりに動作している場合 |
| INTEGRITY_CHECK_ALL | ストレージの操作性 <br/> 保存された値の有効性 | チェック失敗時 | 使用可能なすべての整合性チェックは、ストレージの初期化時に実行されます | SDK ストレージの破損が疑われる場合。<br/> 整合性チェックのいずれかが失敗した場合、ユーザーはログアウトされます |
| INTEGRITY_CHECK_CLEAR | なし | Always | ストレージの初期化時にストレージがクリアされる | SDK フローを期待どおりに完了できない場合 |
