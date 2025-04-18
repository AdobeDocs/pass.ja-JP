---
title: 証明書に関する Q&A
description: 証明書に関する Q&A
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# （レガシー）証明書に関する Q&amp;A {#certificates-q}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>

**Q1:** iOSとAndroidをまたいで証明書を登録することは可能ですか？

**A:** iOSとAndroidの証明書は現在の設定で同じです。 ネイティブ証明書は、両方のプラットフォームで使用されます。

</br>

**Q2:** 実稼動環境とステージング環境で、同じiOS証明書をプライマリ証明書およびバックアップ証明書として使用できますか？ 推奨されない場合は、説明してください。

**A:** プライマリ証明書とバックアップ証明書の両方と同じ証明書を設定する必要はありません。 プライマリ証明書とバックアップ証明書の概念があります。これにより、プライマリ証明書の有効期限が切れたり失効したりした場合に、プログラマー用に複数の証明書を設定できます。 バックアップ証明書があれば、プログラマーはリリースプライマリに影響を与えることなく、環境を変えることができます。 ただし、実稼働プロファイルとステージングプロファイルの両方に同じプライマリ証明書とバックアップ証明書のセットを使用できます。

</br>

**Q3:** 新しい柔軟な TempPass を使用する web ページには、新しい証明書が必要ですか？

**A:** 証明書（および実際の証明書）は、メディア会社およびプログラマーレベルで設定されます。 FlexibleTempPass はMVPDであるため、証明書を設定する必要はありません。そのため、既存のプログラマーを Flexible TempPass に統合する場合は、プログラマー/メディア会社レベルで既に設定されている証明書が使用されます。
