---
title: iOS/tvOS のストレージの整合性チェックメカニズム
description: iOS/tvOS の整合性チェックメカニズム
source-git-commit: 444a81ad18afcb26dcf3ae8b41f4d02c465f4655
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# iOS/tvOS の整合性チェックメカニズム {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに {#Intro}

iOS/tvOS AccessEnabler SDK のバージョン 3.8.3 以降では、AccessEnabler の初期化時に、ストレージの整合性チェックを実行するオプションを使用できます。

このメカニズムを使用するために、AccessEnabler クラスの追加の初期化メソッドで API を拡張しました。

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## 整合性チェック {#Checks}

ストレージの整合性チェックは、AccessEnabler ストレージの破損が疑われる場合（読み取り/書き込みストレージ操作中に競合状態が発生する場合など）に役立ちます。

AccessEnabler の初期化時に、次のチェックを実行できます。
- ストレージの操作性：読み取り/書き込み操作の成功を確認
- 格納された値の整合性：すべての値が有効で、期待された形式であることを確認します。

>[!IMPORTANT]
> 
>いずれかのチェックに失敗した場合、ストレージ内のすべての値がクリアされ、ユーザーがログアウトされるので、ユーザーエクスペリエンスが悪くなる可能性があります。 ストレージの整合性チェックは、必要に応じてのみ使用してください。


## デフォルトの動作 {#Default}

デフォルトの初期化方法を使用して AccessEnabler を初期化すると、ストレージの整合性チェックはデフォルトでオフになります。

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

AccessEnabler の初期化時に実行するストレージの整合性チェックを明示的に指定するには、次の初期化方法を使用します。

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

IntegrityCheckType 列挙は、クライアントアプリケーションに公開され、次の値を持ちます。

| 値 | 実行されたチェック | 消去されたストレージ | 説明 | 推奨される使用例 |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | なし | なし | ストレージの初期化時に整合性チェックは実行されません | SDK フローが期待どおりに動作している場合 |
| INTEGRITY_CHECK_ALL | ストレージの操作性 <br/> 保存された値の有効性 | チェックに失敗した場合 | ストレージの初期化時に、使用可能なすべての整合性チェックが実行されます。 | SDK ストレージの破損が疑われる場合。 <br/> 整合性チェックが失敗した場合、ユーザーはログアウトされます |
| INTEGRITY_CHECK_CLEAR | なし | 常に | ストレージの初期化時にストレージがクリアされます | SDK フローを期待どおりに完了できない場合 |
