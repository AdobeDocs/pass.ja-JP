---
title: プログラマー
description: TVE ダッシュボード内のプログラマーとその設定について説明します。
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# プログラマー {#programmers}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

TVE ダッシュボードの「**プログラマー**」セクションでは、アカウントの使用権限にリンクされた [ プログラマー ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) の設定を表示および管理できます。 また、必要に応じて [ 新しいプログラマーを追加 ](#add-new-programmer) することもできます。

左側のパネルの **プログラマー** タブには、既存のプログラマーのリストと次の詳細が表示されます。

* **プログラマー ID**：システム内のメディア会社識別子。
* **チャネル**：プログラマーにリンクされている関連チャネルの数。

![ 既存のプログラマーのリスト ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmers-list-view.png)

*既存のプログラマーのリスト*

プログラマーの詳細を表示するには、一覧の上にある **検索** バーにプログラマーの名前を入力します。

## プログラマー設定の管理 {#manage-programmer-conf}

特定のプログラマーの様々な設定を管理するには、次の手順に従います。

1. 左パネルの「**プログラマー**」タブを選択します。
1. リストからプログラマーを選択します。
1. 次のいずれかのタブを選択して、選択したプログラマーの対応する設定を表示および編集します。

   * [チャネル](#channels)
   * [証明書](#certificates)
   * [登録アプリケーション](#registered-applications)
   * [カスタムスキーム](#custom-schemes)

   ![ プログラマ設定 ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-tabs-view.png)

   *プログラマ設定*

>[!IMPORTANT]
>
> 設定変更のアクティベートについて詳しくは、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) を参照してください。

### チャネル {#channels}

このタブには、現在のプログラマーにリンクされているチャネルのリストが表示されます。 このリストから特定のチャネルを選択して、「[ チャネル ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)」セクションの詳細情報にアクセスします。

選択したプログラマーに新しいチャネルを追加するには、「**利用可能なチャネル**」セクションの右上隅にある **新しいチャネルを追加** を選択します。 詳細情報 [ 新しいチャネルの追加方法 ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#add-new-channel)。

![ 新しいチャネルを追加 ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-channel-button.png)

*新しいチャネルを追加*

### 証明書 {#certificates}

このタブには、ユーザーメタデータ暗号化フローで使用される [ 使用可能な証明書 ](#available-certificates) の一覧が表示されます。 次の各証明書に関する詳細が表示されます。

* ステータス（「ユーザーメタデータの暗号化 **使用に対し** 有効になっているかどうか）
* シリアル番号
* 発行者組織の名前
* 件名の組織の名前
* 発行日
* 有効期限
* ユーザーメタデータを暗号化するためのドロップダウンメニュー（「はい **を選択すると、証明書によって、郵便番号の値などの機密性の高いユーザー情報が暗号化されます**。

#### 使用可能な証明書 {#available-certificates}

これらの証明書は、秘密鍵または公開鍵として機能し、ユーザーメタデータの暗号化に使用されます。 同じメディア会社に関連付けられているすべてのチャネルが、これらの証明書を使用できます。

使用可能な証明書は、次のように変更できます。

* [新しい証明書を追加](#add-new-certificate)
* [証明書を削除](#delete-certificate)

##### 新しい証明書を追加 {#add-new-certificate}

新しい証明書を追加するには、次の手順に従います。

1. **利用可能な証明書** セクションの右上隅にある **新しい証明書を追加** を選択します。

   ![ 新しい証明書を追加する ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-certificate-button.png)

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

1. **利用可能な証明書** のリストから削除する証明書の上にマウスポインターを置きます。

1. 「**削除**」を選択します。

   ![ 選択した証明書を削除する ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-remove-certificate-button.png)

   *選択した証明書を削除する*

1. **証明書を削除** ダイアログボックスで **削除** を選択します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 証明書は、「レビューとプッシュの変更 **の後にのみ** 使用可能な証明書 [ セクションから削除さ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) ます。

### 登録アプリケーション {#registered-applications}

このタブには、登録済みアプリケーションのリストが表示されます。 登録されたアプリケーションの使用状況について詳しくは、[ 動的クライアント登録の概要 ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントを参照してください。

登録済みアプリケーションでは、次のアクションを実行できます。

* [新しい登録済みアプリケーションを追加](#add-registered-applications)
* [ソフトウェアのステートメントのダウンロード](#download-software-statement)

#### 新しい登録済みアプリケーションを追加 {#add-registered-applications}

新しい登録済みアプリケーションを追加するには、次の手順に従います。

1. **登録済みアプリケーション** セクションの右上隅にある **新しいアプリケーションを追加** を選択します。

   ![ 新しいアプリケーションの追加 ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-application-button.png)

   *新しいアプリケーションの追加*

1. **新規アプリケーション** ダイアログボックスのドロップダウンメニューから **チャネルに割り当て** を選択します。

   >[!IMPORTANT]
   >
   > セキュリティを強化し、不正アクセスを防ぐために、より具体的で制限された権限を持つ登録済みアプリケーションを作成することをお勧めします。 したがって、登録済みアプリケーションを作成する場合は、割り当てられたアプリケーションに対して、より狭いオプショ `channel` を使用することを検討してください。

1. ドロップダウンメニューから「**プラットフォーム**」を選択します。

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

1. **登録済みアプリケーション** のリストからソフトウェアのステートメントをダウンロードする登録済みアプリケーションにポインタを合わせます。

1. 「**ダウンロード**」を選択します。

   ![ ソフトウェアに関する声明のダウンロード ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-download-software-statement-button.png)

   *ソフトウェアに関する声明のダウンロード*


### カスタムスキーム {#custom-schemes}

このタブには、カスタム スキーマの一覧が表示されます。 カスタムスキームの使用方法について詳しくは、[iOS/tvOS アプリケーションの登録 ](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md) を参照してください。

カスタムスキームには、次の変更を加えることができます。

* [新しいカスタムスキームの生成](#generate-custom-schemes)

#### 新しいカスタムスキームを生成 {#generate-custom-schemes}

次の手順に従って、新しいカスタムスキームを生成します。

1. **新しいカスタムスキームを生成** を選択します。

   ![ 新しいカスタムスキームの生成 ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-custom-scheme-button.png)

   *新しいカスタムスキームの生成*

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**カスタムスキーム**」セクションにリストされている新しいカスタムスキームを使用するには、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローを続行します。

## 新しいプログラマの追加 {#add-new-programmer}

新しいプログラマーエンティティを追加するには、次の手順に従います。

1. 左パネルの「**プログラマー**」タブを選択します。

1. **プログラマー** セクションの右上隅にある **新しいプログラマーを追加** を選択します。

   ![ 新しいプログラマーの追加 ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-programmer-button.png)

   *新しいプログラマーの追加*

1. **新しいプログラマー** ダイアログボックスの **プログラマー ID** にメディアの会社識別子を入力します。

1. コンソールの **表示名** に表示する商用ブランド名を入力します。

1. **プログラマーを追加** を選択します。

新しい設定変更が作成され、サーバーを更新する準備が整いました。 「**プログラマー**」セクションにリストされている新しいプログラマーを使用するには、[ 変更のレビューとプッシュ ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) フローに進みます。
