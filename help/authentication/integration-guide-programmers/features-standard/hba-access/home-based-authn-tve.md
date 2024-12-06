---
title: あらゆる場所でテレビを使用する場合のホームベースの認証
description: あらゆる場所でテレビを使用する場合のホームベースの認証
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# あらゆる場所でテレビを使用する場合のホームベースの認証

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## ホームベースの認証とは {#whatis-home-based-authn}

Home Based Authentication （HBA）は、有料テレビの加入者が自宅で MVPD 資格情報を入力しなくてもテレビのコンテンツをオンラインで視聴できる TV Everywhere 機能です。これにより、認証フローのユーザーエクスペリエンスが大幅に向上します。

Open Authentication Technology Committee （OATC）によるホームベースの認証定義：「ホーム内自動認証とは、MVPD/OVD がホームネットワークの特性（またはホームネットワーク上のデバイス間で自動的にアクセスできる識別子）を使用して、そのホームネットワークに関連付けられている加入者アカウントを認証するプロセスです。これにより、ユーザーは、TVE 保護されたコンテンツにアクセスするための TVE セッションを確立する際に資格情報を手動で入力するする必要がなくなります。」



HBA と業界標準について詳しくは、[OATC の使用例と要件 ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} のドキュメントおよび **OATC HBA のユーザーエクスペリエンスガイドライン** を参照してください。

>[!NOTE]
>
>一部の HBA フローは、Premium Workflow パッケージに含まれています。 この機能の使用に興味がある場合は、Adobe Passの営業担当にお問い合わせください。

## HBA が重要な理由 {#why-hba}

HBA は重要です。これは、在宅していて、すでにケーブル契約を結んでいる視聴者のログインバリアを実質的に取り除くからです。 また、ホームベースの認証により、視聴者のエンゲージメントが大幅に向上し、TV Everywhere コンテンツのユーザーエクスペリエンスが向上します。

現在、ログイン試行のほぼ半数が成功していません。

HBA が上位 5 台の MVPD のいずれかによってアクティブ化されると、その認証コンバージョン率は **40% 増加** （45% から 63% に）

![](../../../assets/authn-conv-pre-post.png)

また、異なる MVPD と統合されたチャネルのログイン変換率を以下に示します。このチャネルは、HBA が有効になっているチャネルと HBA が設定されていないチャネルです。 HBA を使用する場合のコンバージョン率は、HBA を使用しない場合よりも大幅に高くなります。

![](../../../assets/hba-vs-non-hba.png)

この MVPD と統合されたほとんどのチャネルで HBA を有効にした 6 か月後、ユニークユーザーが 82% 増加しました（この MVPD を通じて TV Everywhere のチャネルにアクセスするユーザーの数はほぼ 2 倍）。

2w3 これに対し、次の表に示すように、HBA が有効になっていない他の MVPD では、過去 6 か月間にユニーク・ユーザー数が 26% 増加しただけです。

![](../../../assets/unique-visitors-incr.png)

HBA を有効にする 6 か月前と 6 か月後に収集したデータから、HBA を有効にしたチャネルに対する視聴者のエンゲージメントが大幅に増加していることがわかりました。 実際には、HBA が有効になっている MVPD のユーザーは、HBA が有効になっていない MVPD のユーザーよりも、平均で 30% 多くのコンテンツを視聴する傾向があります。

![](../../../assets/user-engagement-increase.png)

## Adobe Pass認証 HBA のサポート {#auth-hba-support}

ここでは、Adobe Pass Authentication が提供する HBA のサポート、HBA フローにおけるAdobe Pass Authentication Platform の動作について説明します。また、HBA の実装に役立つ技術的な詳細についても説明します。

HBA をサポートするAdobe Pass認証機能

* HBA 認証と非 HBA 認証で異なる認証 TTL を設定できる（MVPD のサポートも必要）
* 認証の有効期限が切れた場合に MVPD を自動的に選択する機能（MVPD ピッカーをスキップ）。 これは、特に HBA TTL が小さい場合に便利です。
* 認証が HBA であるかどうかに関わらず、プログラマに公開する機能（MVPD のサポートも必要）

### Adobe Pass Authentication Platforms での HBA のユーザーエクスペリエンス {#hba-user-exp}

次の表は、HBA が有効な場合と HBA が有効でない場合の、サポートされているプラットフォームのユーザーエクスペリエンスに関する情報です。

| ユーザーフロー – プラットフォームタイプ | swf, iOS, Android |
|---|---|
| HBA が有効な場合 | ユーザーは自宅にいると、自動的に認証されます。 HBA AuthN トークンの有効期限が切れると、ユーザーは自動的に再認証されます。 |
| HBA なし | ユーザーは、MVPD を選択し、自宅にいても資格情報を入力するよう求められます。AuthN トークンの有効期限が切れた後、ユーザーは資格情報を再度入力する必要があります。 |

| ユーザーフロー – プラットフォームタイプ | js、Windows （ネイティブ） |
|---|---|
| HBA が有効な場合 | ユーザーは自宅にいると、自動的に認証されます。 HBA AuthN トークンの有効期限が切れた後、ユーザーはピッカーから MVPD を再選択する必要があり、自動的に認証されます。 |
| HBA なし | ユーザーは、MVPD を選択し、自宅にいる場合でも資格情報を入力するように求められます。 AuthN トークンの有効期限が切れると、ユーザーは資格情報を再入力する必要があります。 |

| ユーザーフロー – プラットフォームタイプ | クライアントレス REST API （2 番目の画面の認証） |
|---|---|
| HBA が有効な場合 | ユーザーが自宅でクライアントレス REST API アプリを使用している場合、登録コードを入力して MVPD を選択すると、2 番目の画面デバイスで自動的に認証されます。 HBA AuthN トークンの有効期限が切れると、ユーザーは自動的に再認証されます（2 番目の画面のデバイス）。 |
| HBA なし | ユーザーは、MVPD を選択し、自宅にいる場合でも資格情報を入力するように求められます。 AuthN トークンの有効期限が切れると、ユーザーは資格情報を再入力する必要があります。 |

### HBA 実装の技術的な詳細 {#tech-details-hba}

#### OAuth 2.0 プロトコル {#oauth-2-protocol}

OAuth 2.0 認証プロトコルと統合された MVPD の HBA フローでは、MVPD が更新トークンを発行し、Adobeが HBA 認証トークンを発行します。

* 更新トークンの TTL は、MVPD のビジネス要件によって決定されます。
* HBA 認証トークンの TTL **更新トークンの TTL 以下である必要があります**。


*OAuth 2.0 プロトコルの HBA 認証フローの説明*


| ユーザーアクション | システムアクション |
|---|---|
| ユーザーがプログラマーのサイトに移動します。 ビデオを再生しようとすると、MVPD ピッカーが表示されます。 ユーザーが MVPD を選択し、ログインをクリックします。 | バックグラウンドチェックが実行されます。 MVPD は、ユーザの検出に一連の規則を適用します（たとえば、ユーザの IP アドレスを、ディストリビュータがプロビジョニングしたモデムまたはブロードバンド接続されたセットトップ ボックスのMAC アドレスにマップします）。 |
| 約 3 秒間保持される画面が表示されます。 ユーザーが MVPD アカウントを使用して自動的にログインしていることをユーザーに知らせるインタースティシャルページを表示できます。 | <ol><li>AccessEnabler は、プログラマ側にインストールされ、認証要求（HTTP 要求）をAdobe Pass Authentication エンドポイントに送信します。</li><li>Adobe Pass Authentication エンドポイントは、MVPD 認証エンドポイントにリクエストをリダイレクトします。 <br />**注：** 要求には、MVPD が HBA 認証を試行する必要があることを示す `hba_flag` パラメーター（attempt HBA = true）が含まれています。</li><li>MVPD 認証エンドポイントは、認証コードをAdobe Pass認証エンドポイントに送信します。</li><li>Adobe Pass認証は、認証コードを使用して、MVPD のトークンエンドポイントから更新トークンとアクセストークンをリクエストします。</li><li>MVPD は、認証決定と `hba_status` （true/false）パラメーターを `id_token` で送信します。</li><li>MVPD ユーザープロファイルエンドポイントの呼び出しが送信され、[hba_status キーがユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md#obtaining) に公開されます。</li><li>MVPD は、リフレッシュトークン TTL を MVPD に同意された値に設定し、Adobeは、AuthN トークン TTL をリフレッシュトークンの値以下の値に設定します。</li></ol> |
| ユーザーが認証され、タイトルの付いた TV Everywhere コンテンツを参照できるようになりました。 | 認証トークンは、プログラマーのサイトを正常に閲覧できるユーザーに渡されます。 |

#### SAML プロトコル {#saml-protocol}

SAML 認証プロトコルの HBA 認証フローの説明

| ユーザーアクション | システムアクション |
|---|---|
| ユーザーがプログラマーのサイトに移動します。 ビデオを再生しようとすると、MVPD ピッカーが表示されます。 ユーザーが MVPD を選択し、ログインをクリックします。 | バックグラウンドチェックが実行されます。 MVPD は、ユーザの検出に一連の規則を適用します（たとえば、ユーザの IP アドレスを、ディストリビュータがプロビジョニングしたモデムまたはブロードバンド接続されたセットトップ ボックスのMAC アドレスにマップします）。 |
| 約 3 秒間保持される画面が表示されます。 ユーザーが MVPD アカウントを使用して自動的にログインしていることをユーザーに知らせるインタースティシャルページを表示できます。 | <ol><li>AccessEnabler は、プログラマ側にインストールされ、認証要求（HTTP 要求）をAdobe Pass Authentication エンドポイントに送信します。</li><li>Adobe Pass Authentication エンドポイントは、MVPD 認証エンドポイントにリクエストをリダイレクトします。</li><li>MVPD は、HBA フラグを含む SAML 応答の形式で認証決定を送信する必要があります。hba_status （true/false）。</li><li>MVPD ユーザープロファイルエンドポイントの呼び出しが送信され、[hba_status キーがユーザーメタデータ ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md#obtaining) に公開されます。</li></ol> |
| ユーザーが認証され、タイトルの付いた TV Everywhere コンテンツを参照できるようになりました。 | 認証トークンは、プログラマーのサイトを正常に閲覧できるユーザーに渡されます。 |


## HBA をアクティブにする方法 {#how-to-activate-hba}

* **OAuth プロトコル：**
   * HBA の有効化については、[Adobe Pass TVE Dashboard User Guide を参照してください ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* **SAML プロトコル：** ホームベースの認証は、MVPD 側でアクティブ化されます。 プログラマまたはAdobeは操作を行う必要はありません。
Home Based Authentication をサポートする MVPD の詳細については、「[MVPD の HBA ステータス ](/help/authentication/integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)」を参照してください。

## FAQ {#faqs}


**質問：** SAML と OAuth2 プロトコルを使用したホームベースの認証の分離はなぜですか？

**回答：** HBA のフローは 2 つのプロトコルで異なります。 プログラマーの視点から見ると、SAML MVPD に対して HBA を有効にするための操作は必要ありません。一方、OAuth2 MVPD の場合、Adobe Pass TVE Dashboard で HBA のオン/オフを切り替えることができます。



**質問：** ユーザーは、HBA が有効な場合に初めて認証するときに、ユーザー名とパスワードを入力する必要がありますか？

**回答：** いいえ、ユーザー名とパスワードは必要ありません。



**質問：** 保護者による制限はどのように適用されますか？

**回答 1:** 保護者による制限の承認が必要なチャンネルとの統合について、Adobeが HBA を無効にすることができます。

**回答 2:** Adobeは、ペアレンタルコントロールを使用して HBA を設定する方法を推奨する UX 文書に基づいて OATC を使用しています。



**質問：** HBA をサポートするプロバイダは、HBA の TTL ウィンドウが短く、通常の認証では短いですか？

**回答：** TTL 設定は設定可能です。 誤処理を防ぐため、HBA 認証トークンには短い TTL を設定することをお勧めします。


## 役に立つ情報 {#useful-info}

* [HBA （インスタント・アクセス）Recommendations](http://www.ctamtve.com/instantaccess){target=_blank} - CTAM 提供
* [ プログラマーアプリでの HBA の実装例 ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} - Adobe別
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* OATC の [ ホームベースの認証のユースケースと要件 ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank}
* [ ホームベースの認証のインフォグラフィック ](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} - Adobe別
* [OAuth 2.0 プロトコルを使用した認証](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
* [SAML MVPD を使用した認証](/help/authentication/integration-guide-mvpds/authn-usecase.md)
* [Adobe Pass TVE ダッシュボードユーザーガイド](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* [hba_status ユーザー・メタデータ](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md#obtaining)
