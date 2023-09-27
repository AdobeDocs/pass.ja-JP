---
title: アカウント IQ の概要
description: アカウント IQ は、MVPD やプログラマーが収益や事業運営に対するリスクを把握し、秘密鍵証明書の不正の影響を軽減するために最も効果的な対策を決定するのに役立ちます。
exl-id: c0d85fc8-b5ab-4284-802e-82f52eff401f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---

# [!UICONTROL Account IQ] 概要 {#account-iq-overview}

ストリーミングサービス購読者による資格情報の共有は、業界にとって大きな問題であり、ますます大きくなっています。 さらに、資格情報の共有に関する理解、識別、操作を行うことは複雑なプロセスです。 購読者の使用状況の把握や、同じ世帯内と外部のメンバー間での共有を区別するなど、アクティビティの全体像の開発には複雑さが伴います。 この課題により、ストリーミングサービスプロバイダは、資格情報の共有に対して行動を妨げる可能性があります。


<div class "preview">
一般に、ビデオストリーミングサービスプロバイダーは、ビジネスへの共有のリスクとコストを把握しますが、共有者をブロックしたりオファーを提供したりするなど、修正方法は限られています。 しかし、情報に基づいたターゲット化されたアプローチを推奨します。つまり、サービスが正確に共有を理解し、優れた閲覧行動に対する報酬を与え、同時にビジネスの成長をターゲットにする戦略を導入できます。 </span>

![アカウント IQ のフロー図](assets/aiq-intro.png)

*図：アカウント IQ 情報フロー*

Adobe Pass [!UICONTROL Account IQ] ビデオストリーミングサービスで、購読者の使用パターンを理解し、秘密鍵証明書の共有を識別できます。 ストリーミングサービスは、Adobe独自のマルチレイヤー機械学習モデルを使用して、各加入者が残したデータの長い軌跡を深く分析することで、使用状況を把握し、より確実に秘密鍵証明書の共有を識別できます。 また、同時ストリームの制限やオファーのカスタマイズなど、他のシステムとの統合を通じてアクションを実行でき、正当な表示行動を促すか購読者や売上高の増加に関わらず、それらのアクションの影響を検証できます。

[!UICONTROL Account IQ] は、資格情報の共有を測定、管理および収益化するためのツールと機能を提供します。 レポート、分析、ダッシュボードを使用してデータを調査し、パターンを識別できます。 ダイレクトアクションは、Adobeやサードパーティシステム（キャンペーン管理、通貨制限、購読者登録など）とのエクスポートや統合を通じてサポートされます。 また、専用のトラッキングツールがこれらのアクションの成功を測定し、更新や拡張が可能です。

[!UICONTROL Account IQ] アプリケーションツールと機能については、次の節で説明します。

* [ダッシュボード](/help/accountiq/dashboard.md)
* [一般使用状況レポート](/help/accountiq/general-usage-reports.md)
* [共有アカウントレポート](/help/accountiq/shared-acc-reports.md)
* [使用パターン](/help/accountiq/usage-patterns.md)
* [運用](/help/accountiq/operations.md)

次に、各セクションのグラフとレポートについて詳しく説明します。

>[!MORELIKETHIS]
>
>* [アカウント IQ の使用を開始する方法](/help/accountiq/get-started.md)
>* [ダッシュボード](/help/accountiq/dashboard.md)
>* [一般使用状況レポート](/help/accountiq/general-usage-reports.md)
>* [共有アカウントレポート](/help/accountiq/shared-acc-reports.md)
>* [使用パターン](/help/accountiq/usage-patterns.md)
>* [製品用語集](/help/accountiq/product-concepts.md)
>* [アカウント IQ に関するホワイトペーパー](https://www.adobe.com/content/dam/dx/us/en/products/primetime/resources/primetime-account-iq-whitepaper.pdf)

<!-- Credential sharing is rampant and prevalent among subscribers in the video streaming industry. To add to it, understanding, identifying, and acting on password sharing is a complex process. There is complexity involved in understanding the subscriber usage behavior and developing a holistic view of viewer activity—for example, distinguishing sharing among members within the same household and outside. Due to this challenge, streaming service providers have inhibitions in acting against password sharing.

Generally, video streaming service providers consider password sharing as fatal for business and act strongly against it, by blocking the sharers. However, it is advised to follow a holistic approach that enables them to understand sharing accurately and adopt strategies to reward good viewing behavior and target business growth simultaneously.

![Account IQ flow diagram](assets/aiq-intro.png)

*Figure: Account IQ information flow*

Adobe Pass Account IQ enables video streaming services understand the subscriber usage patterns and identify password sharing by analyzing usage behavior. Moreover, it validates the impact of applying actions to encourage legitimate viewing behavior while maximizing business ROI, eventually growing subscribers and revenue.

By deeply analyzing the long, winding trail of data left behind by each subscriber using Adobe's proprietary multi-layer machine learning model, customers can understand usage behavior and identify password sharing with a greater degree of certainty, use the insights to validate the impact of applying actions to encourage legitimate viewing behavior while maximizing business growth, eventually act on password sharing using validated tactics to improve viewer experience, growing subscribers and revenue (for e.g. converting sharers to paid subscribers, managing ad loads based on sharing behavior, rewarding good behavior with better viewer experience).

Account IQ is helps you understand usage patterns and identify password sharing by leveraging the Adobe Pass Authentication  solution that processes a huge volume of TV Everywhere transactions. A proprietary multi-layer machine learning model trained by this real-world TVE data accurately characterizes usage patterns and helps video streaming services understand usage patterns and identify password sharing at an individual account level. Based on Adobe's customer experience management solutions, Account IQ enables video streaming services to effectively use their audience data to create actionable sharing profiles as well powers integrations with other Adobe Digital Experience and 3rd party solutions—for example, Adobe Pass Concurrency Monitoring or Adobe Analytics—to enable understanding usage patterns, identify and act upon password sharing.


<!-- The widespread availability of video content and streaming services bring with it problem of account sharing; eventually leading to the loss of revenue by content providers. Account IQ helps TV Everywhere and VOD (video on demand) providers understand the risks to their revenue and business operations, and determine the most effective actions to take to mitigate the impacts of credential fraud. It helps these media companies (MVPDs, Programmers, and VOD providers) manage and uncover the instances of password sharing with a high level of confidence, enabling them deliver better business outcomes and provide better viewing experiences for subscribers.

To help media companies better understand the password sharing within their businesses, Adobe Pass Account IQ determines **Password Sharing Risk Index** that rates every subscriber on their likelihood of sharing account credentials for subscription passwords, from very low to very high. Based on these calculations and the resulting indices, analytics are performed and visuals are generated for better understanding and interpretation of the account sharing behavior. Account IQ is a hosted web application, which you can access using your browser.

Account IQ assigns sharing scores to different subscriber accounts, so that the content providers (media companies, programmers, MVPDs, and VOD providers) can take informed decisions about subscriber accounts and check the illicit sharing.

Passwords are the main methods for viewers to authenticate, and there is a misconception that credential sharing is allowed. This idea makes illicit password sharing a common practice; necessitating the need for media companies to educate their viewers about permissible sharing and prevent illicit sharing.-->
