---
title: 選択ダイアログで MVPD を許可
description: 選択ダイアログで MVPD を許可
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# 選択ダイアログで MVPD を許可 {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 問題 {#issue}

プログラマーは、エンドユーザーに公開する前に、新しい MVPD 統合のユーザーエクスペリエンスをテストまたは確認したい場合があります。

## 解決策 {#solution}

Adobe Analytics の `displayProviderDialog()` コールバックを返すと、Adobe Pass認証は、選択したプログラマー（要求者 ID）と統合されたすべての MVPD を返します。 しかし、プログラマは MVPDs の戻り配列にフィルタを適用し、両方のリストにあるものだけを表示することができます。

## 例 {#example}

この例では、MVPD セレクタダイアログ内に CableCompany_1 と CableCompany_2 のみを表示し、CableCompany_NewIntegration を表示しない方法を示します。

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
