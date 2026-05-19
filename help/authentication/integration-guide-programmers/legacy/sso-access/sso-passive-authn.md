---
title: パッシブ認証によるSSO
description: パッシブ認証によるSSO
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 0%

---

# （従来）パッシブ認証によるSSO

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要

このドキュメントの範囲は、パッシブ認証フローの実装と、これが標準的なシングルサインオン アプローチでどのように機能するかを説明することです。

## ユースケース

Adobe Pass認証により、アプリとサイト間のシングルサインオン（SSO）が有効になります。 利用者がMVPDの認証情報を使用してログインすると、Adobe Pass Authenticationは、MVPDの認証セッションを表す安全なトークンを生成し、デバイス IDを使用してそのトークンを利用者のデバイスにバインドします。 Adobe Pass Authenticationは、トークン / デバイス IDをサーバーまたはデバイスに保存します。

トークンがまだ有効である限り、ユーザーは認証済みとして直接表示されます。 これにより、ユーザーはトランザクションを安全に保ちながら、より頻繁に資格情報を入力できるようになりました。



ここで説明するビジネスのユースケースは、非常に具体的な要件です。つまり、訪問したサイトごとに少なくとも1回はユーザーを認証する必要があります。 これにより、MVPDは、ネットワークごとに異なる場合がある認証セッションに関連するビジネスルールを適用できます。 これは、ユーザーが1回だけログインする必要があり、Adobe Pass認証エコシステムの一部であるすべてのサイトで認証されるという、現在のTVEの約束と矛盾しています。



ビジネスルールを管理し、優れたユーザーエクスペリエンスを維持するために、MVPDでは、ユーザーが資格情報を手動で入力する必要はありません。 以前に設定したセッション cookieを利用して、パッシブフローを使用して自動再認証を試みることができます。ユーザーの観点から、彼は自動的にログインしているように見えます。



これを解決するために、ネットワークごとの認証とパッシブ認証のサポートという2つの機能を実装しました。 MVPDは、どのサイトでセッションが作成されたかに関係なく、IdPに認証セッションが存在する場合にユーザーを再認証するSAML パッシブ認証をサポートします。



## ネットワークごとの認証

この機能により、MVPDは、訪問したすべてのサイトに対して1回だけ認証リクエストを受け取ることができます。 この機能を使用すると、Adobe Pass認証トークンが要求者IDにバインドされ、要求したネットワークにのみ有効になります。 その結果、利用者がサイト「A」で認証し、その後サイト「B」にアクセスすると、認証が必要になります。



MVPD IdPでは、利用者は既に認証されているため、ログイン情報を提供する必要はありませんが、代わりにブラウザーは単にサイト「B」からMVPD IdPにリダイレクトされ、その後戻されます。 同じユーザーがサイト「A」に戻っても認証されます。



次のフローは、ネットワークごとの基本的な認証の機能を示しています。





## パッシブ認証

これを行う目的は、完全なブラウザーのリダイレクトとピッカーを表示することなく、再認証プロセスをバックグラウンドで実行することです。 その結果、サイト Aからサイト Bに移動するユーザーは自動的に認証されます。



UXの観点からは、このフローと、通常のMVPDで実行されるフローとの間には違いはありません。 サイト Aを訪問した結果として資格情報を入力すると、サイト Bで自動的に認証されます。



このフローを実行するには、MVPDがセッションを持たない場合に非表示のiframeがログインページで「停止」しないように、MVPDがパッシブ認証をサポートする必要があります。 これは、標準の「isPassive」属性を使用して行われます。



次の図は、改善されたフローと「舞台裏」のパッシブ認証を示しています。





SAML リクエストサンプル
パッシブ認証フローのSAML リクエストサンプルを次に示します。


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

MVPDには、特定のSSO スコープドメイン制限があります。 例えば、一部のMVPD （同じメディア企業のSSO）では一部のドメインのみが許可されますが、企業間では許可されません。
一部のMVPDでは、異なる認証ルールを適用する必要があります。 例えば、MVPDは、異なるネットワークごとに異なる認証TTLを持つ場合があります。 また、MVPDは、一部のネットワークではホームベースの認証を有効にしますが、他のネットワークでは有効にしない場合があります（ペアレンタルコントロールのユースケースは強く表されています）。


これらのビジネス要件は、主なユースケースとして、MVPDで正常にログインした後に再度ログインする必要がないことがあることを念頭に置いてください。

これは、パッシブ認証フラグを使用したネットワークごとの認証を使用することで実現できます。



## 既知の制限事項

iOS - iOS ローカルストレージの性質上、SSO フローは異なるベンダーによって開発されたアプリケーションのiOSでは機能しません。 IOS 8以降のSSOについて詳しくは、このテクニカルノートを参照してください。


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
