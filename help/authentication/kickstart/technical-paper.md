---
title: Adobe Pass認証について
description: Adobe Pass認証について
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Adobe® パス認証について {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

## すべてのテレビについて {#about-tv-everywhere}

今日のテレビ視聴者は、いつでもどこでも有料のテレビコンテンツにシームレスにアクセスすることを期待しています。 インターネットに接続されたデバイスの増加に伴い、オーディエンスは次のような幅広いプラットフォームでコンテンツを消費しています。

* ノート PC
* タブレット
* スマートフォン
* Web サイト
* 連合アプリ
* ゲームコンソール
* セットトップボックス
* スマート TV

TV Everywhere は、Pay TV の加入者が、自宅の内外の複数のデバイスで既に支払っているコンテンツにアクセスできるようにする業界イニシアチブです。

従来の（線形） TV は引き続き強力ですが、ビデオ消費で最も急速に成長しているセグメントは、時間シフトされたコンテンツ、オンラインストリーミング、代替画面です。 この移行により、ビデオ配信市場が混乱し、TV Everywhere が **プログラマー、ペイテレビプロバイダー、購読者** の関心を一致させる重要なソリューションになりました。

### あらゆる場所で利用できるテレビの目標 {#goals-tv-everywhere}

**技術的目標**

* 有料テレビのお客様は、購読しているコンテンツにすべてのデバイスやプラットフォームからシームレスにアクセスできます。

**事業目標**

* 既存のお客様との関係を維持および強化しつつ、新しいオポチュニティを可能にします。
* プログラマーとコンテンツ所有者が、より幅広いオーディエンスにリーチし、プレミアムコンテンツの価値を最大化できるようにします。
* 視聴者との直接のオンラインエンゲージメントを通じてブランドを拡張します。

### どこでも視聴できるテレビの課題 {#challenges-tv-everywhere}

TV Everywhere の機会には大きな課題があり、権利付与が最も重要です。 閲覧者が購読コンテンツにアクセスする前に、システムは使用権限を検証する必要があります。

主な質問は次のとおりです。

* ユーザーが有料 TV プロバイダーとアクティブな契約を結んでいますか？
* 要求されたコンテンツがサブスクリプションに含まれていますか？

有料テレビのオペレーターは顧客データとアクセス権限を制御するので、使用権限を決定することはプログラマーやコンテンツ所有者にとって特に困難です。

使用権以外にも、次のようないくつかの技術的および統合上の課題が発生します。

* シームレスなアクセスを保証するマルチデバイス戦略の開発。
* プログラマーとペイ TV プロバイダーの複雑な関係の管理。
* 不正アクセスの防止と利用規約の適用。
* Web サイトやアプリをまたいで、一貫性のある使いやすい認証エクスペリエンスを提供します。
* アフィリエイト契約に合わせて、市場投入までの時間を短縮する。
* 複数の統合に関連するコストの管理。

これらの課題により、プログラマーと複数の有料 TV 認証システムを直接統合する場合、リソースが非常に多くなり、時間と技術の両方の専門知識が必要になります。

これらの障害を克服するために、**Adobe® パス認証** は使用権限の検証を簡素化および効率化し、TV Everywhere コンテンツへのシームレスで安全なアクセスを可能にします。

## Adobe Pass認証の概要 {#introduction-adobe-pass-authentication}

Adobe Pass Authentication は、プログラマーと有料テレビプロバイダー間の権利取引を安全に仲介し、適切な顧客が適切なコンテンツに簡単にアクセスできるようにします。

![](../assets/programmers-connect-authn.png)

*Adobe Pass認証を通じて接続するプログラマーと有料テレビプロバイダーの一部*

**Adobe Pass認証のメリットを受けるユーザー**

* **プログラマー**

  MVPD （Multichannel Video Programming Distributors）としても知られている有料テレビプロバイダーと簡単に統合して、最も広い視聴者にリーチし、収益を最適化したい人。

* **有料テレビ事業者（MVPD）**

  1 つの統合を通じて複数のコンテンツ所有者（プログラマー）とつながり、オンラインの購読コンテンツへのアクセスを促進することで顧客満足度を向上させることを目指している顧客。

* **有料テレビ番組のお客様**

  いつでもどこでも、あらゆるデバイスで既に支払われているコンテンツを視聴したいユーザー。

**プログラマー**

オーディエンスのリーチと売上高の最大化を目指しているプログラマーは、次のことが可能です。

* 複数の直接接続を管理することなく、トップの有料テレビプロバイダーと簡単に統合できます。
* すべての主要なプロバイダーおよびプラットフォームで認証を確保することで、オーディエンスを拡大します。
* プレミアムコンテンツを安全な認証で保護し、許可されたユーザーやデバイスへのアクセスを制限します。
* シングルサインオン（SSO）を有効にして、アプリや web サイト間でのユーザーエクスペリエンスを向上させます。

**有料テレビ事業者（MVPD）**

有料テレビ事業者は、次の方法で顧客体験を向上させ、運用を合理化できます。

* 単一の統合による複数のコンテンツ所有者との接続。
* デバイスやプラットフォームを問わず、ブランド化されたシームレスな視聴エクスペリエンスを提供します。
* 不正アクセスを防ぎ、世帯ごとの同時ストリームを管理するための安全な認証を確保します。

**有料テレビ番組のお客様**

加入者はテレビの恩恵を受けます Everywhere、彼らはいつでもどこでも、あらゆるデバイスで既に支払ったコンテンツを見ることができます。

### Adobe Pass認証との統合 {#integrating-adobe-pass-authentication}

有料 TV プロバイダーでもプログラマーでも、Adobe Pass Authentication との統合には積極的な参加が必要です。 両方の役割のプロセスの概要を以下に示します。

#### プログラマ統合プロセス {#programmer-integration-process}

統合が正式に開始されると、追加のガイダンスを利用できますが、通常、プロセスには次の手順が含まれます。

**前提条件**

* コンテンツ管理システム（CMS）。
* サードパーティのコンテンツ配信ネットワーク（CDN）を含むコンテンツ配信メカニズム。
* メディアプレーヤーが web サイトまたはスタンドアロンアプリケーションに埋め込まれている既存のオンラインビデオプラットフォーム。

**統合タスク**

* Adobe Pass認証 [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) の統合
* Adobe Pass認証 [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md) の統合
* Adobe Pass認証 [&#x200B; メディアトークンベリファイア &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) を統合します。
* 認証、承認、ログアウトワークフロー用のユーザーインターフェイスを開発します。

[&#x200B; プログラマー統合プロセスの詳細については、「プログラマーのキックスタートガイド」と「プログラマー統合ガイド &#x200B;](/help/authentication/kickstart/programmer-kickstart-guide.md) の各ドキュメントを参照して [&#x200B; ださい &#x200B;](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)。

#### 有料テレビ プロバイダの統合プロセス {#pay-tv-provider-integration-process}

統合が正式に開始されると、追加のガイダンスを利用できますが、通常、プロセスには次の手順が含まれます。

**前提条件**

* Adobe Pass認証機密保持契約書（NDA）に署名します。
* Adobe Pass認証エンジニアリングチームに、認証および承認システムの仕様を提供します。 最も簡単に統合するには、認証用の SAML ベースの ID プロバイダー（IdP）とSOAP ベースの認証システムをお勧めします。

**統合タスク**

* Adobe Pass認証サーバーとの接続を確立します。
* ステージングリリースを完了し、品質保証を確認します。
* 実稼動リリースを完了し、品質保証を確保します。

標準統合は、ドキュメントや基本的な電子メールサポートを含む、有料テレビプロバイダーに無料で提供されます。 ただし、広範なサポートや迅速なタイムラインを必要とするプロバイダーには、サポート料金が発生する場合や、Synacor などの経験豊富なサードパーティパートナーと連携することを選択する場合があります。

Adobe Pass Authentication は、次の 2 つの主要な方法で、有料 TV プロバイダー固有のビジネスロジックを効率的にサポートできます。

* 認可リクエストを受信した際に有料 TV プロバイダーが自己完結型で適用するビジネスロジックの場合、Adobeは、適用をサポートするために必要なデータ（一意のデバイス ID、IP アドレスなど）を提供します。
* Adobeがユーザーの介入や特別な処理を必要とするビジネスロジックの場合、カスタムプロパティは有料 TV プロバイダーごとに維持管理できます。 これらの設定には、認証プロセスの特定のポイントでトリガーされる事前定義済みのワークフローが含まれる場合があります。

有料テレビ プロバイダーの統合プロセスの詳細については、[MVPD キックスタートガイド &#x200B;](/help/authentication/kickstart/mvpd-kickstart-guide.md) および [MVPD統合ガイド &#x200B;](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md) のドキュメントを参照してください。

### 使用権限フロー {#entitlement-flow}

Adobe Pass認証はプロキシとして機能し、プログラマーと MVPD の両方に安全で一貫性のあるインターフェイスを提供することで、両者の間の使用権限のフローを容易にします。

プログラマーの場合、Adobe Pass認証では、次のような API を **Standard** 層または **Premium** 層の一部として提供しています。

* 標準Adobe Pass認証 API:
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass認証 API:
   * [Temp Pass API をリセット](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass フィーチャ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [API の低下](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [縮退機能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [使用権限サービスモニタリング API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

使用権限フローについて詳しくは、「[&#x200B; プログラマー統合ガイド &#x200B;](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow) ドキュメントを参照してください。

#### 使用権限について {#understanding-entitlements}

Adobe Pass認証ソリューションでは、使用権限（認証および承認ワークフローが正常に完了すると生成される特定のデータ）の作成を中心に取り組んでいます。 これらの権限は、保護されたコンテンツへのアクセス権を付与しますが、存続期間が限られています。 使用権限の有効期限が切れたら、認証または承認プロセスを再開して、使用権限を更新する必要があります。

使用権限について詳しくは、次のドキュメントを参照してください。

* **プロファイル**

  認証に成功すると、Adobe Pass Authentication は、要求元のアプリケーション、デバイス、サービスプロバイダーの識別子（要求者識別子）に関連付けられた認証済みプロファイル（「長期間有効」）を作成します。

* **[ユーザーメタデータ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Adobe Pass認証は、認証に成功すると（場合によっては認証後も）、MVPDからユーザーメタデータを受け取り、要求元のアプリケーションに公開できます。

* **[決定](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Adobe Pass Authentication は、認証に成功すると、要求元のアプリケーション、デバイス、サービスプロバイダーの識別子（要求者識別子）および保護された特定のリソース（リソース識別子）に関連付けられた認証決定（「長期間有効」）を作成します。

* **[メディアトークン](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  認証に成功すると、Adobe Pass Authentication によって、成功した再生リクエストに関連付けられたメディアトークン（「短期間有効」）が作成され、不正を軽減するための業界のベストプラクティス（ストリームリッピングなど）のサポートが提供されます。

プロファイルと決定の有効期間（「TTL」）値は、関係するすべてのユーザーに最適な値に同意するプログラマーと有料テレビプロバイダーの契約に基づいて設定されます。

#### ユーザーインターフェイスの作成 {#building-user-interface}

プログラマーは、Web サイトまたはアプリ内のエンタイトルメントワークフローのユーザーインターフェイス（UI）を設計および実装する責任を負います。一方、ログインプロセスなどの特定の要素は有料テレビプロバイダーが処理します。

少なくとも、プログラマーは次の操作を行う必要があります。

* **プロバイダー選択インターフェイスの実装**
   * 新規ユーザーが有料テレビ事業者を特定し、初めてログインできるようにします。
   * 一部の有料テレビプロバイダーは、ユーザーを外部のログインページにリダイレクトしますが、iframe 内でのログインが必要なプロバイダーもあります。 プログラマーは、必要に応じて iframe を生成するコールバック関数を実装する必要があります。

* **サポートされている有料テレビ プロバイダの一覧を管理する**
   * ユーザーが、承認されたプロバイダーを通じてのみコンテンツにアクセスできることを確認します。

* **認証ステータスの表示**
   * アプリまたは web サイト内でユーザーが認証された際に表示します。

* **保護されたリソースの特定**
   * 表示する前に、認証が必要なコンテンツを明確に示します。
   * アクセス権が付与された後、正常に認証されたことを反映するように UI を更新します。

## FAQ {#faqs}

**テレビはどこでも何ですか？**

TV Everywhere は、Pay TV のお客様が、様々なインターネット接続デバイスで既に購読しているプレミアムコンテンツにアクセスできるようにする業界イニシアチブです。 これには、パソコン、タブレット、スマートフォン、ゲーム機、セットトップボックス、スマートテレビなどが含まれます。 主な課題は、シームレスで使いやすい認証プロセスを確保し、お客様が複数のログインや技術的な障壁なしに購読コンテンツにアクセスできるようにすることです。

**Adobe Pass Authentication とは何ですか？また、TV Everywhere はどのようにサポートされますか？**

Adobe Pass Authentication は、シンプルかつ効率的な方法で、コンテンツに対するユーザーの使用権限を安全に検証することにより、TV Everywhere に生命をもたらします。 これは、プログラマーとペイ TV プロバイダーの両方が設定したビジネスルールに基づいて、迅速なバックエンド統合を容易にするホストサービスです。 これにより、すべての関係者が市場投入までの時間を短縮し、不正を最小限に抑えるより安全な環境を実現し、ユーザーエクスペリエンスを向上させ、複数のプラットフォームからより多くの TV コンテンツにアクセスできるようになります。

**Adobe Pass認証はどのように提供されますか？**

Adobe Pass Authentication は、Software as a Service （SaaS）ソリューションとして提供されます。 このアプローチにより、エンドユーザー、プログラマー、有料テレビプロバイダー間の安全な通信が確保され、コンテンツの権利付与の検証が行われます。

**Adobe Pass認証が他の TV Everywhere ソリューションと異なる理由は何ですか？**

Adobe Pass認証には、代替ソリューションに比べて次のような利点があります。

* **シームレスなシングルサインオン（SSO）** – 個々のプロバイダーと直接統合する場合とは異なり、Adobe Pass認証は、ユーザーが異なる web サイトやアプリを移動しても永続的なログインエクスペリエンスを可能にします。
* **幅広い市場浸透** - プログラマーはAdobe Pass Authentication と統合すると、米国の世帯の 90% 以上をカバーする有料テレビ事業者にすぐにアクセスできるようになります。
* **Adobeのエコシステムとの統合** - Adobe Analyticsを含む他のAdobe ソリューションとシームレスに連携して、コンテンツ配信、保護および収益化を行います。

**Adobe Pass認証のセキュリティはどの程度ですか？**

セキュリティは最優先事項です。 Adobe Pass認証を使用すると、ユーザーのデバイスへのアクセスをバインドして、許可されたユーザーのみがプレミアムコンテンツにアクセスできるようになります。 また、世帯ごとの同時ストリーム、セッションまたはデバイスの数を制限するオプションも提供されます。

**Adobe Pass Authentication はどのデバイスをサポートしていますか？**

Adobe Pass認証は、Web ベースのデバイス、モバイルデバイス、TV 接続デバイスなど、様々なデバイスで機能するように設計されています。

**Adobe Pass認証にエンドユーザーが費やすコストは発生しますか？**

いいえ。 Adobe Pass認証は、エンドユーザーに対して無料です。
