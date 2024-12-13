---
title: サーバーサイド指標について
description: サーバーサイド指標について
exl-id: 516884e9-6b0b-451a-b84a-6514f571aa44
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2232'
ht-degree: 0%

---

# サーバーサイド指標について {#understanding-server-side-metrics}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。


## 概要 {#intro}

このドキュメントでは、Entitlement Service Monitoring （ESM）サービスによって生成されるAdobe Pass認証サーバーサイドの指標について説明します。 クライアントサイドの観点から見た場合と同じイベント（ページやアプリケーションにAdobe Analyticsなどの測定サービスを実装する場合にプログラマーに表示されるイベント）ではありません。

## イベントの概要 {#events_summary}

Adobe Pass認証サーバー側の観点から、次のイベントが生成されます。

* **認証フローで発生するイベント** （MVPD による実際のログイン）

   * AuthN 試行の通知 – ユーザーが MVPD ログインサイトに送信されたときに生成されます。
   * AuthN 保留の通知 – ユーザーが MVPD でログインに成功した場合、これはユーザーが        Adobe Pass認証にリダイレクトされます。
   * Notification of AuthN Granted - ユーザーがプログラマーのサイトに戻り、Adobe Pass Authentication から認証トークンを正常に取得したときに生成されます。
* **認証フロー** （単に
MVPD）\
  *前提条件：* 有効な AuthN トークン
   * AuthZ 試行の通知
   * Notification ofAuthZ Granted
* **再生リクエストの成功**\
  *前提条件：* 有効な AuthN および AuthZ トークン
   * Adobe Pass Authentication を使用したチェックの通知
   * 再生リクエストには、許可された認証と許可された認証の両方が必要です


ユニークユーザーの数について詳しくは、以下の [ ユニークユーザー ](#unique-users) の節を参照してください。 概して、付与された認証応答と承認応答は通常キャッシュされるので、次の式が通常適用されます。

* AuthN の試行回数\> AuthN の付与回数
* AuthZ の試行回数\> AuthZ の付与回数
* AuthZ の試行回数\> 許可された AuthN の回数（通常）
* 成功した再生リクエストの数\> 許可された AuthZ の数


### 例 {#example}

次の例は、1 か月間のサーバーサイド指標を示しています
1 つのブランド：

| 指標 | MVPD 1 | MVPD 2 | ... | MVPD n | 合計 |
| -------------------------- | ------ | ------ | - | ------ | ---------------------------------------------- |
| 成功した認証 | 1125 | 2892 |   | 2203 | SUM （MVP1+...MVPD n） |
| 成功した承認 | 2527 | 5603 |   | 5904 | SUM （MVP1+...MVPD n） |
| 成功した再生リクエスト | 4201 | 10518 |   | 10737 | SUM （MVP1+...MVPD n） |
| Unique Users | 1375 | 2400 |   | 2890 | すべての重複除外された MVPD のすべてのユーザーの合計\* |
| 認証試行 | 2147 | 3887 |   | 3108 | SUM （MVP1+...MVPD n） |
| 認証試行済み | 2889 | 6139 |   | 6039 | SUM （MVP1+...MVPD n） |

</br>

異なる MVPD ユーザーが同じユーザー ID を受け取らないように、重複排除を行っても効果はありません。 2 つの異なるブランドで同じ MVPD に対して合計を行う場合、重複除外効果ははるかに大きくなる必要があります。

## イベントのトリガー {#event_triggers}

### 新規ユーザー – フルフロー {#new-user-full-flow}

次のグラフは、認証トークンがないユーザー（新しいユーザーまたは認証トークンの有効期限が切れているユーザー）のイベントと手順を示しています。



![](../../../assets/ae-flow-with-events.png)



このフローでは、認証（\#7 への#5）と認証（\#11）の両方で MVPD へのラウンドトリップが必要です。



フローが完了すると、認証トークンと認証トークンがユーザーのデバイスにキャッシュされます。 認証トークンの有効期間（TTL）の値は、6 時間～90 日です。 AuthN トークンの有効期限が切れると、AuthZ トークンの有効期限が自動的に強制されます。 認証トークンの TTL 値は通常 24 時間です。

| サーバーサイドイベントがトリガーされる | <ul><li>認証の試行、認証の保留、認証の許可</li><li>承認の試行、承認が付与されました</li><li>成功した再生リクエスト</li></ul> |
|---|---|


### 再来訪ユーザー – キャッシュされた AuthZ および AuthN トークン

有効な AuthZ および AuthN トークンをキャッシュしているユーザーの場合は、以下を行います
実行される手順：


![](../../../assets/ae-flow-tokens-cached-web.png)



これは `getAuthorization()` の呼び出し時に自動的にトリガーされ、Adobe Pass Authentication でのチェックのみが必要です。 MVPD はこのフローには関与しません。


| サーバーサイドイベントがトリガーされる | *再生リクエストの成功 |
|---|---|


### 再来訪ユーザー – AuthN トークンがキャッシュされ、AuthZ トークンが期限切れです

有効な AuthN トークンをまだ持っているユーザーの場合、次の手順が行われます。

![](../../../assets/ae-flow-authn-token-cached.png)


このフローは、MVPD への往復を含みます。


| サーバーサイドイベントがトリガーされる | <ul><li>認証の試行、認証 OK</li><li>成功した再生リクエスト</li> |
|---|---|

## 認証イベント {#authn_events}

### 認証の試行 {#authentication-attempt}

上の図に示すように、認証イベントは、ユーザーが MVPD にラウンドトリップした場合にのみトリガーされます。認証イベントには、キャッシュされたトークン認証は含まれません。

認証試行イベントは、ユーザーがピッカーから特定の MVPD をクリックした後にトリガーされます。

* これに近い MVPD 側の最初のイベントは、ページの読み込みです
* Adobe Pass認証で、ユーザーが MVPD ページにログインする繰り返し試行がカウントされません（パスワードが正しくありません。もう一度試してください）
* 複数の試行は 1 回としてカウントされます
* 一部の MVPD では、認証手順で認証が実行され、認証が失敗した場合にユーザーがリダイレクトされません。

### 認証を保留中 {#authentication-pending}

このイベントは、Adobe Pass認証へのリダイレクトプロセスが開始されたときに発生します。

### Authentication Granted {#authentication-granted}

ユーザーは MVPD の既知の加入者であり、通常は有料のテレビ購読を持っていますが、時にはインターネットアクセスのみがあります。 認証が成功する理由は、ユーザーが MVPD を使用して有効な認証情報を明示的に入力したか、以前に有効な認証情報を入力し、「記憶する」をオンにしていたか（以前のセッションの有効期限が切れていなかった）です。

したがって、MVPD は、Adobe Pass Authentication に認証リクエストに対する肯定的な応答を送信し、Adobe Pass Authentication は *AuthN トークン* を作成します。

* 通常、認証は長期間（1 か月以上）キャッシュされます。 これにより、トークンの有効期限が切れてフローが再開されるまで、認証イベントは表示されなくなります。
* シングルサインオンを介して別のサイトやアプリから受信した場合、認証イベントはトリガーされません。



### マルチキャスト認証 {#comcast-authentication}

Comcast には、他の MVPD とは異なる AuthN フローがあります。

次の機能で違いを説明します。

* **セッション cookie の動作**：これにより、ユーザーがブラウザーを閉じた後に、認証トークンが完全に削除されます。 この機能は web にのみ存在します。 主な目的は、Comcast セッションが安全でない/共有されたコンピューターで保持されないようにすることです。 その影響は、他の MVPD に比べて、認証の試行や許可されたフローが多くなることです。

* **要求者 ID ごとの AuthN**: Comcast では、要求者 ID 間で AuthN 状態をキャッシュすることはできません。 このため、各サイト/app は Comcast にアクセスして認証トークンを取得する必要があります。 ユーザーエクスペリエンスに関する考慮事項だけでなく、前述のように、より多くの認証試行/許可されたイベントが生成されるという影響があります。

* **パッシブ認証**：ユーザーエクスペリエンスを向上させるためですが、
requestorID ごとの AuthN 機能は引き続き維持されますが、非表示の iFrame ではパッシブ認証フローが発生します。 ユーザーには何も表示されませんが、イベントは引き続き以前と同様にトリガーされます。

ユーザーが Comcast ログインページ上の「自分を記憶する」をクリックすると、その後の（2 週間の）このページへの訪問は、単なるクイックリダイレクトになります。 そうでない場合、ユーザーは実際にはそのページで認証を行う必要があります。

### 認証に失敗しました {#unsuccessful-authentication}

認証の失敗は、Adobe Pass認証のイベントそのものではありませんが、試行と成功の違いとして計算できます。

2013 年 5 月のリリースでは、DRM エラー（トークンバインディングに失敗しました）、LSO エラー（トークンを書き込むスペースがありません）など、システムエラーまたはネットワークエラーが原因の認証失敗のエラーコードがAdobe Pass認証によって追加されます。

### 認証コンバージョン率 {#authenitication-conversion-rate}

プログラマーが追跡できる興味深い指標の 1 つは、（AuthN リクエスト/AuthN 許可された）パーセントとして計算される認証コンバージョン率です。

指標に関する注意事項：

* これはイベントベースの指標なので、ユニークユーザーのコンバージョン率は実際には反映されません。つまり、ユーザーが 8 回試行して 9 回目に成功した場合、上記のコンバージョン率にひどく反映されます。
* Adobe Pass認証（サーバーサイド）では、一意のベースの認証変換を計算する方法はありません。
* サイト / アプリに自動 AuthN 再試行が存在する場合は、上記の指標もゆがみます。

## 認証イベント {#authorization_events}

### 認証の試行 {#authorization_attempt}

認証トークンの取得に加えて、ユーザーはコンテンツを再生する前に認証トークンも取得する必要があります。 これは通常、認証後、または認証トークンの有効期限が切れた場合に発生します。 このチェックは（Adobe Pass認証サーバーから MVPD サーバーまで）サーバーサイドで行われるので、ユーザーが何もしなくても構いません。

### 付与された認証 {#authorization-granted}

「認可された」信号は、認証されたユーザの購読が要求されたプログラミングを含むことを示す。

なお、すべての MVPD が個別の認証手順をサポートしているわけではありません。一部の認証は認証と同じです。 MVPD は、Adobe Pass Authentication にバックチャネル AuthZ リクエストへの成功したレスポンスを送信し、Adobe Pass Authentication は AuthZ トークンを作成します。

* AuthZ トークンは一定の期間（通常は 24 時間）キャッシュされます。この期間中、AuthZ イベントは発生しません。
* 一部の MVPD はアセットレベルの権限で動作し、他の MVPD はチャネルレベルの権限で動作します。どちらを使用するかによって、発生する AuthZ イベントの数は異なります。 チャネルレベルの認証の場合でも、キャッシュは行われているので、同じアセットが 24 時間以内にリクエストされた場合、イベントは発生しません。

### 認証が拒否されました {#authorization-denied}

認証が拒否された場合、認証済みユーザーには、要求されたプログラミングに対する確認されたサブスクリプションがありません。 最も可能性の高い原因は、チャネルがユーザーのサブスクリプションパッケージに含まれていないことですが、MVPD からのインターネットアクセスのみを持つユーザーが反映されている可能性もあります。

一部の MVPD では、ユーザーは MVPD からのインターネット購読（有料 TV 購読なし）しかなくても、正常に認証されます。 この場合、ユーザーが認証をリクエストするチャネルがベースパッケージ内にある場合でも、認証は拒否されます。

一部の MVPD は、AuthZ 拒否に対してカスタムエラーメッセージを提供し、パッケージのアップグレードのオファーを含めることができます。


### 認証コンバージョン率 {#authorization-conversion-rate}

認証コンバージョン率は、（AuthZ リクエスト/AuthZ 付与） % として計算できます。

### 成功した再生リクエスト {#successful-play-request}

認証済みおよび承認済みのユーザーは、保護されたコンテンツを表示できます。

再生リクエストが成功すると、Adobe Pass認証は、リクエストされたビデオを表示する権限がユーザーに付与されていることをアサートする、短期間有効なメディアトークンを生成します。 プログラマーは、このメディアトークンを使用して、将来のビューアをさらに検証します。 メディアトークンは、成功した再生リクエストとして追跡されます。

* Adobe Pass Authentication は、メディアトークンの生成後に *ビデオの再生が実際に開始されたかどうかをトラッキングし* せん。 例えば、コンテンツに地域制限がある場合、ストリームが実際には開始されなくても、トランザクションは引き続き成功した再生リクエストとしてカウントされます。
* AuthN および AuthZ トークンは、MVPD 応答を一定期間キャッシュするので、成功した再生リクエストイベントは指標で最も頻繁に発生するイベントです。

## Unique Users {#unique-users}

### 定義 {#definition}

認証に成功すると、Adobe Pass Authentication は、返された MVPD User ID 値に基づいて一意のユーザーの存在を追跡します。  この値はユーザーのログイン情報に基づいていますが、個人を特定できる情報は含まれていません。

この値も sendTrackingData コールバックのサイト/アプリに渡されます。

この値は、デバイス間で永続的に使用できます（MVPD は、ログインが発生した場所に関係なく、特定のユーザーに対して同じ値を生成します）。または一時的な値で使用されます（各ログインに対して、新しい値が生成され、MVPD がバックエンドでマッピングします）。 通常、MVPD からAdobe Pass認証に提供される値は、セッションやデバイスをまたいで永続的ですが、前述のように、永続性は保証も検証もされません。

この値は、一意のユーザーを計算する方法として使用されます。 レポートされた値（リクエスター ID/インターバル/MVPD ごと）は、特定のインターバルについて重複排除されます。 したがって、通常、1 日あたりの一意のユーザーの合計は月額の値とは異なり、月額の値はより低い値になります。

この数には、Adobe Pass認証からのすべてのイベントから、ユーザー ID を持たない認証試行を差し引き、試行された（および失敗した）認証を含みます。

### 例 {#examples}

#### 1 日目 {#day1}

ユーザー XYZ はビデオを視聴するためにサイトに移動します。

発生したイベント：

* AuthN 試行（一意のユーザーはまだありません）
* AuthN が付与されました
   * この時点で、MVPD が返す値に基づいてユーザーを一意に識別するので、1 日あたりのユニークユーザー数が 1 増えます
   * authN トークンは 30 日間キャッシュされます
* AuthZ 試行/許可されたイベント
   * 1 日間キャッシュされた AuthZ トークン
* 再生リクエストイベントの成功

#### 1 日目（後日） {#day1-later-on}

ユーザー XYZ が別のビデオを視聴します。

発生したイベント：

* 再生リクエストイベントの成功（残りはキャッシュされます）
* 日次または月次のユニーク数は増加しません

#### 3 日目 {#day3}

ユーザー XYZ が別のビデオを視聴します。

発生したイベント：

* AuthZ 試行/許可されたイベント
   * 1 日目から 1 日間のキャッシュが期限切れになりました
* 再生リクエストイベントの成功（残りはキャッシュされます）
* 日別ユニークユーザー数が 1 増加 – 月別のユニーク数は引き続き 1 です

#### 31 日目 {#day31}

ユーザー XYZ が別のビデオを視聴します。

AuthN キャッシュの有効期限が切れてから 1 日目と同じです。

同じユーザーが認証に失敗した場合でも、ユーザー ID を含むイベント（許可された認証と認証試行）が 2 つあるので、月間ユニークユーザー数は 1 増加します。

### シングルサインオン（SSO） {#single-sign-on-sso}

一意のユーザーの数が、成功した認証の数よりも多くなる場合があります。 これは通常、多くのユーザーが他のサイトやアプリから SSO を介して入ってきて、現在のサイトやアプリで認証を受ける必要があるだけの場合です。

### クライアントサイドとサーバーサイドの一意のユーザーの比較 {#comparing-client-side-and-server-side-unique-users}

`sendTrackingData()` のユーザー ID 値をクライアントサイドで使用して一意のユーザーをカウントする場合は、クライアントサイドとサーバーサイドの番号が一致する必要があります。

大きな違いが生じる場合、次の理由が考えられます
相違点：

* ビデオ再生のユニーク数とすべてのイベントのユニーク数。 前述のように、Adobe Pass認証では、AuthN の試行を除くすべてのイベントについて、一意のユーザーがカウントされます。 つまり、ユーザーが（ページ上で）認証のみを行い、ビデオを表示しない場合でも、一意のユーザー数の増加がトリガーされます。

* 承認に失敗したユーザーのカウント - Adobe Pass認証は、報告された数で同様にこれらのユーザーをカウントします。

<!--
## Related Information {#related-information}

- [Entitlement Service Monitoring API](/help/authentication/entitlement-service-monitoring-api.md)

-->