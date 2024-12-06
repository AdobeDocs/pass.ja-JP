---
title: トラッキング防止の評価：Apple Safari
description: トラッキング防止の評価：Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 0%

---

# トラッキング防止の評価 – Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## Safari 10 {#safari10}

**詳細**

Safari 10 以降、デフォルトのブラウザープライバシー設定により、シングルサインオン（SSO）、シングルログアウト（SLO）、パッシブ認証機能が動作を停止します。 でも、シングルサインオン（SSO）とパッシブ認証は機能しません
複数のタブまたはブラウザーウィンドウ間の同じセッション。

これらの変更は、Adobe Passの認証プロセスに影響を与え、その影響を受けています
AccessEnabler JavaScript SDK の v2 （バージョン 2.x）、v3 （バージョン 3.x）、v4 （バージョン 4.x）の各バージョンについて説明します。

### 軽減 {#mitigation-safari10}

これらの制限を軽減するために、以下の画像に示すように、Safari 10 ブラウザープライバシー設定を変更し、環境設定からブラウザーのプライバシータブの「**Cookie と Web サイトデータ**」エントリに対して「**常に許可**」オプションを使用するようにユーザーに指示できます。

![](../assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**詳細**

>[!IMPORTANT]
>
>Safari 10 のセクションからの上記の詳細はすべて、Safari 11 の場合は引き続き適用されます。

Safari 11 以降、ブラウザーは、クロスサイトトラッキングを防ぐためにヒューリスティックを使用する技術 [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) （ITP）メカニズムを導入します。 これらのヒューリスティックは、サードパーティ cookie の保存およびネットワーク呼び出し時の再生方法に影響します。つまり、ITP メカニズムのアクティベーションに応じて、Safari ブラウザーはクライアント – サーバーモデル通信でサードパーティ cookie をブロックします。

Adobe Pass Authentication サービスは、（機能するために **認証プロセスの一部として Cookie を使用し、これに依存し** す。 認証プロセスが自動的に実行される状況（Temp Pass など）や、iFrames または「リフレスレス」機能を使用する実装では、Adobeの Cookie はサードパーティ Cookie と見なされ、デフォルトでブロックされます。 その他のケースでは、Safari は機械学習アルゴリズムを使用して、すべてのAdobeーのパス認証サービス Cookie にトラッキング Cookie としてフラグを立て、ITP のブロックの対象となる可能性があります。

最後に、Safari 11 ブラウザーのユーザーは、Intelligent Tracking Prevention （ITP）メカニズムのアクティブ化後、特にユーザーが複数のAdobe Pass認証が有効な web サイトを使用している場合、Adobe Pass認証が有効な web サイトで認証できない可能性があります。 したがって、ユーザーの認証操作は、予期せず未定義である可能性があります。例えば、ログインできない場合や、予想される認証時間よりも短い場合などです。

これらの変更は、AccessEnabler JavaScript SDK の v2 （バージョン 2.x）、v3 （バージョン 3.x）の各バージョンのAdobe Pass認証プロセスに影響を与え、その影響を受けています。

### 軽減 {#mitigation-safari11}

AccessEnabler JavaScript SDK v3 （バージョン 3.x）および AccessEnabler JavaScript SDK v4 （バージョン 4.x）の両方について、ライブラリには、必要な Cookie がないためにユーザーの認証がブロックされた状況を特定するためのメカニズムが含まれています。 このような状況で、ライブラリは特定のエラーコールバック [N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference) をトリガーし、Adobe Pass認証が有効な web サイトに返されて、問題を軽減できるアクションを実行するようにユーザーに指示するシグナルとして使用されます。 このメカニズムを活用するために、web サイトでは [ エラーレポート ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) 仕様を実装する必要があります。

AccessEnabler JavaScript SDK v2 （バージョン 2.x）の場合、ライブラリでは前述のメカニズムが提供されないので、問題を軽減するアクションを実行するようユーザーに指示する際に、Adobe Pass認証が有効な Web サイトを通知することができません。

AccessEnabler JavaScript SDK の前述の問題 **3 つのバージョンすべてに適用** を軽減できるアクションのリストです。

[N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference) エラーコールバックが実装者の web サイトによって受信された場合は、次の方法で Intelligent Tracking Prevention （ITP）を無効にし、サードパーティ Cookie を有効にするようにユーザーに指示する必要があります。

* Mac OS X High Sierra 以降の場合：以下の図に示すように、ブラウザーの環境設定から、ブラウザーのプライバシータブにある「**Website tracking**」エントリに対する「**クロスサイトトラッキングを防ぐ**」オプションのチェックを外します。

  ![](../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* Mac OS X Sierra およびそれ以前の場合：以下の図に示すように、環境設定からブラウザーのプライバシータブの「**Cookies and website data**」エントリに対して「**Always allow**」オプションをオンにします。

  ![](../assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**詳細**

>[!IMPORTANT]
>
>Safari 10 および Safari 11 のセクションからの上記の詳細はすべて、Safari 12 の場合に引き続き適用されます。

ここでは、Safari 12 における **AccessEnabler JavaScript SDK バージョン 4.x** の互換性の問題について説明します。

>[!NOTE]
>
>AccessEnabler JavaScript SDK version 2.x および AccessEnabler JavaScript SDK version 3.x の場合、両方ともサードパーティ Cookie を認証処理に使用します。Safari 11 以降の ITP およびサードパーティ Cookie ポリシーにより、ログインできないなど、予想されない認証時間や予想されない認証時間など、ユーザーの認証体験が未定義になる場合があります。


### Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4} ージでの AccessEnabler JavaScript SDK v4 （バージョン 4.x）の認定機能

**認証** ユーザーの操作を利用するフローは、ユーザーのブラウザーでサードパーティ Cookie が無効になっていても常に機能します。これは、バージョン 4.0 以降、AccessEnabler JavaScript SDK が認証プロセスにサードパーティ Cookie を使用しなくなったためです。

>[!NOTE]
>
>ユーザーは、ログインポップアップを開いたり、MVPD ログインページを操作したりするために、サイトとやり取りする必要があります。

**認証/プリフライト/ユーザーメタデータ** ユーザーが既に認証されている場合、操作は完全に機能します。

### Safari 12 での AccessEnabler JavaScript SDK v4 （バージョン 4.x）の既知の問題 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO と SLO

   * Safari 10 以降の Safari における localStorage の実装により、JS SDK は共通ドメイン iFrame を介してログイン状態を共有できなくなりました。 つまり、AccessEnabler JavaScript SDK を使用するすべてのサイトにログインする必要があります。 また、ログアウトしてもサイト間で認証トークンが削除されないので、Adobe Pass認証が有効な各 web サイトからログアウトする必要があります。

* 一時パス

   * 一時的なパスの場合、AccessEnabler JavaScript SDK は個別化メカニズムを使用して、特定のデバイス（ブラウザーインスタンス）に認証トークンをロックします。 トラッキングを防ぐように設計された Safari 12 の新しいメカニズムにより、個人化メカニズムで計算および使用している指紋 **同じ IP アドレスを持つすべてのユーザーで同じになります**。 クライアントの IP は個人に合わせたものとして扱いますが、同じパブリック IP アドレスを共有するユーザーに対する影響は大きくなります。 これらのユーザーについては、同じ個人化 ID を計算し、一時パスがそれに関連付けられます。 つまり、そのようなユーザーが一時パスを使用すると、他のユーザーは誰もアクセスできなくなります\! これは、特に企業ユーザー、教育機関、または NAT または共通のプロキシを使用してインターネットにアクセスする複数のユーザーを持つその他の組織に影響を与えます。

>[!NOTE]
>
>この問題がユーザーに影響するのは、ユーザー操作の結果として実装担当者が一時パスを使用した場合のみです。それ以外の場合、一時パス認証は以下の **自動フロー** の対象となります。

* 自動フロー

   * JS SDK 4.0 を使用している場合、Safari 12 では、自動モードで試行された認証フローは、ユーザーのインタラクションがないと成功しません。今後の JS SDK 4.1 では、自動フローに関するすべての問題が修正されます。

この問題の影響を受ける使用例：

* 自動 TempPass （無料プレビュー）認証 – このようなフローの場合、SDK は N130 エラーをスローします。

* パッシブ認証（サイレントに失敗）：ユーザーはこの MVPD を選択し、認証情報を入力するよう求められます

### 軽減 {#mitigation-safari12}

**SSO と SLO**

このドキュメントの作成時点では、軽減策は利用できないか、利用できる可能性もありません。 Appleでは、Safari 12 （`https://webkit.org/blog/8124/introducing-storage-access-api`）で「ストレージアクセス API」を導入しましたが、現在の実装は localStorage には適用されず、cookie にのみ適用されます。 さらに、API を使用するにはユーザーの操作が必要です。また、API を使用すると、以下のような権限ダイアログも表示されます。

![](../assets/permission-dialog-apple.png)


この時点では、これらの Safari の要件/プロンプトは UX の要件と一致せず、共通ドメイン localStorage にトークンを保存すると SSO が「機能」する他のブラウザーの場合とは異なります。

**一時パス**

個人化の問題を軽減し、ユーザーとのインタラクションを実現するために、**[プロモーション一時パス](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)** をインタラクティブな方法で使用し、ユーザーに関する 1 つ以上の追加情報（メールアドレスなど）を提供することをお勧めします。

## Safari 13 {#safari13}

**詳細**

>[!IMPORTANT]
>
>Safari 10 のセクションから Safari 12 のセクションまでのすべての詳細は、Safari 13 の場合も引き続き適用されます。


Safari 13 以降、ブラウザーは [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) （ITP）に新しい変更を導入し、クロスサイトトラッキングを防ぐためにサードパーティ cookie にトラッキング cookie のフラグを設定するプロセスで、メカニズムの背後にあるヒューリスティックを強化しました。

前の節で説明したように、Adobe Pass Authentication サービスでは、AccessEnabler JavaScript SDK v2 （バージョン 2.x）と AccessEnabler JavaScript SDK v3 （バージョン 3.x）を実装者が使用する場合の認証プロセスの一部として、サードパーティ Cookie を使用してそれに依存します。 ユーザーと関係者（プログラマーの web サイトやAdobe）の間のやり取りについて「学ぶ」ために少し時間を費やした後に ITP が立ち上がった以前のバージョンの Safari ブラウザーと比較して、Safari 13 ブラウザーは、クライアント – サーバーモデルの通信で Cookie のトラッキングと見なされるサードパーティ Cookie を最初からブロックしています。

最後に、Safari 13 ブラウザーのユーザーは、古いバージョンの AccessEnabler JavaScript SDK、v2 （バージョン 2.x）または v3 （バージョン 3.x）を使用しているAdobe Pass認証が有効な web サイトで新しい認証を開始できない可能性が高くなります。 これは、必要なAdobeの Primetime Authentication Service Cookie がすべて ITP によってブロックされ、サービスが認証リクエストを満たすことができないことが原因で発生します。

AccessEnabler JavaScript SDK v4 （バージョン 4.x）ライブラリでは、認証プロセスにサードパーティ Cookie を使用しないので、Safari 13 の変更点によって、その処理は影響を受けません。

### 軽減 {#mitigation-safari13}

まず第一に、Safari ブラウザーで安定した予測可能な動作を実現するには、**AccessEnabler JavaScript SDK バージョン 4.x への移行** を強くお勧めします。

次に、AccessEnabler JavaScript SDK v3 （バージョン 3.x）の場合、必要な Cookie がないためにユーザー認証がブロックされた状況を特定するメカニズムが含まれます。 このような状況で、ライブラリは特定のエラーコールバック（[N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference)）をトリガーし、Adobe Pass認証が有効な web サイトに返されて、問題を軽減できるアクションを実行するようにユーザーに指示するシグナルとして使用されます。 このメカニズムを活用するために、web サイトでは [ エラーレポート ](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) 仕様を実装する必要があります。

AccessEnabler JavaScript SDK v2 （バージョン 2.x）の場合、ライブラリでは前述のメカニズムが提供されないので、問題を軽減するアクションを実行するようユーザーに指示する際に、Adobe Pass認証が有効な Web サイトを通知することができません。

[N130](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md#advanced-error-codes-reference) エラーコールバックが実装者の web サイトによって受信された場合は、次の方法で Intelligent Tracking Prevention （ITP）を無効にし、サードパーティ Cookie を有効にするようにユーザーに指示する必要があります。

* Mac OS X High Sierra 以降の場合：以下の図に示すように、ブラウザーの環境設定から、ブラウザーのプライバシータブにある「**Website tracking**」エントリに対する「**クロスサイトトラッキングを防ぐ**」オプションのチェックを外します。

  ![](../assets/prvnt-cross-site-tr-safari13.png)

* Mac OS X Sierra およびそれ以前の場合：以下の図に示すように、環境設定からブラウザーのプライバシータブの「**Cookies and website data**」エントリの「**Always allow**」オプションをオンにします </span>

  ![](../assets/always-allow-safari13.png)
