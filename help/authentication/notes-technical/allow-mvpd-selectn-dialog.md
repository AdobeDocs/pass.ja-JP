---
title: 選択ダイアログの MVPD を許可
description: 選択ダイアログの MVPD を許可
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%

---

# 選択ダイアログの MVPD を許可 {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 問題 {#issue}

プログラマーは、エンドユーザーに公開する前に、新しい MVPD 統合のユーザーエクスペリエンスをテストまたは確認する必要があります。

## 解決策 {#solution}

`displayProviderDialog()` コールバックで、Adobe Pass認証は、選択したプログラマー（リクエスター ID）と統合されているすべての MVPD を返します。 ただし、プログラマは MVPD の戻り値の配列にフィルタを適用し、両方のリストに含まれる配列のみを表示できます。

## 例 {#example}

この例では、MVPD 選択ダイアログ内で CableCompany_1 と CableCompany_2 のみを表示し、CableCompany_NewIntegration を表示しない方法を示します。

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
