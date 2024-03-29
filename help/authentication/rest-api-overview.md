---
title: REST API の概要
description: Rest API の概要
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 0%

---

# REST API の概要 {#rest-api-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


## 概要 {#over}

Adobe Pass Authentication REST API を使用すると、TV Everywhere(TVE) 認証および承認サービスに直接アクセスできます。 この API は、サーバー間または接続デバイス（ゲームコンソール、スマートテレビ、セットトップボックスなど）の 2 つの主要なアーキテクチャをサポートしています。 Web 閲覧機能を持たないアプリケーション。

### スロットルメカニズム

Adobe Pass Authentication REST API は、 [スロットルメカニズム](/help/authentication/throttling-mechanism.md).


### サーバー間

サーバー間ソリューションには、TVE フロー用のAdobe Pass認証サービスと接続するプログラマーサービスと統合するプログラマークライアントアプリケーションが含まれます。 このアプローチでは、TVE 実装のほとんどをクライアントからサーバに移行し、単一の統合認証モジュールを構築し、維持することができます。 クライアントアプリケーションの主な残りの責任は、ユーザー認証用の Web ビューの管理です。



### 接続されたデバイス

Connected Devices アプリは、REST API を通じてAdobe Pass認証と直接通信し、設定、登録、認証ステータスチェックおよび認証フローを実行します。一方、認証フローには 2 つ目の画面（ブラウザー）アプリが必要です。 したがって、ネイティブ SDK は使用されません。



### その他のアーキテクチャ

スマートデバイス用の 2 つの主要な REST API ベースのアーキテクチャ、サーバー間および直接のクライアントソリューションに加えて、他のアーキテクチャもあります。  プライマリは SDK アーキテクチャです。このアーキテクチャは、Adobe Pass Authentication がプログラマーに提供する Access Enabler と呼ばれるクライアントコンポーネントを使用します。  デスクトップアプリケーションは、Access Enabler API を使用して起動、認証、承認、ログアウトを処理します。  プログラマーのアプリとAdobe Pass認証サーバ間の通信は、Access Enabler を通じて行われます。  JavaScript、iOS、tvOS、Android および FireTV の各プラットフォームでは、異なる種類の Access Enabler を使用できます。

サーバー間ソリューション以外でネイティブ SDK をサポートするクライアントプラットフォームでは、REST API を直接使用できますが、この方法はお勧めしません。



## REST API の長所と短所 {#ProsAndCons}

Adobe Pass Authentication REST API は、Web 閲覧機能や永続的なストレージを持たないデバイス向けに、TV Everywhere(TVE) ソリューションを提供するために作成されました。 REST API は、すべての認証および承認フローをサポートしますが、ネイティブの SDK コンポーネントが不足しているのでサポートします。 Adobe Pass Authentication で提供および管理される SDK には、ビジネスルールを実装する標準の機能が付属しています。REST API の場合は、プログラマーが実装および管理する必要があります。 以下の「プログラマーの責務」の表では、プログラマーが対処する必要がある、現在の REST API の制限事項について説明します。



### サーバ間とクライアントベースの長所と短所

サーバー間アーキテクチャを使用すると、認証と承認に関連するロジックのほとんどを 1 つの論理ユニットまたは実装に統合できます。  このアプローチには長所と短所があります。  長所は次のとおりです。

* 認証および承認ビジネスロジック用の単一実装。
* そのプラットフォームでは、サポートされる各プラットフォームにそのロジックを実装する必要はありません。
* 関連するすべての要件（アプリストアの更新など）でクライアントを更新する必要なく、機能を更新する機能。
* **より簡単に** 拡張およびカスタム authN および authZ 機能（D2C の追加など）。
* 関連するトラフィックを直接管理して、より詳細な制御、品質、および監視を実現します。



この場合も、プログラマーの責任に短所が記載されますが、次の内容が含まれます。

* Platform SSO を使用しないプラットフォームでは、クライアントごとに SSO を実装する必要があります。
* プログラマーは、必要に応じて MVPD 固有のロジックを実装する必要があります。
* REST API を使用するすべてのプラットフォームは、認証 TTL などのプロパティを管理する単一の設定を共有します。



### 接続されたデバイス

ほとんどの接続デバイスでは、SDK を使用できないので、REST API を一方向または別の方法で使用する必要があります。 接続されたデバイスは、REST API を直接使用するか、REST API を使用するサーバー間ソリューションと統合します。

## プログラマーの責務 {#programmer-responsibilities}

次の操作は、Server-to-Server と Connected Device の両方のアプリケーションに適用されます。

| **機能** | **機能の処理を担当** | **現在のクライアントレス API の制限事項とネイティブ SDK との違いについて説明します。** |
| --- | --- | --- |
| プラットフォームごとに適用された設定 | Adobe | 1 つ **重大な制限** すべてのプラットフォーム (iOS、Android などのモバイルデバイス ) で REST API を使用する場合、TVE ダッシュボード設定ツールの REST API に対応する設定は、(REST API 上にネイティブアプリケーションが実装されているiOSデバイスがある場合でも ) すべてのデバイスに適用されます。 この制限 **壊れる** 同意された TTL と MVPD とのプラットフォーム設定（プラットフォームごとに異なる場合）。 [1](#1) |
| シングルサインオン | プログラマー | REST API を使用すると、SSO は Platform SSO をサポートするプラットフォーム (Apple、Roku、Amazonなど ) でのみ使用できますが、REST API を使用する場合は SSO を他のプラットフォームで保証できません。 SDK は、サイト/アプリをまたいでデータをキャッシュします。 つまり、ユーザーはサイトやアプリで 1 回だけログインし、ユーザーの操作なしで、既に参加しているサイトにログインしています。 [2](#2) |
| 単一ログアウト | プログラマー | ネイティブの SDK SSO シナリオでは、参加している 1 つのアプリケーションからログアウトすると、あらゆる場所からユーザーがログアウトされます。 現在の REST API では SLO をサポートしていませんが、1 つのアプリケーションからログアウトすると、その特定のアプリケーションに対してのみユーザーがログアウトされます。 |
| キャッシュ | プログラマー | REST API 実装では、ビジネス上の合意を得たデータ項目に対して、独自のキャッシュメカニズムを実装する必要があります。 SDK は、様々なビジネスルールを考慮しながら、様々なデータ項目を自動的にキャッシュします。 例えば、ユーザーメタデータは認証トークンと同じ TTL を使用してキャッシュされますが、一部の項目はプログラムによってキャッシュ（プリフライト）から除外される場合があります。 |
| 詳細なエラー報告メカニズム | プログラマー | REST API は主に、アプリケーションエラーの報告に HTTP エラーコードを使用しますが、SDK は、アプリケーション開発者が何が起きているかをより深く理解するのに役立つ詳細なエラー報告メカニズムを備えています。 |
| アプリケーションエラーの回復（エラー時の再試行、ループ検出など） | プログラマー | REST API 実装は、独自のアプリケーション回復システムを構築する必要がありますが、SDK 上に実装がある場合は、SDK のエラー回復システムがメリットを得ます。一時的なネットワークエラーからの回復は、「ループ」を防ぐために、特定のネットワーク呼び出しを再試行します。 |
| リクエスト元ごとの認証 | プログラマー | 一部の MVPD は、ビジネス上の理由または技術的な課題が原因で、各サイト/アプリに対する認証を必要とします。 SDK は、TVE ダッシュボードの設定に基づいて、これを自動的に実行します。 REST API 実装者は、ビジネス契約に違反しないように、またはアプリケーションごとに認証データをスコープ設定する MVPD に対して認証を完了できるように、これを自ら実装する必要があります。 |
| パッシブ認証 | プログラマー | 一部の MVPD は「パッシブ」認証をサポートしています。この認証では、ユーザーは資格情報を入力する必要がなく、ユーザーの認証を自動的に試みます。 これは、要求元ごとの「認証」も持つ MVPD に特に便利です。 このような場合、SDK が有効にする UX はシームレスです。この場合、ユーザーはアプリケーションで 1 回だけ認証され、SDK はエコシステム内の他のアプリケーションに対して「パッシブ」認証を実行します。 ユーザーは追加のパッシブ呼び出しを確認できず、既に認証済みであるだけです。一方、MVPD は、アプリケーションごとに別々の認証セッションを持つという目標を達成します。 |
| 暗黙的で均一なデバイス検出 | プログラマー | SDK は、デバイスタイプを自動的に検出し、その情報を標準的な方法で送信します。 REST API 実装では、実装者に応じて異なる情報が送信されがちなので、サイトをまたいでビジネスルールや統計を偏らせる傾向があります。 **プログラマーは、正しいデバイス情報が送信されたことを確認する必要があります。** 各アプリで サーバー間実装の場合、追加の手順がおこなわれない限り、REST API はエンドユーザーの IP アドレスを正しく識別しません。 IP アドレスは、不正防止や HBA などの特定の使用事例で重要です。 |
| 改ざん防止 TempPass | プログラマー | TempPass の強制は、安定したデバイス ID に基づいておこなわれます。 SDK は、ハードウェア情報/フィンガープリント手法を使用してデバイスを識別します。このメカニズムは公開されておらず、より安全で衝突のないものです。 REST API の実装の場合、プログラマーは、デバイスの識別/フィンガープリント用に独自のアルゴリズムを実装する必要があります。 |
| デバイスのバインディング | プログラマー | SDK 経由のアプリケーションを持つデバイスで生成されたトークンは移植性がないので、悪意のあるユーザーがトークンを共有し、他のユーザーにアクセスを提供するのが困難です。 これは、「改ざん防止用 TempPass」と同じデバイス ID メカニズムに基づいています。 REST API の場合、トークンはクラウドに残ります。つまり、同じ呼び出しをおこなう同じ deviceID を持つ 2 つのデバイスが、同じ応答/アクセスを受け取ることになります。 プログラマーは、デバイス ID を生成/割り当てるための、堅牢で安全で衝突のないメカニズムを備えていることを確認する必要があります。 |
| 予測可能で認定済みの機能セット | プログラマー | 各 SDK の既存の機能セットは、一貫性があり、予測可能で、完全に認定され、テストされています。 一部のビジネス要件は SDK を介して適用されるので、プログラマーが REST API を使用している間に契約に違反するリスクが高くなります。 REST API はより柔軟性が高い可能性がありますが、Adobeは、SDK 実装によって TVE ダッシュボードからのビジネス上の決定と設定が強制されることを保証します。 クライアントレスの場合、各プログラマーは、特定の時点でアプリケーションをスイートするビジネスルールの独自のサブセットを実装します。 |


1. アドビは、新しい One API イニシアティブの一環として、この制限を修正し、デバイスの識別に基づいてプラットフォームごとにルールを適用できるようにする予定です。

2. Adobeは、アドビの REST API で使用できる Platform SSO を実装するために、すべての主要なプラットフォームで引き続き動作します。 アドビの One API イニシアチブは、ネイティブ SDK を使用して実装されたアプリと、REST API を使用して実装されたアプリの間で SSO サポートを提供します。

## 最小デバイス要件 {#min_reqs}

Adobe Pass Authentication REST API を使用するには、デバイスが、 [Adobe Pass Authentication Platform /デバイス/ツールの要件ドキュメント](#general_clientless_reqs).
