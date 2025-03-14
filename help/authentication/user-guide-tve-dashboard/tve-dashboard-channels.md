---
title: チャネル
description: TVE ダッシュボード内のチャネルとその様々な設定について説明します。
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# チャネル {#channels}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

TVE ダッシュボードの「**チャネル**」セクションでは、特定のプログラマーに関連付けられたチャネルの設定を表示および管理できます。 また、必要に応じて [ 新しいチャネルを追加 ](#add-new-channel) することもできます。

左側のパネルの「**チャネル**」タブには、リンクされたチャネルのリストと次の詳細が表示されます。

* **表示名**：商業目的で使用されるチャネルのブランド名。
* **チャネル ID**：一意の ID。依頼者 ID とも呼ばれます。
* **統合**: [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) で確立された接続数。

![ 既存のチャネルのリスト ](../assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*既存のチャネルのリスト*

チャネルについて詳しくは、リストの上にある **検索** バーにチャネルの名前を入力します。

## チャネル設定の管理 {#manage-channel-conf}

特定のチャネルの様々な設定を管理するには、次の手順に従います。

1. 左パネルの「**チャネル**」タブを選択します。

1. 使用可能なリストからチャネルを選択します。

1. 次のいずれかのタブを選択して、選択したチャネルの対応する設定を表示および編集します。

   * [一般設定](#general-settings)
   * [統合](#integrations)
   * [証明書](#certificates)
   * [ドメイン](#domains)
   * [登録アプリケーション](#registered-applications)
   * [カスタムスキーム](#custom-schemes)

   ![ チャネル設定 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *チャネル設定*

>[!IMPORTANT]
>
> 設定変更のアクティベートについて詳しくは、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) を参照してください。

### 一般設定 {#general-settings}

このタブには **チャネル情報** と **Analytics 設定** が表示されます。

#### チャネル情報 {#channel-information}

このセクションでは、次の詳細を編集できます。

* **表示名**：商業目的で使用されるチャネルのブランド名。

* **デフォルトのリダイレクト URL**：認証およびログアウト用のバックアップリダイレクト URL。

* **エラーレポート**:「**はい**」を選択すると、Adobe Pass SDK はエラーレポートを分析用にAdobe Pass バックエンドに送信します。

![ チャネル情報の編集 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*チャネル情報の編集*

#### Analytics 設定 {#analytics-configuration}

この節では、Adobe AnalyticsへのAdobe Pass認証イベントの転送を設定できます。

**Analytics 設定** を有効にするには、テクニカルアカウントマネージャー（TAM）に連絡して、レポートスイート ID （RSID）の設定について詳しく問い合わせてください。

![Analytics 設定の有効化 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*Analytics 設定の有効化*

「**新しい Analytics 設定を追加**」を選択して、複数の設定を追加します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**Analytics 設定**」セクションの新しい分析設定を使用するには、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローに進みます。

### 統合 {#integrations}

このタブには、現在選択されているチャネルと MVPD 間で使用可能な統合のリストが表示されます。 このリストには、各統合とそのステータスが表示され、統合が有効かどうかが示されます。 このリストから特定の統合を選択して、「[ 統合 ](tve-dashboard-integrations.md)」セクションに記載されている詳細情報にアクセスします。

![ 使用可能な統合のリスト ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*使用可能な統合のリスト*

### 証明書 {#certificates}

このタブには、ユーザーメタデータ暗号化フローで使用される [ 使用可能な証明書 ](#available-certificates) および [ 継承された使用可能な証明書 ](#inherited-avail-certificates) の一覧が表示されます。 次の各証明書に関する詳細が表示されます。

* ステータス（「ユーザーメタデータの暗号化 **使用に対し** 有効になっているかどうか）
* シリアル番号
* 発行者組織の名前
* 件名の組織の名前
* 発行日
* 有効期限
* ユーザーメタデータを暗号化するドロップダウンメニュー（「はい **を選択すると、証明書によって、郵便番号の値などのユーザーの機密情報が暗号化されます**。

#### 使用可能な証明書 {#available-certificates}

これらの証明書は、秘密鍵または公開鍵として機能し、ユーザーメタデータの暗号化に使用されます。
「利用可能な証明書」セクションで、次の変更を行うことができます。

* [新しい証明書を追加](#add-new-certificate)
* [証明書を削除](#delete-certificate)

##### 新しい証明書を追加 {#add-new-certificate}

新しい証明書を追加するには、次の手順に従います。

1. **利用可能な証明書** セクションの上部にある **新しい証明書を追加** を選択します。

   ![ 新しい証明書を追加する ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

   *新しい証明書を追加する*

1. 証明書の公開鍵を **新しい証明書** ダイアログボックスに貼り付けます。

1. 「**証明書を追加**」を選択します。

1. **使用可能な証明書** の一覧で、新しい証明書を見つけます。

   >[!IMPORTANT]
   >
   > システムが最新で、新しい証明書を使用する準備ができていることを確認します。

1. **暗号化されたユーザーメタデータに使用** ドロップダウンメニューから **はい** を選択して、新しい証明書をアクティブにします。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 **使用可能な証明書** セクションにリストされている新しい証明書を使用するには、[ 変更の確認とプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローに進みます。

##### 証明書を削除 {#delete-certificate}

次の手順に従って、証明書を削除します。

1. **使用可能な証明書** のリストから削除する証明書の上にマウスポインターを置きます。

1. 「**削除**」を選択します。

   ![ 選択した証明書を削除する ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *選択した証明書を削除する*

1. **アクティブな証明書を削除** ダイアログボックスから **削除** を選択します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 証明書は、「レビューとプッシュの変更 [ の後にのみ **使用可能な証明書** セクションから削除さ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) ます。

#### 継承された使用可能な証明書 {#inherited-avail-certificates}

メディア会社は、これらの証明書を独自のレベルで定義します。 同じメディア会社に関連付けられているすべてのチャネルが、これらの証明書を使用できます。

![ 継承された使用可能な証明書 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*継承された使用可能な証明書*

### ドメイン {#domains}

このタブには、各チャネルがAdobe Pass Authentication と通信する際に使用できる使用可能なドメインのリストが表示されます。

ドメインには、次の変更を加えることができます。

* [新しいドメインを追加](#add-domains)
* [ドメインを削除](#delete-domain)

>[!TIP]
>
> より一般的なドメインがリストに存在する場合は、新しいサブドメインを追加しないでください。

#### 新しいドメインを追加 {#add-domains}

ドメインを追加するには、次の手順に従います。

1. **使用可能なドメイン** セクションの右上隅にある **新しいドメインを追加** を選択します。

   ![ 新しいドメインを追加 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *新しいドメインを追加*

1. **新規ドメイン** ダイアログボックスにドメインの名前を入力します。

1. **ドメインを追加** を選択して、選択したチャネルに新しいドメインを追加します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**使用可能なドメイン**」セクションにリストされている新しいドメインを使用するには、[ 変更の確認とプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローに進みます。

#### ドメインを削除 {#delete-domain}

ドメインを削除するには、次の手順に従います。

1. **利用可能なドメイン** のリストから削除するドメインにポインタを合わせます。

1. 「**削除**」を選択します。

   ![ 選択したドメインを削除 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *選択したドメインを削除*

1. **ドメインを削除** ダイアログボックスの **削除** を選択します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 ドメインは、「確認とプッシュの変更 [ の後にのみ **「使用可能なドメイン** セクションから削除され ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) す。

選択したドメインは使用できなくなりました。 その結果、このドメインに関連付けられているアプリケーションは、Adobe Pass Authentication サービスへのアクセスができなくなります。

### 登録アプリケーション {#registered-applications}

このタブには、登録済みアプリケーションのリストが表示されます。 登録されたアプリケーションの使用状況について詳しくは、[ 動的クライアント登録の概要 ](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

登録済みアプリケーションでは、次のアクションを実行できます。

* [新しい登録済みアプリケーションを追加](#add-registered-applications)
* [ソフトウェアのステートメントのダウンロード](#download-software-statement)

#### 新しい登録済みアプリケーションを追加 {#add-registered-applications}

新しい登録済みアプリケーションを追加するには、次の手順に従います。

1. **登録済みアプリケーション** セクションの右上隅にある **新しいアプリケーションを追加** を選択します。

   ![ 新しいアプリケーションの追加 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *新しいアプリケーションの追加*

1. **新規アプリケーション** ダイアログボックスのドロップダウンメニューから **プラットフォーム** を選択します。

   >[!IMPORTANT]
   >
   > セキュリティを強化し、不正アクセスを防ぐために、より具体的で制限された権限を持つ登録済みアプリケーションを作成することをお勧めします。 したがって、登録済みアプリケーションを作成する場合は、割り当てられたアプリケーションに対して、より狭いオプショ `platforms` を使用することを検討してください。

1. ドロップダウンメニューから **ドメイン** を選択します。

   >[!IMPORTANT]
   >
   > クライアント登録プロセスでは、認証フローの最終処理にリダイレクト URL を使用することをクライアントアプリケーションに許可するリクエストを実行できます。 クライアントアプリケーションで特定のリダイレクト URL を使用すると、この選択で選択された `domains` に対して検証されます。

1. アプリケーションの **名前** を入力します。

1. アプリケーションの **バージョン** を入力します。

   >[!IMPORTANT]
   >
   > クライアントアプリケーションのライフサイクルと使用状況を管理するには、クライアントアプリケーションのメジャーアップデートごとに新しい登録アプリケーションを作成することをお勧めします。 必要に応じて、アドビの [Zendesk](https://adobeprimetime.zendesk.com) を通じてチケットを作成し、テクニカルアカウントマネージャー（TAM）に依頼して、特定のクライアントアプリケーションバージョンの機能をブロックするために、登録されたアプリケーションを失効させます。

1. ドロップダウンメニューから **タイプ** 値「ダイレクト」を選択します。

1. **アプリケーションを追加** を選択します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**登録済みアプリケーション**」セクションにリストされている新しい登録済みアプリケーションを使用するには、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローを続行します。

#### ソフトウェアのダウンロードに関する声明 {#download-software-statement}

ソフトウェア・ステートメントをダウンロードするには、次の手順に従います。

1. **登録済みアプリケーション** のリストからソフトウェアステートメントをダウンロードする登録済みアプリケーションにポインタを合わせます。

1. 「**ダウンロード**」を選択します。

   ![ ソフトウェアに関する声明のダウンロード ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *ソフトウェアに関する声明のダウンロード*

### カスタムスキーム {#custom-schemes}

このタブには、カスタム スキーマの一覧が表示されます。 カスタムスキームの使用方法について詳しくは、[iOS/tvOS アプリケーションの登録 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md) を参照してください。

カスタムスキームには、次の変更を加えることができます。

* [新しいカスタムスキームの生成](#generate-custom-schemes)

#### 新しいカスタムスキームを生成 {#generate-custom-schemes}

次の手順に従って、新しいカスタムスキームを生成します。

1. **新しいカスタムスキームを生成** を選択します。

   ![ 新しいカスタムスキームの生成 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *新しいカスタムスキームの生成*

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**カスタムスキーム**」セクションにリストされている新しいカスタムスキームを使用するには、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローを続行します。

#### 継承されたカスタム スキーマ {#inherited-custom-schemes}

メディア会社は、これらのカスタムスキームを独自のレベルで定義します。 同じメディア会社に関連付けられたすべてのチャネルは、これらのカスタムスキームを使用できます。

![ 継承されたカスタム スキーマ ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*継承されたカスタム スキーマ*

## 新しいチャネルを追加 {#add-new-channel}

新しいチャネルを追加するには、次の手順に従います。

1. 左パネルの「**チャネル**」タブを選択します。

1. **チャネル** セクションの右上隅にある **新しいチャネルを追加** を選択します。

   ![ 新しいチャネルを追加 ](../assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *新しいチャネルを追加*

1. **新規チャネル** ダイアログボックスのドロップダウンメニューから **プログラマー ID** を選択します。

1. **チャネル ID** に一意の ID を入力します。

1. **表示名** に、商用目的で使用するチャネルのブランド名を入力します。

1. **チャネルを追加** を選択します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**チャネル**」セクションにリストされている新しいチャネルを使用するには、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローに進みます。
