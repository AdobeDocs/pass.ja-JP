---
title: プログラマー向けキックスタートガイド
description: プログラマー向けキックスタートガイド
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# プログラマー向けキックスタートガイド {#programmer-kickstart-guide}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#prog-kickstart-guide-intro}

このたびは、Adobe Pass Authentication for TV Everywhere をご利用いただき、誠にありがとうございます。 皆様のご協力を心よりお待ちしております。

>[!NOTE]
>
>これは、プログラマー（コンテンツプロバイダー）向けのキックスタートガイドです。 Multi-channel Video Programming Distributor （MVPD）を使用している場合は、必ず [MVPD キックスタートガイド ](/help/authentication/kickstart/mvpd-kickstart-guide.md) を参照してください。


Adobe Pass認証連絡先：

* サポート – すべての質問、インシデント、機能リクエストに対して、`tve-support@adobe.com`
* イネーブルメント連絡先は、プロジェクトが開始される時点までにプロジェクトに割り当てられます。

以下の情報は、堅実で効率的なスタートを切るための重要な最初のステップの概要です。 目標は、パートナーと協力して統合を達成する方法について説明し、期待を与えることです。 各アイテムについて、以下の「お客様が提供する」 / 「Adobeが提供する」の節に注意してください。 これらは、プロジェクトの作業中にチェックリストやガイドとしてリストされます。

このドキュメントでは、プログラマが選択した MVPD パートナーと協力するためにサインアップしていることを前提としています。

## リリーススケジュール {#release-schedule}

Adobeスプリントの開発サイクルが計画されており、リリースのスケジュールや、開発体制を通じた各リリースのプロモーション方法を確認できます。

各スプリントが完了すると、QE で使用したり、UAT サーバーで新しい実装を確認したりできます。

約 6 週間ごとに、Adobe事前認定サーバにリリースが行われます。 （これは次期リリースを予定しているサーバーで、最終 QE を実施しています。 これらのビルドには、最後のドロップ以降にスプリントで完了したすべての作業が含まれます。 現時点では、パートナーがこのリリースをテストするために、2 週間の QE 期間が利用可能です。

前の 2 週間のテスト期間中に重大な問題が発生しなかったとしても、リリースは実稼動環境に昇格されます。 つまり、統合はAdobeリリース環境で利用できますが、パートナーはリリースを公開するタイミングを選択します。

<!--For the latest release schedule information, see the Release Calendar.-->

## サポートドキュメント {#supp-doc}

Adobeは以下を提供します。

* デプロイメントガイド：**`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Zendesk カスタマーサポートシステムにアクセスできます。 また、いくつかのプロセスに関するサンプル、情報、ビデオチュートリアルも見つけることができます。 Zendesk でこのドキュメントにアクセスするには、他のドキュメントと共に、`https://tve.zendesk.com/home` でアカウントを登録して作成する必要があります。 登録できるユーザー数に制限はありません。  提出されたチケットに関するコメントを表示して共有できます。 すべてのサポートに関する質問は、`tve-support@adobe.com` に問い合わせてください。
* [プログラマー統合ガイド](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)
* メディア トークン検証子ライブラリ：`https://tve.zendesk.com/entries/471323-media-token-validator-library`。

## テスト環境の設定 {#test-env-setup}

Adobeでは、まず、Adobeのテストサイトを設定します。このサイトでは、Adobeがテスト目的で MVPD として機能します。 その後、チームは、AdobeAPI を呼び出すテスト web サイトを設定できます。 デフォルトの MVPD セレクターを使用し、idP として「Adobe」を選択します。

以下を指定します。

1. 要求者 ID。 これは、Adobe Pass Authentication に対してリクエストを行う web サイトまたはアプリケーションのブランドを一意に識別する文字列です。 文字列自体は任意ですが、Adobeーとプログラマーの間で合意する必要があります
1. チャネル情報。 リクエスター ID によってリクエストされるコンテンツチャネルを識別する文字列のセットです。 多くの場合、チャネルとリクエスター ID は同じです。 ただし、同じ ID でリクエストできるコンテンツのチャネルが複数ある場合もあります。 チャンネル名の文字列は、ケーブル TV チャンネルに対応している必要があります。 一部の MVPD では、この値を AuthN や AuthZ プロトコルで検証します。
1. ドメイン名（そのリクエスター ID に使用できます）。 これは、要求者 ID を受け入れるためにAdobeによって一覧表示される、実際のドメイン名のリストになります。 これにより、承認されたドメインのみがメタデータを使用してAdobe Pass認証にアクセスできるようになります。 メモ：実稼動で有効なドメイン名は、テストやステージングで異なる場合があり、提供および識別する必要があります。

Adobeがアカウントを設定し、Adobeが提供する情報：

* テストサイトにアクセスするためのログインとパスワード

## MVPD のセットアップ {#setup-mvpd}

このセクションでは、MVPD を使用するためにAdobe テスト サイトから移行する際に必要な事項について説明します。

（MVPD 経由で）以下を指定します。

* **2 組の資格情報**:
   * AuthN + AuthZ：認証および承認されたユーザーのログイン/パスワード
   * AuthN +非 AuthZ：認証されたが承認されていないユーザーのログイン/パスワード
* **リソース ID**。 これは、AuthZ プロトコル経由で MVPD で検証される特定のコンテンツ識別子です。 これは、チャネル、番組、エピソード、アセットレベルのいずれかで設定できます。MVPD と合意する必要があります。

Adobe Pass認証では、MRSS ベースのメタデータスキーマがサポートされています。つまり、リソース ID は必要に応じて特定にすることができ、特定の MVPD に固有の ID を含めることができます。

**新しい MVPD 統合**：選択した MVPD が、統合の完了に不可欠な役割を果たしていることを覚えておくことが重要です。 Adobeは、仕様に応じて各 MVPD のコードを記述する必要があります。 これらの手順が完了するまで、ダイアログからその MVPD を選択したり、製品テストを完了したりすることはできません。 Adobeは、次に利用可能なスプリントに合わせて、この作業を事前にスケジュールする必要があります。 （現在のスケジュール情報については、リリースカレンダーを参照してください）。

**既存の MVPD 統合**：選択した MVPD が既にAdobeで設定されている場合、接続手順はより簡単で（高速で）なはずで、多くの場合、設定を変更することで接続を確立できます。

>[!NOTE]
>
>MVPD は、プログラマーを引き続き有効にする必要があり、関連するビジネス取引にサインオフする必要があります。

**MVPD を使用した QE**：すべての統合には共同 QE が含まれ、エンドユーザーは最終的に MVPD の顧客なので、多くのユーザーは「ライブ」プッシュの前にテストサイクルを設定しています。 これには MVPD リソースのスケジューリングが含まれるため、遅延が発生する可能性があります。

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
