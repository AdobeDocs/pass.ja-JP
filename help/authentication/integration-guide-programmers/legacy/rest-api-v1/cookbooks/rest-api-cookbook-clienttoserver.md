---
title: REST API クックブック（クライアントからサーバー）
description: Rest API クックブッククライアントからサーバーへ。
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# （レガシー） REST API クックブック（クライアントからサーバー） {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要 {#overview}

このドキュメントでは、プログラマーのエンジニアリングチームが、REST API サービスを使用して「スマートデバイス」（ゲームコンソール、スマート TV アプリ、セットトップボックスなど）をAdobe Pass認証と統合する手順を順を追って説明します。 このクライアントからサーバーへのアプローチは、クライアント SDKではなく REST API を使用するので、一意の SDK を数多く開発することは不可能な、様々なプラットフォームの幅広いサポートが可能になります。 クライアントレスソリューションの仕組みに関する技術的な概要については、[ クライアントレス技術概要 ](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md) を参照してください。


このアプローチでは、必要なフローを完了するために 2 つのコンポーネント（ストリーミングアプリと AuthN アプリ）が必要です。ストリーミングアプリでの起動、登録、承認、ビューメディアフローと、AuthN アプリでの認証フローです。

### スロットルメカニズム

Adobe Pass認証 REST API は、[ スロットルメカニズム ](/help/authentication/integration-guide-programmers/throttling-mechanism.md) によって制御されます。

## コンポーネント {#components}

動作しているクライアントからサーバーへのソリューションには、次のコンポーネントが関係しています。



| タイプ | コンポーネント | 説明 |
| --- | --- | --- |
| ストリーミングデバイス | ストリーミングアプリ | ユーザーのストリーミングデバイス上に存在し、認証済みビデオを再生するプログラマーアプリケーション。 |
| | \[ オプション\] AuthN モジュール | streaming Device にユーザーエージェント（Web ブラウザー）がある場合、MVPD IdP 上でユーザーの認証は AuthN モジュールが行います。 |
| \[ オプション\] AuthN デバイス | AuthN アプリ | ストリーミングデバイスにユーザーエージェント（Web ブラウザー）がない場合、AuthN アプリケーションは、Web ブラウザーを使用して別のユーザーのデバイスからアクセスする、プログラマー向けの Web アプリケーションです。 |
| Adobe インフラストラクチャ | Adobe Pass サービス | MVPD IdP および AuthZ サービスと統合され、認証と承認の決定を行うサービス。 |
| MVPD インフラストラクチャ | MVPD IdP | ユーザーの ID を検証するために、資格情報ベースの認証サービスを提供するMVPD エンドポイント。 |
| | MVPD AuthZ サービス | ユーザーの購読、保護者による制限などに基づいて認証の決定を行うMVPD エンドポイント。 |

## フロー{#flows}

### 動的クライアント登録（DCR）

Adobe Passは、DCR を使用して、プログラマーアプリケーションまたはサーバーとAdobe Pass サービスの間のクライアント通信を保護します。 DCR フローは独立しており、[Dynamic Client Registration Overview](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) ドキュメントで説明されています。


### ストリーミング（スマートデバイス）アプリフロー

![](../../../../assets/smart-device-app-flow.png)

#### 起動フロー

1. アプリが起動し、最初の UI が読み込まれます。

2. デバイス ID を取得または生成します。

3. Check-authentication 呼び出しを発行して、デバイスがすでに認証されているかどうかを確認します。  例：[`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. `checkauthn` 呼び出しが成功した場合は、手順 2 以降の認証フローに進みます。  失敗した場合、登録フローを開始します。



#### 登録フロー

1. ユーザーが 2nd screen ログインアプリへのアクセスに使用する登録コードと URL を取得し、ユーザーに提示します。

   a. ハッシュ化されたデバイス ID と「登録 URL」を渡して、Adobe Registration Code Service に POST リクエストを送信します。  例：[`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b.返された登録コードと URL をユーザーに提示します。

   c. Web 対応デバイスに切り替えるようにユーザーに指示し、URL に移動して、登録コードを入力します。



#### 認証フロー

1. ユーザーは 2 番目の画面アプリから戻り、デバイスの「続行」ボタンを押します。 または、ポーリングメカニズムを実装して認証ステータスを確認することもできますが、Adobe Pass認証では、ポーリングよりも「続行」ボタンを使用することをお勧めします。 <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)--> 例：[\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. GET リクエストをAdobe Pass Authentication Authorization サービスに送信して、認証を開始します。 例：`<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* 応答が成功を示している場合：ユーザーに有効な AuthN トークンがあり、ユーザーがリクエストされたメディアを視聴する権限がある（このユーザーには有効な AuthZ トークンがあります）。

* 応答が失敗を示している場合：スローされた例外を調べて、そのタイプ（AuthN、AuthZ など）を特定します。

   * AuthN エラーの場合は、登録フローを再開します。

   * AuthZ エラーの場合、ユーザーは要求されたメディアを監視する権限を持っていません。ユーザーに何らかのエラーメッセージが表示されるはずです。

   * その他のエラー（接続エラー、ネットワークエラーなど）が発生した場合は、適切なエラーメッセージをユーザーに表示します。



#### メディアフローを表示

1. メディアの選択肢を提示する。 ユーザーが表示するメディアを選択します。

2. メディアは保護されていますか？

   a. アプリがメディアが保護されているかどうかを確認します。

   b. メディアが保護されている場合、アプリは認証を開始します
（AuthZ）上記のフロー

   c. メディアが保護されていない場合は、メディアを再生する
ユーザー。

3. メディアを再生します。


### AuthN （2 番目の画面）アプリのフロー

![](../../../../assets/secnd-screen-authn-flow.png)

1. このユーザーの MVPD のリストを取得します。 例：[`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. 認証フローを開始します。  例：[`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. 認証が成功したかどうかを確認します。 例：[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. ユーザーをスマートデバイスアプリに送り返して、認証フローを完了します。

## パートナーのシングル サインオン {#partner-sso}

一部のデバイスは、パートナーのシングルサインオン （SSO）に対する専用のサポートを提供しています。

* [APPLE SSO](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## Platform のシングルサインオン {#platform-sso}

一部のデバイスは、Platform シングルサインオン（SSO）用の専用サポートを提供しています。

* [AMAZON SSO](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)

## REST API の TempPass およびプロモーション TempPass {#temppass}

ユーザーが資格情報を入力する必要がない TempPass およびプロモーション TempPass 実装の場合、認証をストリーミングアプリに直接実装できます。

**この API を使用するには、ストリーミングアプリでデバイス ID が一意であることを確認する必要があります。これは、オプションの追加データと共に、トークンの識別に使用されるためです。**


![](../../../../assets/temp-pass-promo-temppass.png)
