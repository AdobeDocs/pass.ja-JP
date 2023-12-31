---
title: Dynamic Client Registration Management
description: Dynamic Client Registration Management
exl-id: 2c3ebb0b-c814-4b9e-af57-ce1403651e9e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 0%

---

# Dynamic Client Registration Management {#dynamic-client-registration-management}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

～の普及に伴い [Android Chrome カスタムタブ](https://developer.chrome.com/multidevice/android/customtabs){target_blanck} および [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target_blanck} お客様のアプリケーションでは、Adobe Pass Authentication のユーザー認証フローを更新しています。 具体的には、状態の維持という目標を達成できなくなり、MVPD 加入者を認証するユーザーエージェントフローをリダイレクト間で追跡できるようになりました。 これは、以前は HTTP Cookie を使用しておこなわれていました。 この制限は、すべての API を OAuth 2.0 に移行し始めるためのドライバーです [RFC6749](https://tools.ietf.org/html/rfc6749){target_blanck}.

この更新により、Adobe認証クライアントが OAuth 2.0 クライアントになり、Adobe Pass認証サービスのニーズに対応するために、カスタム OAuth 2.0 認証サーバーがデプロイされます。

クライアントアプリケーションで OAuth 2.0 認証を利用するには、サーバーが動的に登録して、OAuth 2.0 認証とやり取りできるようにする必要があります。 登録プロセスの一環として、クライアントは組み込みメタデータのセットをクライアント登録エンドポイントに提示する必要があります。

このメタデータは、ソフトウェアステートメントとして伝達されます。このステートメントには、「software_id」が含まれ、同じソフトウェアステートメントを使用してアプリケーションの異なるインスタンスを関連付けるための認証サーバーが使用できます。

A **ソフトウェア文書** は、クライアントソフトウェアに関するメタデータ値をバンドルとしてアサートする JSON Web トークン (JWT) です。 クライアント登録要求の一環として認証サーバーに提示される場合、ソフトウェア文は JSON Web Signature(JWS) を使用してデジタル署名または MACed する必要があります。

ソフトウェアステートメントの内容と仕組みに関する詳細な説明は、公式ドキュメントを参照してください。 [RFC7591](https://tools.ietf.org/html/rfc7591).

ソフトウェア文は、ユーザーのデバイス上のアプリケーションと共にデプロイする必要があります。

この更新以前は、アプリケーションがAdobe Pass Authentication を呼び出し実行できるようにする 2 つのメカニズムがありました。

* ブラウザーベースのクライアントは、許可されたを介して登録されます。 [ドメインリスト](/help/authentication/programmer-overview.md#reg-and-init)
* iOSや Android アプリケーションなどのネイティブアプリケーションクライアントは、を通じて登録されます。 **署名した要求者** 機構


クライアント登録認証メカニズムでは、アプリケーションを TVE ダッシュボードに追加する必要があります。

お客様が新しい Android SDK と今後のiOS SDK の実装を開始するには、ソフトウェア文が必要です。 ソフトウェア文は、TVE ダッシュボードで作成されたアプリケーションを識別します。

TVE ダッシュボードで登録済みアプリケーションを作成するには、以下のセクションの手順に従います。

## 登録済みアプリケーションの作成 {#create_app}

TVE ダッシュボードで登録済みアプリケーションを作成するには、次の 2 つの方法があります。

* [プログラマーレベル](#prog-level)  — 登録済みアプリケーションを作成し、任意のまたはすべてのプログラマーチャネルにリンクできます。

* [チャネルレベル](#channel-level)  — このチャネルのみに永続的にリンクされる登録アプリケーションを作成できます。

### プログラマーレベルでの登録済みアプリケーションの作成 {#prog-level}

に移動します。 **プログラマー** > **登録済みアプリ** タブをクリックします。

![](assets/reg-app-progr-level.png)

[ 登録済みアプリケーション ] タブで、 **新しいアプリケーションを追加**. 新しいウィンドウの必須フィールドに入力します。

以下の画像に示すように、入力が必要なフィールドは次のとおりです。

* **アプリ名**  — アプリケーションの名前

* **チャネルに割り当て済み**  — チャネルの名前 (t)</span>このアプリケーションがリンクされている場所。 ドロップダウンマスクのデフォルト設定は次のとおりです。 **すべてのチャネル。** インターフェイスでは、1 つのチャネルまたはすべてのチャネルを選択できます。

* **アプリケーションのバージョン**  — デフォルトでは「1.0.0」に設定されていますが、独自のアプリケーションバージョンを使用して変更することを強くお勧めします。 ベストプラクティスとして、アプリケーションのバージョンを変更する場合は、新しく登録されたアプリケーションを作成して、そのバージョンを反映させます。

* **アプリケーションプラットフォーム**  — アプリケーションがリンクされるプラットフォーム。 すべての値または複数の値を選択することもできます。

* **ドメイン名**  — リンク先のアプリケーションのドメイン。 ドロップダウンリスト内のドメインは、すべてのチャネルからすべてのドメインを統合して選択したものです。 リストから複数のドメインを選択することもできます。 ドメインの意味は、リダイレクト URL です [RFC6749](https://tools.ietf.org/html/rfc6749). クライアント登録プロセスでは、クライアントアプリケーションは、認証フローの最終化にリダイレクト URL を使用することを許可するように要求できます。 クライアントアプリケーションが特定のリダイレクト URL を要求すると、ソフトウェアステートメントに関連付けられたこの登録済みアプリケーションにホワイトリストに登録されているドメインに対して検証されます。


![](assets/new-reg-app.png)


フィールドに適切な値を入力したら、「完了」をクリックして、アプリケーションを設定に保存する必要があります。

次の点に注意してください。 **作成済みのアプリケーションを変更するオプションはありません**. 作成されたアプリケーションが要件を満たさなくなった場合は、新しい登録済みアプリケーションを作成し、要件を満たすクライアントアプリケーションで使用する必要があります。


### チャネルレベルでの新しいアプリケーションの登録 {#channel-level}

チャネルレベルで登録済みアプリを作成するには、「チャネル」メニューに移動し、アプリを作成するアプリを選択します。 次に、「登録済みアプリ」タブに移動した後、「新しいアプリを追加」ボタンをクリックします。

![](assets/reg-new-app-channel-level.png)

以下に示すように、ここでは、プログラマーレベルで実行したのと同じアクションとは少し異なる点は、「割り当てられたチャネル」ドロップダウンです。このドロップダウンは有効ではないので、登録されたアプリケーションを現在のチャネル以外にバインドするオプションはありません。

![](assets/new-reg-app-channel.png)

## アプリのリスト {#list-reg-app}

登録されたアプリケーションを作成した後、ソフトウェアステートメントを取得して、認証サーバをリクエストの一部として提示することができます。

これは、登録済みアプリケーションが作成されたプログラマーまたはチャネルに移動し、そのアプリケーションが表示されることで実行できます。

以下に示すように、リスト内のすべてのエントリは、バインド先のプラットフォームの名前、バージョンおよび記号で識別されます。

![](assets/reg-app-list.png)

それぞれに対して、次の操作を実行できます。

* [表示](#view)
* [ソフトウェアステートメントをダウンロード](#download-statement)

### 登録済みアプリケーションの表示 {#view}

アプリケーションのリストで、いずれかを選択し、「表示」ボタンをクリックすると、作成時に使用した詳細が表示されます。 前述のように、何も変更するオプションはありません。


![](assets/view-reg-app.png)


### ソフトウェア文のダウンロード {#download-statement}

ソフトウェアステートメントが必要なリストエントリの「ダウンロード」ボタンをクリックすると、テキストファイルが生成されます。 このファイルには、次のサンプル出力のような内容が含まれます。


![](assets/download-software-statement.png)

ファイルの名前は、ファイルの前に「software_statement」を付け、現在のタイムスタンプを追加することで一意に識別されます。

同じ登録済みアプリケーションでは、ダウンロードボタンがクリックされるたびに異なるソフトウェア文が受け取られますが、このアプリケーションで以前に取得したソフトウェア文は無効になりません。 これは、アクションリクエストごとに、その場で生成されるからです。

一人ある **制限** ダウンロードアクションに関する情報。 登録済みアプリケーションを作成した直後に「ダウンロード」ボタンをクリックしてソフトウェア文が要求され、これがまだ保存されておらず、設定 json が同期されていない場合は、次のエラーメッセージがページの下部に表示されます。

![](assets/error-sw-statement-notready.png)

これは、登録されたアプリケーションの ID がまだ伝播されておらず、コアがそれを知らないため、コアから受け取った HTTP 404 Not Found エラーコードをラップします。

解決策は、登録されたアプリケーションを作成した後、設定が同期されるまで最大 2 分待つことです。 この後、エラーメッセージは受信されなくなり、ソフトウェアステートメントを含むテキストファイルをダウンロードできるようになります。

エンドツーエンドプロセスの仕組みの詳細、またはリクエストの実行方法と期待される応答についてのインサイトを得るには、以下の関連情報のリンクと他の役に立つリンクを参照してください。

<!--
## Related Information {#related}

* [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
* [TVE Dashboard User Guide](/help/authentication/tve-dashboard-user-guide.md)
-->

## 機能のデモ {#tutorial}

ご覧ください [このウェビナー](https://my.adobeconnect.com/pzkp8ujrigg1/) これは、機能の詳細なコンテキストを提供し、 TVE Dashboard を使用して software 文を管理する方法と、Android SDK の一部としてAdobeが提供するデモアプリケーションを使用して生成された文をテストする方法に関するデモを含みます。
