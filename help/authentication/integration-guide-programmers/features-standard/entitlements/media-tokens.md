---
title: メディアトークン
description: メディアトークン
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 0%

---

# メディアトークン {#media-tokens}

>[!IMPORTANT]
>
> このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

メディアトークンは、保護されたコンテンツ（リソース [&#x200B; に対する表示アクセスを提供するための認証決定の結果として、Adobe Pass認証 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)REST API V2）によって生成されるトークンです。

メディアトークンは、問題の時点で指定された制限された短い期間（デフォルトは 7 分）有効で、クライアントアプリケーションで検証および使用される必要がある前の時間制限を示します。 メディアトークンは 1 回限りの使用に制限され、絶対にキャッシュしないでください。

メディアトークンは、クリアテキストで送信される公開鍵インフラストラクチャ（PKI）に基づく署名済み文字列で構成されます。 PKI ベースの保護機能を使用すると、トークンは、証明機関（CA）によってAdobeに発行された非対称キーを使用して署名されます。

メディアトークンはプログラマーに渡され、その後、ビデオストリームを開始する前にメディアトークン検証機能を使用して検証し、そのリソースへのアクセスのセキュリティを確保できます。

メディアトークンベリファイアは、Adobe Pass認証によって配布されるライブラリで、メディアトークンの信頼性の検証を担当します。

## メディアトークン検証子 {#media-token-verifier}

Adobe Pass Authentication では、ビデオストリームを開始する前に安全にアクセスできるよう、メディアトークン検証機能ライブラリを統合してメディアトークンを独自のバックエンドサービスに送信することをお勧めします。 メディアトークンの有効期間（TTL）は、トークン生成サーバーと検証サーバーの間で発生する可能性のあるクロック同期の問題を考慮するように設計されています。

トークン形式は保証されず、将来的に変更される可能性があるので、Adobe Pass認証ではメディアトークンの解析とそのデータの直接抽出に対する強いアドバイスを行います。 メディアトークン検証者ライブラリは、トークンの内容を分析するために使用される唯一のツールである必要があります。

メディアトークン検証機能ライブラリは、次のリンクからダウンロードできます。

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

メディアトークンベリファイライブラリでは、JDK バージョン 1.5 以降が必要で、署名アルゴリズム（`SHA256WithRSA`）用に優先される Java 暗号化拡張機能（JCE）プロバイダーの使用がサポートされています。

`mediatoken-verifier-VERSION.jar` Java アーカイブによって表されるメディアトークン検証者ライブラリには、次のものが含まれます。

* Adobe公開鍵。
* トークン検証 API （`ITokenVerifier.java`）。
* 参照実装（`com.adobe.entitlement.test.EntitlementVerifierTest.java`）。
* 依存関係と証明書キーストア。

>[!IMPORTANT]
> 
> 含まれる証明書キーストアのデフォルトのパスワードは `123456` です。

### メソッド {#methods}

`ITokenVerifier` クラスは、次のメソッドを定義します。

* メディアトークンの検証に使用する `isValid()` メソッド。 単一の引数 [&#x200B; リソース識別子 &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier) を受け入れます。 指定されたリソース識別子が `null` の場合、メソッドはメディアトークンの信頼性と有効期間のみを検証します。

  `isValid()` メソッドは、次のいずれかのステータス値を返します。

  | VALID_TOKEN | トークンの検証に成功しました |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | トークン形式が無効です |
  | INVALID_SIGNATURE | トークンの信頼性を検証できませんでした |
  | TOKEN_EXPIRED | トークン TTL が無効です |
  | INVALID_RESOURCE_ID | 指定されたリソースのトークンが無効です |
  | ERROR_UNKNOWN | トークンはまだ検証されていません |

* メディアトークンに関連付けられたリソース識別子を取得し、認証決定応答から返された識別子と比較するために使用される `getResourceID()` メソッド。

* メディアトークンが発行された時刻を取得するために使用された `getTimeIssued()` メソッド。

* メディアトークンの TTL を取得するために使用される `getTimeToLive()` メソッド。

* MVPDによって設定された匿名化 GUID を取得するために使用される `getUserSessionGUID()` メソッド。

* ユーザーを認証したMVPDの識別情報を取得するために使用される `getMvpdId()` メソッド。

* ユーザーを認証したプロキシMVPDの識別子を取得するために使用される `getProxyMvpdId()` メソッド。

### サンプル {#sample}

メディアトークン検証者アーカイブには、参照実装（`com.adobe.entitlement.test.EntitlementVerifierTest.java`）と、テストクラスを使用した API の呼び出し例が含まれています。 このサンプル（`com.adobe.entitlement.text.EntitlementVerifierTest.java`）では、メディアトークンベリファイアライブラリをメディアサーバーに統合する方法を説明しています。

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

メディアトークンは、次の API を使用して取得できます。

* [特定の mvpd を使用した認証決定の取得](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

認証決定とメディアトークンの構造については、上記 API の **Response** および **Samples** の節を参照してください。

>[!IMPORTANT]
>
> [&#x200B; メディアトークン &#x200B;](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md) は、ユーザーアクセスを許可する認証決定に既に含まれているので、クライアントアプリケーションは、別のエンドポイントをクエリして取得する必要はありません。

上記の API を統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [&#x200B; 承認フェーズに関するよくある質問 &#x200B;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
