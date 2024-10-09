---
title: 暗号化用のユーザーメタデータ証明書
description: 暗号化用のユーザーメタデータ証明書
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a2
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 暗号化用のユーザーメタデータ証明書

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

暗号化されたユーザーメタデータのAdobe Pass認証統合の場合は、秘密鍵と公開鍵のペアが必要です。

ここでは、Adobe Pass認証で使用する公開鍵証明書を生成するプロセスについて説明します。 ここで説明するプロセスは、OpenSSL ツールキットを利用します。

## 証明書生成プロセスのチュートリアル（#generation）

1. OpenSSL ツールキット（http://www.openssl.org）をダウンロードしてインストールします。

1. 証明書署名リクエスト（CSR）を生成します。

   * キーペアを生成します。  コマンド/ターミナルウィンドウを開き、次のコマンドを実行します。

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSR を生成します。 コマンドラインで次を実行します。

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     秘密鍵のパスワードを入力するよう求められます。

   * 秘密鍵とパスワードのバックアップコピーを作成します。 サンプル CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. CSR を認証局（CA）（例：Verisign）に送信します。

1. CA から.p7b 形式（PKCS#7, Cryptographic Message Syntax Standard）で証明書が送信されます

1. .p7b 証明書をデプロイします。 秘密鍵を使用して PKCS#7 （.p7b）ファイルを PKCS#12 （PFX ファイル、Personal Information Exchange Syntax Standard）に変換し、PEM ファイル（連結証明書コンテナファイル）を生成します。

   * PKCS#7 ファイルを一時 PEM ファイルに変換します。 コマンドラインで次を実行します。

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 一時 PEM ファイルを PFX ファイルに変換します。  コマンドラインで次を実行します。

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 一時 PEM ファイルを最終 PEM ファイルに変換します。 コマンドラインで次を実行します。

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. 最終的な PEM ファイルをAdobeに送信して、設定を完了させます。

   * 最終的に PEM ファイルを受け取る必要があるのは、統合/検証に割り当てられたAdobeイネーブルメントエンジニアです。 そのユーザーと直接作業していない場合は、Adobeの担当者からファイルの送信先を調べることができます。
   * Adobeは、プライマリ証明書とバックアップ証明書の両方をサポートしています。 プライマリ証明書が侵害された場合は、その証明書を失効させ、セカンダリ証明書に切り替えて、アプリのリクエスター ID に署名できます。 これにより、お客様への影響を最小限に抑えながら、実稼働環境で証明書間をスムーズに移行できます。
   * Adobeが PEM ファイルを受け取ると、認証エンジニアはサーバー側の設定に PEM ファイルを追加し、ファイルの受信を確認します。
