---
title: MVPD Direct Integration Plan
description: MVPD Direct Integration Plan
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# MVPD キックスタートガイド：MVPD直接統合プラン {#mvpd-dir-int-plan}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#mvpd-kickstart-intro}

このたびは、Adobe Pass Authentication for TV Everywhere をご利用いただき、誠にありがとうございます。  皆様のご協力を心よりお待ちしております。

>[!NOTE]
>
>これは、マルチチャンネル・ビデオ・プログラミング・ディストリビューター（MVPD）向けのキックスタート・ガイドです。 プログラマー（コンテンツプロバイダー）の場合は、「[ プログラマー向けキックスタートガイド ](/help/authentication/kickstart/programmer-kickstart-guide.md)」を参照してください。

サポートは、Zendesk のAdobe Pass認証チケットシステムを通じていつでも利用できます。 また、ここには、プロセスに関するサンプル、ドキュメント、ビデオチュートリアルも含まれています。 [Zendesk](https://adobeprimetime.zendesk.com/) を使用するには、https://tve.zendesk.com/homeでアカウントを登録して作成する必要があります。 登録できるユーザー数や、提出されたチケットに対して表示またはコメントを投稿できるユーザー数に制限はありません。 すべてのサポートに関する質問は、次のアドレスに問い合わせる必要があります。tve-support （adobe.com）

**チームの連絡先**:

**サポート** – すべての質問、インシデント、機能リクエストに対して **tve-support@adobe.com**。

## 1.キックオフ会合 {#kickoff-meetings}

これらの会合の場は、AdobeとMVPDの間の技術的な協議の開始から構成されている。 プロセスのこの時点で、ドキュメントは両側から共有する必要があります。 フォローアップとして、Adobeはチケットをチケット発行システム（https://tve.zendesk.com/）で開き、統合のステータスをトラッキングする必要があります。

## 2.機能 {#features}

Adobeは、統合の全体的なスケジュール、手順、タイムライン、実装の詳細について話し合い、追跡するために、毎週ステータスコールを設定します。 このフェーズでは、AdobeはMVPDの仕様を確認します。 この結果として、MVPDに必要なすべての機能の詳細を示す仕様ページが得られます。 MVPDから、MVPDの認証/承認の実装について詳しく記載した仕様書がAdobeに送信されます。

詳しくは、[MVPD統合機能 ](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md) を参照してください。

この時点で、いくつかの設定について詳しく説明する必要があります。

* **MVPDのロゴ URL** – これは、112 x 33 ピクセルの大きさのファイルです。 ロゴは、ユーザーが「サインイン」ボタンをクリックして有料テレビプロバイダーを選択すると、プログラマーがサイトに表示されます。
* **TTL （time-to-live）値** - TTL は通常、認証/承認プロセス中にMVPDによって設定されます。 ただし、Adobeは、これらの TTL 値を上書きし、プログラマーとMVPDの両方が同意した内容に応じて異なる値を提供できます。
* **表示名** - ユーザーが「サインイン」ボタンをクリックして有料テレビプロバイダーを選択すると、プログラマーがサイトに表示されます。
* **テスト資格情報** – 両方のプロファイル（ステージングおよび実稼動）に、テスト資格情報のリストが必要です。

>[!IMPORTANT]
>
>ユーザーが使用権限フローを開始するたびに、単一の不透明な一意のユーザー ID に関連付けられます。  ユーザー ID は、プログラマーのアプリのユーザーを識別するために使用されますが、送信元はMVPDです。

>[!CAUTION]
>
>各MVPDに関する重要な注意事項：ユーザー ID には、PII （個人を特定できる情報）、単独で使用できる情報、またはユーザーを識別、連絡、またはユーザーを特定するためにその他の情報と共に使用できる情報を含めないでください。

## 2.メタデータの交換 {#metadata-ex}

両者は、関係するすべての環境（実稼働、ステージングなど）のメタデータを交換する必要があります。

* **Adobe**
   * ステージング環境の場合、Adobeの SP メタデータは、次の場所から取得できます。[ 認証ステージング SP メタデータ ](https://sp.auth-staging.adobe.com/sp/metadata)
   * 本番環境のAdobeの SP メタデータは、次の場所から取得できます。[ 本番 SP メタデータの認証 ](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * メタデータ（ステージング/実稼動）を追加するには：

## 4. IP 許可リスト {#allow-ip-list}

次の IP は、MVPDのファイアウォールで許可リストに登録する必要があります。 IP のリストについては、Adobeにお問い合わせください。

* Adobe Pass Authentication では、制限されたリソースへのアクセスを許可するために、ポート 80 および 443 でファイアウォールを開く必要があります。

* MVPDは、認証サーバーと承認サーバー（この場合に限る）の IP アドレスのリストを追加する必要があります。

## 5.開発 {#deve}

開発フェーズの期間は、仕様を確認し、双方が承認する項目を考慮して決定されます。 Adobeは、認証部分のカスタムコードを記述する必要があります。

>[!NOTE]
>
>承認はリソースごとに実行されます。 認証トランザクションは、通常、ユーザーが認証を要求しているチャネルを表す ID 文字列をプログラマーのサイトから渡して実行されます。 このリソース ID は、プログラマーとMVPDの間で確立され、必要に応じて詳細に設定できます。

## 6.Adobe環境 {#adobe-env}

Adobeは、開発プロセスの様々なステージに対して異なる環境を提供します。

* **事前選定** （PRE-QUAL）: PRE-QUAL 環境には、次のリリース候補が含まれています。 最初にAdobeはこの環境で新しいパートナーと統合し、その後リリース環境にアップグレードします。 パートナーは PRE-QUAL 環境で 2 週間テストし、PRE-QUAL 設定に対する変更を明示的にリクエストする必要があります（変更リクエストプロセスについて詳しくは、Adobe担当者にお問い合わせください）。 バグ修正：この環境の新しいデプロイメントをトリガーします。
* **リリース** （リリース）:Adobeの現在の実稼動ビルドが、ここで実稼動環境にデプロイされます。

Adobeの環境の使用方法について詳しくは、「[Adobe環境について ](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md) を参照してください。

## 7. ステージングデプロイメント {#stag-env}

MVPDから受け取ったメタデータに基づいて、Adobeは、Adobe Pass認証システムで新しいMVPDを作成し、設定します。 これは、Adobeの以前のステージング環境にデプロイされ、テストプログラマー（TestDistributors）と共に設定されます。

MVPDは、QA/ステージング/テスト環境で同じデプロイメントを行う必要があります。

## 8. テストとトラブルシューティング {#tes-troubleshoot}

このフェーズでは、AdobeとMVPDは、統合をテストし、トラブルシューティングを行います。 Adobeのテストに役立てるため、Adobe Pass認証チームは、統合の API テストサイトを使用できます。 Adobeの API テストサイトの使用について詳しくは、[AdobeAPI テストサイトを使用した認証および承認フローのテスト ](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md) を参照してください。

テストとトラブルシューティングが正常に終了したら、Adobeのリリースステージング環境で統合が有効になります。 この時点で、AdobeはMVPDを実際のプログラマーと統合できます。

## 9.実稼動デプロイメント {#prod-dep}

* 接続をテストするには、まずMVPDを実稼動プロファイルにデプロイする必要があります。

* Adobeは初期状態の実稼働にデプロイされます。

* すべての関係者が実稼動プロファイルでテストできるようになりました。

* この時点ですべてが正常な場合、Adobeは、すべてのユーザーが利用できるリリース実稼動環境（「ライブ」）に統合を移行できます。

## 10. エスカレーション手順 {#esc-proc}

統合が実稼動環境に移行したら、最適なカスタマーエクスペリエンス（顧客体験）を提供することが重要です。 サーバのダウンの問題に最適な対応を行うために、MVPD は、Adobeが注意を向けた問題に関するエスカレーション手順のドキュメントを提供する必要があります。

次に、Adobeは、最新のAdobe Pass認証エスカレーションプロセスを MVPD に提供します。


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
