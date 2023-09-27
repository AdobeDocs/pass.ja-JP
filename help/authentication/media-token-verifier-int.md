---
title: メディアトークン検証ツールの統合
description: メディアトークン検証ツールの統合
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 0%

---

# メディアトークン検証ツールの統合

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## メディアトークン検証ツールについて {#about-media-token-verifier}

認証が成功すると、Adobe Pass Authentication は長期間有効な認証 (AuthZ) トークンを作成します。  AuthZ トークンは、クライアントのプラットフォームに応じて、クライアント側に渡されるか、サーバー側に保存されます。  ( 詳しくは、 [トークンについて](/help/authentication/programmer-overview.md#understanding-tokens) トークンが異なるクライアントシステム上でどのように保存されるか、およびその他の詳細 )。


AuthZ トークンは、特定のリソースを表示することをサイトのユーザーに許可します。  通常の有効期間 (TTL:time-to-live) は 6～24 時間で、その後トークンの有効期限が切れます。 **実際の表示アクセスの場合、Adobe Pass認証では AuthZ トークンを使用して、取得してメディアサーバーに渡す、短時間有効なメディアトークンを生成します**. これらの短時間のみ有効なメディアトークンの TTL は非常に短い（通常、数分）。


AccessEnabler の統合では、 `setToken()` コールバック。 クライアントレス API 統合の場合は、 `<SP_FQDN>/api/v1/tokens/media` API 呼び出し。 トークンは、PKI(Public Key Infrastructure) に基づくトークン保護を使用して、Adobeが署名した、クリアテキストで送信される文字列です。 この PKI ベースの保護を使用すると、トークンは、認証局によってAdobeに発行された非対称キーを使用して署名されます。


トークンの検証がクライアント側にないので、悪意のあるユーザーは、偽のを挿入するツールを使用する可能性があります `setToken()` 呼び出し。 したがって、 **できません** という事実に頼る `setToken()` がトリガーされました。 短時間のみ有効なトークン自体が正当なものであることを検証する必要があります。 検証を実行するツールは、メディアトークン検証者ライブラリです。


>[!TIP]
>
>返されたトークン文字列の全長をメディアトークン検証ツールに渡して、検証を行う必要があります。

## メディアトークン検証ツールを使用した短時間のみ有効なトークンの検証 {#validate-short-livedttokens}

実際にビデオストリームを開始する前にトークンを検証するには、プログラマーが Media Token Verifier Library を使用する Web サービスにトークンを送信することをお勧めします。 短時間有効なメディアトークンの非常に短い TTL は、トークンを生成するサーバとトークンを検証するサーバとの間のクロック同期の問題を可能にするのに十分な長さに定義されますが、それ以上は長くはなりません。



The [Media Token Verifier Library](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue){target=_blank} は、Adobe Pass認証パートナーで使用できます。



メディアトークン検証ライブラリは、Java アーカイブに含まれています。 `mediatoken-verifier-VERSION.jar`. ライブラリは次を定義します。

* トークン検証 API (`ITokenVerifier` インターフェイス )、JavaDoc ドキュメントを使用
* トークンが実際にAdobeから来たことを検証するために使用されるAdobe公開鍵
* 参照実装 (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) を参照してください。


アーカイブには、すべての依存関係と証明書キーストアが含まれます。 含まれる証明書キーストアのデフォルトのパスワードは「123456」です。

* 検証ライブラリには、JDK バージョン 1.5 以降が必要です。
* 署名アルゴリズム「SHA256WithRSA」には、JCE プロバイダーを使用します。


**トークンの内容を分析するためには、検証者ライブラリを唯一の方法にする必要があります。 トークンの形式は保証されず、将来の変更の対象となるので、プログラマーはトークンを解析してデータ自体を抽出しないでください。** Verifier API のみが正しく機能することを保証されています。 文字列を直接解析すると、一時的に機能する可能性がありますが、将来、形式が変更される可能性がある場合に問題が発生します。 Verifier API は、次のような情報をトークンから取得します。

* トークンが有効である ( `isValid()` 方法 )?
* トークン ( `getResourceID()` メソッド )。これは、 `setToken()` 関数のコールバック。 一致しない場合は、不正な動作を示している可能性があります。
* トークンが発行された時刻 (`getTimeIssued()` メソッド )。
* TTL (`getTimeToLive()` メソッド )。
* MVPD (`getUserSessionGUID()` メソッド )。
* ユーザーを認証したディストリビューターの ID、およびその場合（ディストリビューターの認証を提供した proxy-MVPD）。

## Verifier API の使用 {#using-verifier-api}

The `ITokenVerifier` クラスは、特定のリソースのトークンの信頼性を検証するために使用するメソッドを定義します。 以下を使用します。 `ITokenVerifier` に応じて受け取ったトークンを分析する方法 `setToken()` リクエスト。


The `isValid()` メソッドは、トークンを検証する主な方法です。 引数は 1 つ、リソース ID です。 NULL のリソース ID を渡すと、トークンの信頼性と有効期間のみが検証されます。


The `isValid()` メソッドは、次のいずれかのステータス値を返します。



| VALID_TOKEN | すべての検証に成功しました |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | トークンの形式が無効です |
| INVALID_SIGNATURE | トークンの信頼性を検証できませんでした |
| TOKEN_EXPIRED | トークン TTL が無効です |
| INVALID_RESOURCE_ID | 指定されたリソースに対してトークンが無効です |
| ERROR_UNKNOWN | トークンはまだ検証されていません |

追加のメソッドでは、リソース ID、発行時間、特定のトークンの有効期間に対する特定のアクセスを提供します。

* 用途 `getResourceID()` を使用して、トークンに関連付けられているリソース ID を取得し、setToken() リクエストから返される ID と比較します。
* 用途 `getTimeIssued()` をクリックして、トークンが発行された時間を取得します。
* 用途 `getTimeToLive()` をクリックして TTL を取得します。
* 用途 `getUserSessionGUID()` :MVPD によって設定された匿名化された GUID を取得します。
* 用途 `getMvpdId()` をクリックして、ユーザーを認証した MVPD の ID を取得します。
* 用途 `getProxyMvpdId()` をクリックして、ユーザーを認証した Proxy MVPD の ID を取得します。

## サンプルコード {#sample-code}

メディアトークン検証アーカイブには、参照実装 (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) と、test クラスで API を呼び出す例です。 このサンプル (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) は、トークン検証ライブラリがメディアサーバーに統合されたことを示します。


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
