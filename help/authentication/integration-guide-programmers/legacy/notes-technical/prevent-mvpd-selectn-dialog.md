---
title: MVPD が選択ダイアログに表示されないようにする
description: MVPD が選択ダイアログに表示されないようにする
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# （レガシー） MVPD が選択ダイアログに表示されないようにする

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 問題 {#issue-prevent-mvpd-sel-dialog}

（「ブロックリスト」）特定の MVPD がMVPD セレクターに表示されないようにする必要があります。


## 解決策 {#solution-prevent-mvpd-sel-dialog}

これを解決するには、`displayProviderDialog()` が呼び出されたときにブロックリストへの登録を行います。

例えば、CableCompany_1 と CableCompany_2 がMVPD セレクター内に表示されないようにするには、次の例のように操作します。

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
