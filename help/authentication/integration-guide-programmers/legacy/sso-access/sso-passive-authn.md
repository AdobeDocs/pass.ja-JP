---
title: パッシブ認証を介した SSO
description: パッシブ認証を介した SSO
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 0%

---

# （レガシー）パッシブ認証を使用した SSO

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [ 製品のお知らせ ](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要

このドキュメントの範囲は、パッシブ認証フローの実装と、標準のシングルサインオンアプローチとどのように連携するかについて説明することです。

## ユースケース

Adobe Pass認証は、アプリやサイト間でのシングルサインオン（SSO）を有効にします。 Adobe Pass Authentication は、ユーザーがMVPD資格情報を使用してログインすると、MVPD認証セッションを表すセキュアトークンを生成し、デバイス ID を使用してそのトークンをユーザーのデバイスにバインドします。 Adobe Pass Authentication は、トークン/デバイス ID をサーバーまたはデバイスに保存します。

トークンが有効である限り、ユーザーは認証済みとして直接表示されます。 これにより、ユーザーはトランザクションの安全性を維持しながら、資格情報を入力する頻度を減らすことができます。



ここで詳しく説明するビジネスのユースケースは、非常に具体的な要件です。つまり、訪問したすべてのサイトに対して少なくとも 1 回はユーザーを認証する必要があります。 これにより、MVPDは、authN セッションに関連付けられたビジネスルールをネットワークごとに適用できます。 これは、ユーザーが一度だけログインする必要があり、Adobe Pass認証エコシステムの一部であるすべてのサイトで認証されるという現在の TVE Promise と競合します。



ビジネスルールを維持しながらユーザーエクスペリエンスを良好に保つために、MVPDではユーザーが資格情報を手動で入力する必要はありません。 以前に設定したセッション Cookie を利用して、パッシブフローを使用した自動再認証を試みることができます。ユーザーの観点からは、自動的にログインしているように見えます。



これを解決するために、ネットワークごとの認証とパッシブ認証の 2 つの異なる機能を実装しました。 MVPD は、IdP に authN セッションが存在する場合は、そのセッションが作成されたサイトに関係なく、単にユーザーを再認証する SAML パッシブ authN をサポートします。



## ネットワークごとの認証

この機能により、訪問されたすべてのサイトに対して、MVPDが 1 回だけ認証リクエストを受け取るようになります。 つまり、Adobe Pass認証認証トークンは、要求者 ID に制限されるので、そのトークンを要求したネットワークに対してのみ有効になります。 その結果、ユーザーがサイト「A」で認証を行い、その後サイト「B」にアクセスした場合は、認証が必要になります。



MVPD IdP ではユーザーは既に認証されているので、ログイン情報を提供する必要はなく、代わりに、ブラウザーはサイト「B」からMVPD IdP にリダイレクトされ、その後戻ります。 戻ってきた場合、同じユーザーがサイト「A」で引き続き認証されます。



次のフローは、ネットワークごとの基本認証機能を示しています。





## パッシブ認証

この目的は、完全なブラウザーリダイレクトやピッカーを表示することなく、バックグラウンドで再認証プロセスを実行することです。 その結果、サイト A からサイト B に移動するユーザーは自動的に認証されます。



UX の観点では、このフローと通常のMVPDで実行されるフローに違いはありません。 ユーザーには、サイト A にアクセスした結果として認証情報を入力した後、サイト B で自動的に認証されることが表示されます。



このフローを実行可能にするには、MVPDでパッシブ認証をサポートして、MVPDにセッションがない場合にログインページで非表示の iframe が「停止」しないようにする必要があります。 これは、標準の「isPassive」属性を介して行われます。



次の図は、改善されたフローと「バックグラウンド」のパッシブ認証を示しています。





SAML リクエストのサンプル
パッシブ authN フローの SAML リクエストサンプルは次のとおりです。


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## ビジネスルール

MVPD には、特定の SSO スコーピングドメイン制限があります。 例えば、一部の MVPD （同じメディア会社に対する SSO で、企業間では許可されていない）で許可されるドメインは一部のみです。
一部の MVPD では、別の認証ルールの適用が必要になる場合があります。 例えば、MVPD はネットワークごとに異なる認証 TTL を持つ場合があります。 また、MVPD は、一部のネットワークではホームベースの認証を有効にしますが、他のネットワークでは有効にしません（ペアレンタルコントロールのユースケースは、ここでは強く表されています）。


これらのビジネス要件は、主なユースケースとして、MVPDで正常にログインした後にユーザーが再度ログインする必要がないことに注意してください。

これは、パッシブ authN フラグでネットワークごとの認証を使用することで実現できます。



## 既知の制限事項

iOS - iOS ローカルストレージの性質上、異なるベンダーが開発したアプリケーションの場合、SSO フローはiOSでは機能しません。 iOS 8 以降の SSO について詳しくは、このテクニカルノートを参照してください。


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
