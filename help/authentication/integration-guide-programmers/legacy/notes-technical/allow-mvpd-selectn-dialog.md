---
title: 選択ダイアログでのMVPDの許可
description: 選択ダイアログでのMVPDの許可
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%
---
# （レガシー）選択ダイアログでMVPDを許可する {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## イシュー {#issue}

プログラマーは、エンドユーザーに公開する前に、新しいMVPD統合のユーザーエクスペリエンスをテストまたは確認したい場合があります。

## Solution {#solution}

`displayProviderDialog()` コールバックで、Adobe Pass Authenticationは、選択したプログラマー（依頼者ID）と統合されたすべてのMVPDを返します。 しかし、プログラマはMVPDの戻り配列にフィルターを適用し、両方のリストにある配列のみを表示できます。

## 例 {#example}

この例では、MVPD セレクターダイアログ内でCableCompany_1とCableCompany_2のみを表示し、CableCompany_NewIntegrationを表示しない方法を示します。

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
