---
title: TV のあらゆる場所でのホームベースの認証
description: TV のあらゆる場所でのホームベースの認証
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1687'
ht-degree: 0%

---

# TV のあらゆる場所でのホームベースの認証

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## ホームベースの認証とは {#whatis-home-based-authn}

HBA(Home Based Authentication) は、TV Everywhere の機能で、有料テレビの加入者が自宅にいるときに MVPD の資格情報を入力することなく、TV コンテンツをオンラインで視聴できるので、認証フローのユーザーエクスペリエンスが大幅に向上します。

OATC(Open Authentication Technology Committee:OATC) によるホームベース認証の定義： 「自動認証とは、MVPD/OVD がホームネットワークの特性（またはホームネットワーク上のデバイス間で自動的にアクセス可能な ID）を使用して、保護 TVE の TVE セッションを確立する際に内容」



HBA と業界標準の詳細については、 [OATC の使用例と要件](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} ドキュメントと **HBA に関する OATC ユーザーエクスペリエンスのガイドライン**.

>[!NOTE]
>
>一部の HBA フローは、Premium Workflow パッケージの一部です。 この機能の使用に関心がある場合は、Adobe Passの営業担当にお問い合わせください。

## HBA が重要な理由 {#why-hba}

HBA は、実際には自宅にあり、既にケーブルを購読しているビューアのサインイン障壁を取り除くので、重要です。 また、ホームベースの認証を使用すると、視聴者のエンゲージメントを大幅に向上させ、TV Everywhere コンテンツのユーザーエクスペリエンスを向上させることができます。

現在、ログインの試みのほぼ半分が成功していません。

HBA が上位 5 件の MVPD の 1 つによってアクティブ化されると、その認証の変換率が向上します。 **40%増加する** (45%～63%)

![](assets/authn-conv-pre-post.png)

また、以下に、異なる MVPD と統合されたチャネルのサインインコンバージョン率（HBA を有効にしたチャネルと HBA を持たないチャネル）を示します。 HBA を使用する場合のコンバージョン率は、HBA を使用しない場合のコンバージョン率よりも大幅に高くなります。

![](assets/hba-vs-non-hba.png)

この MVPD と統合されたほとんどのチャネルの HBA を有効にしてから 6 か月後、ユニークユーザー数が 82%増加しました（この MVPD を通じて TV Everywhere チャネルにアクセスするユーザーの数は、ほぼ 2 倍になりました）。

2w3 これに対し、下の図で示すように、HBA を有効にしていない他の MVPD は、過去 6 か月間でユニークユーザー数が 26%増加しました。

![](assets/unique-visitors-incr.png)

HBA を有効にする 6 か月前と 6 か月後に収集したデータから、HBA が有効になっているチャネルに対するビューアのエンゲージメントが大幅に増加しました。 HBA を有効にした MVPD の実用的なユーザーは、HBA を有効にしていない MVPD のユーザーよりも、平均 30%多くのコンテンツを監視する傾向があります。

![](assets/user-engagement-increase.png)

## Adobe Pass認証 HBA のサポート {#auth-hba-support}

このセクションでは、Adobe Pass認証が提供する HBA のサポート、HBA フローにおけるAdobe Pass認証プラットフォームの動作、および HBA の実装に役立つ技術的な詳細について説明します。

HBA をサポートするAdobe Pass認証機能

* HBA 認証と非 HBA 認証に異なる認証 TTL を設定する機能（MVPD のサポートも必要）
* 認証が期限切れになった場合に、MVPD（MVPD ピッカーをスキップ）を自動的に選択する機能。 これは、特に HBA の TTL が小さい場合に役立ちます。
* 認証が HBA かどうか（MVPD のサポートも必要）をプログラマに公開する機能

### Adobe Pass認証プラットフォームでの HBA ユーザーエクスペリエンス {#hba-user-exp}

次の表は、HBA が有効な場合と、HBA が無効な場合の、サポートされるプラットフォームのユーザーエクスペリエンスに関する情報を示しています。

| ユーザーフロー — プラットフォームタイプ | swf, iOS, Android |
|---|---|
| HBA が有効な場合 | ユーザーが不在の場合は、自動的に認証されます。 HBA AuthN トークンの期限が切れると、ユーザーは自動的に再認証されます。 |
| HBA なし | ユーザーは、MVPD を選択し、自宅にいる場合でも資格情報を入力するように求められます。AuthN トークンが期限切れになった後、ユーザーは資格情報を再度入力する必要があります。 |

| ユーザーフロー — プラットフォームタイプ | js、Windows（ネイティブ） |
|---|---|
| HBA が有効な場合 | ユーザーが不在の場合は、自動的に認証されます。 HBA AuthN トークンの期限が切れた後、ユーザーはピッカーから MVPD を再選択する必要があり、自動的に認証されます。 |
| HBA なし | ユーザーは、自宅にいる場合でも、MVPD を選択し、資格情報を入力するように求められます。 AuthN トークンの有効期限が切れた後、ユーザーは資格情報を再入力する必要があります。 |

| ユーザーフロー — プラットフォームタイプ | クライアントレス REST API（2 つ目の画面認証） |
|---|---|
| HBA が有効な場合 | ユーザーが自宅にいて、Clientless REST API アプリを使用している場合、登録コードを入力し、MVPD を選択した後、2 番目の画面デバイスで自動的に認証されます。 HBA AuthN トークンの期限が切れると、ユーザーは（2 番目の画面デバイスで）自動的に再認証されます。 |
| HBA なし | ユーザーは、自宅にいる場合でも、MVPD を選択し、資格情報を入力するように求められます。 AuthN トークンの有効期限が切れた後、ユーザーは資格情報を再入力する必要があります。 |

### HBA の実装に関する技術的な詳細 {#tech-details-hba}

#### OAuth 2.0 プロトコル {#oauth-2-protocol}

OAuth 2.0 認証プロトコルと統合された MVPD の HBA フローでは、MVPD は更新トークンを発行し、Adobeは HBA 認証トークンを発行します。

* リフレッシュトークンは、MVPD のビジネス要件によって決定される TTL を持ちます。
* HBA 認証トークンの TTL **は次よりも小さいか等しい必要があります** 更新トークンの TTL。


*OAuth 2.0 プロトコルの HBA 認証フローの説明*


| ユーザーアクション | システムアクション |
|---|---|
| ユーザーは、プログラマーのサイトに移動します。 ビデオを再生しようとすると、MVPD ピッカーが表示されます。 ユーザーが MVPD を選択し、ログインをクリックします。 | バックグラウンドチェックが実行されます。 MVPD は、ユーザ検出のための一連の規則を適用します ( たとえば、ユーザの IP アドレスを、ディストリビュータがプロビジョニングしたモデムのMACアドレスや、ブロードバンド接続したセットトップボックスにマッピングします )。 |
| 約 3 秒間表示される画面です。 MVPD アカウントを使用して自動的にサインインしていることをユーザに知らせるインタースティシャルページを表示できます。 | <ol><li>AccessEnabler は、プログラマー側にインストールされ、（HTTP リクエストとして）認証リクエストをAdobe Pass Authentication エンドポイントに送信します。</li><li>Adobe Pass Authentication エンドポイントは、リクエストを MVPD 認証エンドポイントにリダイレクトします。 <br />**注意：** リクエストには `hba_flag` パラメータ（HBA の試行= true）。MVPD が HBA 認証を試行する必要があることを示す。</li><li>MVPD 認証エンドポイントは、認証コードをAdobe Pass Authentication エンドポイントに送信します。</li><li>Adobe Pass認証は、認証コードを使用して、MVPD のトークンエンドポイントから更新トークンとアクセストークンをリクエストします。</li><li>MVPD は、認証の決定と `hba_status` (true/false) パラメーターを `id_token`.</li><li>MVPD ユーザープロファイルエンドポイントへの呼び出しが送信され、 [ユーザメタデータの hba_status キー](/help/authentication/user-metadata-feature.md#obtaining).</li><li>MVPD は、リフレッシュトークン TTL を MVPD 同意値に設定し、Adobeは、AuthN トークン TTL をリフレッシュトークンの値以下の値に設定する。</li></ol> |
| ユーザーが認証され、権利が付与された TV Everywhere コンテンツを閲覧できるようになりました。 | 認証トークンは、プログラマーのサイトを正常に参照できるユーザーに渡されます。 |

#### SAML プロトコル {#saml-protocol}

SAML 認証プロトコルの HBA 認証フローの説明

| ユーザーアクション | システムアクション |
|---|---|
| ユーザーは、プログラマーのサイトに移動します。 ビデオを再生しようとすると、MVPD ピッカーが表示されます。 ユーザーが MVPD を選択し、ログインをクリックします。 | バックグラウンドチェックが実行されます。 MVPD は、ユーザ検出のための一連の規則を適用します ( たとえば、ユーザの IP アドレスを、ディストリビュータがプロビジョニングしたモデムのMACアドレスや、ブロードバンド接続したセットトップボックスにマッピングします )。 |
| 約 3 秒間表示される画面です。 MVPD アカウントを使用して自動的にサインインしていることをユーザに知らせるインタースティシャルページを表示できます。 | <ol><li>AccessEnabler は、プログラマー側にインストールされ、（HTTP リクエストとして）認証リクエストをAdobe Pass Authentication エンドポイントに送信します。</li><li>Adobe Pass Authentication エンドポイントは、リクエストを MVPD 認証エンドポイントにリダイレクトします。</li><li>MVPD は、SAML 応答の形で認証の決定を送信する必要があります。この決定には、HBA フラグ hba_status (true/false) が含まれます。</li><li>MVPD ユーザープロファイルエンドポイントへの呼び出しが送信され、 [ユーザメタデータの hba_status キー](/help/authentication/user-metadata-feature.md#obtaining).</li></ol> |
| ユーザーが認証され、権利が付与された TV Everywhere コンテンツを閲覧できるようになりました。 | 認証トークンは、プログラマーのサイトを正常に参照できるユーザーに渡されます。 |


## HBA のアクティブ化方法 {#how-to-activate-hba}

* **OAuth プロトコル：**
   * HBA を有効にする場合は、を参照してください。 [Adobe Pass TVE ダッシュボードユーザガイド](/help/authentication/tve-dashboard-user-guide.md)
* **SAML プロトコル：** 「Home Based Authentication」は MVPD 側で有効化されます。 プログラマーやAdobeは、何もする必要はありません。
ホームベースの認証をサポートする MVPD について詳しくは、 [MVPD の HBA ステータス](/help/authentication/hba-status-mvpds.md).

## FAQ {#faqs}


**質問：** SAML および OAuth2 プロトコルを使用したホームベースの認証を分離する理由を教えてください。

**回答：** HBA のフローは、2 つのプロトコルによって異なります。 プログラマーの観点からは、HBA が SAML MVPD で有効になっていることを保証するアクションは必要ありませんが、OAuth2 MVPD では、Adobe Pass TVE ダッシュボードで HBA のオン/オフを切り替えることができます。



**質問：** HBA が有効になっているときに初めて認証を行う際に、ユーザー名とパスワードを入力する必要がありますか。

**回答：** いいえ。ユーザー名とパスワードは不要です。



**質問：** 保護者による制限をどのように実施しますか？

**回答 1:** Adobeは、保護者による制御の承認が必要なチャネルとの統合に対して HBA を無効にできます。

**回答 2:** Adobeは、UX ドキュメントで OATC を使用しています。UX ドキュメントでは、HBA エクスペリエンスを保護者による制御で設定する方法を推奨しています。



**質問：** HBA をサポートするプロバイダは、HBA の TTL 期間を短くし、通常の認証に使用しますか。

**回答：** TTL 設定は設定可能です。 HBA 認証トークンの TTL を短く設定して、誤処理を防ぐことをお勧めします。


## 役に立つ情報 {#useful-info}

* [HBA (Instant Access) Recommendations](http://www.ctamtve.com/instantaccess){target=_blank} - CTAM 別
* [プログラマーアプリケーションでの HBA の実装例](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} -Adobe別
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* [ホームベースの認証の使用例と要件](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank} OATC によって
* [ホームベースの認証の解説図](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} -Adobe別
* [OAuth 2.0 プロトコルを使用した認証](/help/authentication/authn-oauth2-protocol.md)
* [SAML MVPD を使用した認証](/help/authentication/authn-usecase.md)
* [Adobe Pass TVE ダッシュボードユーザガイド](/help/authentication/tve-dashboard-user-guide.md)
* [hba_status ユーザのメタデータ](/help/authentication/user-metadata-feature.md#obtaining)
