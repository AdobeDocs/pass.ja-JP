---
title: CMU API アクセス
description: CMU API アクセス
source-git-commit: 30631ac006b7944cb2eb8996c2c165343b6be5fe
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---

# 同時実行監視使用状況 API アクセス {#cmu-api-usage-access}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。 可用性に関するご質問は、Adobe担当者にお問い合わせください。

## アクセス手順の概要 {#api-access-procedure-overview}

CMU レポートのアクセスを更新し、OAuth 2.0 Dynamic Client Registration Protocol との互換性を確保しました。
同時実行監視アプリケーションのニーズに対応するために、カスタム OAuth 2.0 認証サーバーがデプロイされます。 \
クライアントアプリケーションで OAuth 2.0 認証を利用するには、サーバーが動的に登録して、OAuth 2.0 認証とやり取りできるようにする必要があります。 登録プロセスの一環として、クライアントは組み込みメタデータのセットをクライアント登録エンドポイントに提示する必要があります。
このメタデータは、ソフトウェアステートメントとして伝達されます。このステートメントには、「software_id」が含まれ、同じソフトウェアステートメントを使用してアプリケーションの異なるインスタンスを関連付けるための認証サーバーが使用できます。
ソフトウェアステートメントは、クライアントソフトウェアに関するメタデータ値をバンドルとしてアサートする JSON Web トークン (JWT) です。 クライアント登録要求の一環として認証サーバーに提示される場合、ソフトウェア文は JSON Web Signature(JWS) を使用してデジタル署名または MACed する必要があります。 \
ソフトウェアステートメントの内容と仕組みに関する詳細な説明は、公式ドキュメントを参照してください。  <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
次の節の手順に従って、アクセス権を取得します。

## アクセス手順 {#access-procedure-steps}

1. Adobe Pass DCR サーバーに登録済みのアプリケーションがある。 この手順については、 [サポートチーム](mailto:tve-support@adobe.com).
2. ソフトウェアステートメントを取得する
   1. TVE ダッシュボードに移動 <a href="https://console-preprod.auth.adobe.com/#!/" target="_blank"> Pre-Prod </a> または <a href="https://console.auth.adobe.com/" target="_blank">PROD</a>
   2. プログラマーを選択
   3. 「アプリケーション」タブに移動します。
   4. アプリケーションを選択
   5. 「DownLoad Software Statement」をクリックして、以下のキャプチャを含むファイルを取得します。
      <figure>
          <img src="assets/software_statement_1_download.png"
               alt="ソフトウェア文のダウンロード">
       </figure>
      <figure>
          <img src="assets/software_statement_2.png"
               alt="ソフトウェアステートメントの例">
       </figure>

3. アクセストークンの取得
   1. 上記で取得したソフトウェア文を使用し、以下の呼び出しを実行して、クライアントの資格情報を取得します。 この方法で、 client_id と client_secret のペアが取得され、アクセストークンの取得に使用できます。
      *この手順は、毎回実行しないでください。 資格情報が期限切れになった場合にのみ、再度おこなう必要があります。*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="クライアント資格情報の取得">
       </figure>

   2. 以下の呼び出しを使用してアクセストークンを取得します。 このアクセストークンを使用して、トークンが期限切れになるまで CMU API を呼び出します。
      *この手順は、最後に生成されたトークンの有効期限が切れた場合にのみ実行する必要があります。*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="アクセストークンを取得">
       </figure>

4. CMU API を呼び出す — 以下の関連情報を参照してください。
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="CMU API を呼び出す">
       </figure>

## 関連情報 {#related-information}

* [CMU の概要](/help/concurrency-monitoring/cm-usage-reports.md)
* [CMU API](/help/concurrency-monitoring/cmu-api.md)
