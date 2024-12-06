---
title: MVPD が選択ダイアログに表示されないようにする
description: MVPD が選択ダイアログに表示されないようにする
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---

# MVPD が選択ダイアログに表示されないようにする

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 問題 {#issue-prevent-mvpd-sel-dialog}

MVPD セレクターに（「ブロックリスト」）特定の MVPD が表示されないようにする必要があります。


## 解決策 {#solution-prevent-mvpd-sel-dialog}

これを解決するには、`displayProviderDialog()` が呼び出されたときにブロックリストへの登録を行います。

たとえば、CableCompany_1 と CableCompany_2 を MVPD セレクタ内に表示しないようにするには、次の例のように操作します。

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```

<!--
**Related Information**

* [Allow MVPDs in the Selection Dialog](/help/authentication/allow-mvpd-selectn-dialog.md)
* **Code samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
