---
title: メディアトークン
description: メディアトークン
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 6fdd77e300d01269357d7917702f31aef35d6751
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%
---
# メディアトークン {#media-tokens}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

メディアトークンは、保護されたコンテンツ（リソース）への表示アクセスを提供するための認証決定後に、Adobe Pass認証[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)によって生成されるトークンです。

メディアトークンは、問題の時点で指定された制限付き短い期間（デフォルトは7分）有効であり、クライアントアプリケーションで検証して使用する前の時間制限を示します。 メディアトークンは1回限りの使用に制限されており、キャッシュしないでください。

メディアトークンは、クリアテキストで送信される公開鍵基盤（PKI）に基づく署名済み文字列で構成されます。 PKI ベースの保護では、トークンは、認証局（CA）によってAdobeに発行された非対称キーを使用して署名されます。

メディアトークンはプログラマーに渡され、プログラマーはビデオストリームを開始する前にメディアトークン検証ツールを使用してメディアトークンを検証し、そのリソースへのアクセスのセキュリティを確保します。

Media Token Verifierは、メディアトークンの信頼性を検証するAdobe Pass Authenticationによって配布されるライブラリです。

## Media Token Verifier {#media-token-verifier}

ビデオストリームを開始する前に安全なアクセスを確保するために、Adobe Pass Authenticationでは、Media Token Verifier ライブラリを統合する独自のバックエンドサービスにMedia Tokenを送信することをお勧めします。 メディアトークンのTTL （Time-to-Live）は、トークン生成サーバーと検証サーバー間の潜在的なクロック同期の問題を考慮するように設計されています。

Adobe Pass Authenticationは、メディアトークンのフォーマットが保証されておらず、将来的に変更される可能性があるため、メディアトークンを解析し、そのデータを直接抽出することを強くお勧めします。 Media Token Verifier ライブラリは、トークンのコンテンツを分析するために使用される唯一のツールである必要があります。

次のリンクからMedia Token Verifier ライブラリをダウンロードします。

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

Media Token Verifier ライブラリにはJava バージョン 8以降が必要で、署名アルゴリズム （`SHA256WithRSA`）に優先Java Cryptography Extension （JCE） プロバイダーの使用をサポートしています。

Media Token Verifier ディストリビューションには、次のものが含まれます。

* Adobe公開鍵。
* トークン検証API （`ITokenVerifier.java`）。
* 参照実装（`com.adobe.entitlement.test.EntitlementVerifierTest.java`）。
* 証明書キーストア （`media_token_keystore.jks`）。
* `mediatoken-verifier-VERSION-application-with-dependencies.jar`のランタイム依存関係。 シン `mediatoken-verifier-VERSION.jar`には、これらの依存関係が個別に必要です。

>[!IMPORTANT]
> 
> 含まれる証明書キーストアのデフォルト パスワードは`123456`です。

>[!IMPORTANT]
>
> REST API V1およびV2から返された`serializedToken`は、Base64 エンコードされています。 `getInstance(...)`に渡す前にUTF-8としてデコードします。それ以外の場合、検証は`INVALID_SIGNATURE`を返します。 以下のサンプルは、エンコードされた値を受け入れて、このデコードを実行します。

### メソッド {#methods}

`ITokenVerifier` インターフェイスは、次のメソッドを定義します。

* メディアトークンの検証に使用される`isValid()` メソッド。 単一の引数[ リソース識別子](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)を受け入れます。 指定されたリソース識別子が`null`の場合、メソッドはメディアトークンの真正性と有効期間のみを検証します。

  `isValid()` メソッドは、次のいずれかのステータス値を返します。

  | ステータス | 意味 |
  |----------------------|-------------------------------------------|
  | VALID_TOKEN | トークンの検証に成功しました |
  | INVALID_TOKEN_FORMAT | トークン形式が無効です |
  | INVALID_SIGNATURE | トークンの信頼性を検証できませんでした |
  | TOKEN_EXPIRED | トークン TTLが無効です |
  | INVALID_RESOURCE_ID | トークンは、指定されたリソースに対して無効です |
  | ERROR_UNKNOWN | トークンはまだ検証されていません |

* メディアトークンに関連付けられているリソース IDを取得し、承認決定応答から返された識別子と比較するために使用される`getResourceID()` メソッド。

* メディアトークンが発行された時間を取得するために使用される`getTimeIssued()` メソッド。

* メディアトークンのTTLをミリ秒単位で取得するために使用される`getTimeToLive()` メソッド。

* MVPDによって設定された匿名化されたGUIDを取得するために使用される`getUserSessionGUID()` メソッド。

* ユーザーを認証したMVPDのIDを取得するために使用される`getMvpdId()` メソッド。

* ユーザーを認証したプロキシ MVPDのIDを取得するために使用される`getProxyMvpdId()` メソッド。

### サンプル {#sample}

Media Token Verifier アーカイブには、参照実装（`com.adobe.entitlement.test.EntitlementVerifierTest.java`）と、テストクラスでAPIを呼び出す例が含まれています。 このサンプル （`com.adobe.entitlement.test.EntitlementVerifierTest.java`）は、Media Token Verifier ライブラリをメディア サーバーに統合する方法を示しています。

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.entitlement.verifier.SignatureVerificationCredential;
import java.io.InputStream; 
import java.nio.charset.StandardCharsets;
import java.util.Base64;

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
        tokenToVerify = new String(Base64.getDecoder().decode(tokenToVerify),
                StandardCharsets.UTF_8);
        System.out.println("RequestorID: " + requestorID);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
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
            try (InputStream certInputStream =
                    EntitlementVerifierTest.class.getClassLoader().getResourceAsStream(keystoreFile)) {
                // load the keystore file 
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null || !cred.isValid()) {
            throw new IllegalStateException("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

メディアトークンは、次のAPIを使用して取得できます。

* [特定のmvpdを使用して認証の決定を取得する](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

承認決定とメディアトークンの構造については、上記のAPIの&#x200B;**応答**&#x200B;および&#x200B;**サンプル**&#x200B;のセクションを参照してください。

>[!IMPORTANT]
>
> クライアントアプリケーションは、ユーザーアクセスを許可する承認決定に既に含まれているため、[ メディアトークン ](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)を取得するために別のエンドポイントをクエリする必要はありません。

上記のAPIを統合する方法とタイミングについて詳しくは、次のドキュメントを参照してください。

* [プライマリアプリケーション内で実行される基本的な認証フロー](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [承認フェーズに関するFAQ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
