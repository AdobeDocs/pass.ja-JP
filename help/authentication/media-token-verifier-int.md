---
title: メディアトークンベリファイアの統合
description: メディアトークンベリファイアの統合
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '887'
ht-degree: 0%

---

# メディアトークンベリファイアの統合

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## メディアトークンベリファイアについて {#about-media-token-verifier}

認証に成功すると、Adobe Pass認証は永続的認証（AuthZ）トークンを作成します。  AuthZ トークンは、クライアントのプラットフォームに応じて、クライアントサイドに渡されるか、サーバーサイドに保存されます。  （様々なクライアントシステムでのトークンの保存方法とその他の詳細については、[ トークンについて ](/help/authentication/programmer-overview.md#understanding-tokens) を参照してください。）


AuthZ トークンは、特定のリソースの表示をサイトのユーザーに許可します。  これは通常、6～24 時間の有効期間（TTL）で、その後トークンの有効期限が切れます。 **実際の表示アクセスの場合、Adobe Pass認証は AuthZ トークンを使用して、取得してメディアサーバーに渡す短期間有効なメディアトークンを生成します**。 これらの短時間のみ有効なメディアトークンには、非常に短い TTL （通常は数分）があります。


AccessEnabler の統合では、`setToken()` コールバックを使用して、短時間のみ有効なメディア・トークンを取得します。 クライアントレス API 統合の場合は、`<SP_FQDN>/api/v1/tokens/media` API 呼び出しで短期間有効なメディアトークンを取得します。 トークンは、PKI （公開鍵インフラストラクチャ）に基づくトークン保護を使用して、Adobeが署名したクリアテキストで送信される文字列です。 この PKI ベースの保護では、トークンは非対称キーを使用して署名され、証明機関によってAdobeに発行されます。


トークンのクライアント側には検証がないので、悪意のあるユーザーがツールを使用して偽の `setToken()` 呼び出しを挿入する可能性があります。 したがって **ユーザーが認証されているかどうかを検討する際に** トリガーされ `setToken()` という事実に単純に依存することはできません。 短期間有効なトークン自体が正当であることを検証する必要があります。 検証を実行するツールは、メディアトークン検証者ライブラリです。


>[!TIP]
>
>検証用に、返されるトークン文字列の長さ全体をメディアトークン検証子に渡す必要があります。

## メディアトークン検証機能を使用した短時間のみ有効なトークンの検証 {#validate-short-livedttokens}

実際にビデオストリームを開始する前にトークンを検証するために、プログラマーはメディアトークン検証者ライブラリを使用する web サービスにトークンを送信することをお勧めします。 短時間のみ有効なメディアトークンの非常に短い TTL は、トークンを生成するサーバーとトークンを検証するサーバーとの間のクロック同期の問題に対応するために十分に長く定義されますが、それ以上は長くなります。



[ メディアトークンベリファイアーライブラリ ](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue){target=_blank} は、Adobe Pass認証パートナーが利用できます。



メディアトークン検証者ライブラリは、Java アーカイブ `mediatoken-verifier-VERSION.jar` ージに含まれています。 ライブラリは次の項目を定義します。

* トークン検証 API （`ITokenVerifier` インターフェイス）、JavaDoc ドキュメント
* トークンが実際にAdobeから取得されたことを検証するために使用されるAdobe公開鍵
* Verifier API の使用方法と、ライブラリに含まれるAdobe公開鍵を使用してオリジンを検証する方法を示す参照実装（`com.adobe.entitlement.test.EntitlementVerifierTest.java`）


アーカイブには、すべての依存関係と証明書キーストアが含まれています。 含まれる証明書キーストアのデフォルトのパスワードは、「123456」です。

* 検証ライブラリには JDK バージョン 1.5 以降が必要です。
* 署名アルゴリズム「SHA256WithRSA」には、好みの JCE プロバイダーを使用します。


**検証者ライブラリは、トークンの内容を分析するために使用される唯一の手段である必要があります。 トークンの形式は保証されず、今後も変更される可能性があるので、プログラマーはトークンを解析して、データを自分で抽出しないでください。** Verifier API のみが正しく機能することが保証されています。 文字列を直接解析すると、一時的に機能する場合がありますが、今後、形式が変更される可能性があり、問題が発生する可能性があります。 Verifier API は、トークンから次のような情報を取得します。

* トークンは有効ですか（`isValid()` メソッド）。
* トークンに関連付けられたリソース ID （`getResourceID()` メソッド）。これは、`setToken()` 関数コールバックの他のパラメーターと比較できます（また、一致する必要があります）。 一致しない場合は、不正行為を示している可能性があります。
* トークンが発行された時刻（`getTimeIssued()` メソッド）。
* TTL （`getTimeToLive()` メソッド）。
* MVPD から受信した匿名化された認証 GUID （`getUserSessionGUID()` メソッド）。
* ユーザを認証したディストリビュータの ID。その場合は、ディストリビュータに認証を提供したプロキシ MVPD。

## Verifier API の使用 {#using-verifier-api}

`ITokenVerifier` クラスは、特定のリソースのトークンの信頼性を検証するために使用するメソッドを定義します。 `ITokenVerifier` のメソッドを使用して、`setToken()` リクエストに応答して受信したトークンを分析します。


`isValid()` メソッドは、トークンを検証する主な手段です。 1 つの引数（リソース ID）を使用します。 Null リソース ID を渡した場合、メソッドはトークンの信頼性と有効期間のみを検証します。


`isValid()` メソッドは、次のいずれかのステータス値を返します。



| VALID_TOKEN | すべての検証に成功しました |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | トークン形式が無効です |
| INVALID_SIGNATURE | トークンの信頼性を検証できませんでした |
| TOKEN_EXPIRED | トークン TTL が無効です |
| INVALID_RESOURCE_ID | 指定されたリソースのトークンが無効です |
| ERROR_UNKNOWN | トークンはまだ検証されていません |

その他のメソッドでは、特定のトークンのリソース ID、発行時間および有効期間に対する特定のアクセスが提供されます。

* `getResourceID()` を使用して、トークンに関連付けられたリソース ID を取得し、setToken （） リクエストから返された ID と比較します。
* トークンが発行された時刻を取得するには、`getTimeIssued()` を使用します。
* `getTimeToLive()` を使用して TTL を取得します。
* `getUserSessionGUID()` を使用して、MVPD によって設定された匿名化 GUID を取得します。
* `getMvpdId()` を使用して、ユーザーを認証した MVPD の ID を取得します。
* `getProxyMvpdId()` を使用して、ユーザーを認証したプロキシ MVPD の ID を取得します。

## サンプルコード {#sample-code}

メディアトークン検証者アーカイブには、参照実装（`com.adobe.entitlement.test.EntitlementVerifierTest.java`）と、テストクラスを使用した API の呼び出し例が含まれています。 このサンプル（`com.adobe.entitlement.text.EntitlementVerifierTest.java`）では、トークン検証ライブラリをメディアサーバーに統合する方法を示しています。


```Java
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
