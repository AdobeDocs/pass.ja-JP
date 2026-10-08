---
title: 選択ダイアログにMVPDが表示されないようにする
description: 選択ダイアログにMVPDが表示されないようにする
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%
---
# （レガシー）選択ダイアログにMVPDが表示されないようにする

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## イシュー {#issue-prevent-mvpd-sel-dialog}

MVPD セレクターに特定のMVPDが表示されないようにする必要があります（「ブロックリスト」）。


## Solution {#solution-prevent-mvpd-sel-dialog}

解決策は、`displayProviderDialog()`が呼び出されたときにブロックリストを作成することです。

例えば、CableCompany_1とCableCompany_2をMVPD セレクタ内に表示しない場合は、次の例に示すような操作を行います。

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
