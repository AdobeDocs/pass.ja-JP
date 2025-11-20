---
title: CMU API アクセス
description: CMU API アクセス
exl-id: 8d216703-aabc-489e-93fe-d4d105616b1d
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 0%

---

# 同時実行性の監視の使用 API アクセス {#cmu-api-usage-access}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。 可用性に関するご質問については、Adobe担当者にお問い合わせください。

## アクセス手順の概要 {#api-access-procedure-overview}

OAuth 2.0 動的クライアント登録プロトコルと互換性を持たせるために、CMU レポートへのアクセスを更新しました。 同時実行性監視アプリケーションのニーズに対応するために、カスタム OAuth 2.0 認証サーバーがデプロイされます。 \
クライアントアプリケーションが OAuth 2.0 認証を利用するには、サーバーが操作できるように特定の情報（クライアント資格情報）を取得するために、サーバーが動的に登録される必要があります。 登録プロセスの一環として、クライアントは組み込みメタデータのセットをクライアント登録エンドポイントに提示する必要があります。
このメタデータは、ソフトウェアステートメントとして通信されます。このステートメントには「software_id」が含まれ、承認サーバーが同じソフトウェアステートメントを使用してアプリケーションの異なるインスタンスを関連付けることができます。
ソフトウェアステートメントは、クライアントソフトウェアに関するメタデータ値をバンドルとしてアサートする JSON web トークン（JWT）です。 クライアント登録リクエストの一環として認証サーバーに提示する場合、ソフトウェアのステートメントは、JSON web 署名（JWS）を使用してデジタル署名または MAC 化する必要があります。 \
ソフトウェアのステートメントとその仕組みについて詳しくは、公式ドキュメント <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a> を参照してください。
以下の節の手順に従って、アクセス権を取得します。

## アクセス手順 {#access-procedure-steps}

1. Adobe Pass DCR サーバーにアプリケーションを登録してある。 この手順については、アドビの [ サポートチーム ](mailto:tve-support@adobe.com) にお問い合わせください。

2. ソフトウェアのステートメントを取得
   1. [Adobe Pass TVE ダッシュボード ](https://experience.adobe.com/#/pass/authentication) に移動
   2. プログラマーを選択
   3. *登録済みアプリケーション* タブに移動します
   4. アプリケーションを選択
   5. ソフトウェア文を取得する登録済みアプリケーション行で「ダウンロード」をクリックし、ローカルマシンにファイルとして保存します
      <figure>
          <img src="../assets/programmer-download-software-statement-button.png"
               alt="ソフトウェアのダウンロード ステートメント">
      </figure>

      <figure>
          <img src="../assets/software_statement_2.png"
               alt="ソフトウェア明細書のサンプル">
      </figure>

3. アクセストークンの取得
   1. 上記で取得したソフトウェア ステートメントを使用し、次の呼び出しを実行して、クライアント資格情報を取得します。 このようにして、client_id と client_secret のペアが取得され、アクセストークンを取得するために使用できます。
      *この手順は、毎回実行しないでください。 この操作は、資格情報の有効期限が切れた場合にのみ実行してください。*
      <figure>
          <img src="../assets/dcr_request_1_get_client_credentials.png"
               alt="クライアント資格情報の取得">
       </figure>

   2. 次の呼び出しを使用してアクセストークンを取得します。 このアクセストークンを使用して、トークンの有効期限が切れるまで任意の CMU API を呼び出します。
      *この手順は、最後に生成されたトークンの有効期限が切れた場合にのみ実行する必要があります。*
      <figure>
          <img src="../assets/dcr_get_access_token_call.png"
               alt="アクセストークンを取得">
       </figure>

4. CMU API を呼び出す – 以下の関連情報を参照してください。
   <figure>
          <img src="../assets/call_cmu_reports_sample.png"
               alt="Cmu API の呼び出し">
       </figure>

## 関連情報 {#related-information}

* [CMU の概要](/help/concurrency-monitoring/reports/cm-usage-reports.md)
* [CMU API](/help/concurrency-monitoring/reports/cmu-api.md)
