---
title: パッシブ認証を使用した SSO
description: パッシブ認証を使用した SSO
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 0%

---

# パッシブ認証を使用した SSO

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。


## はじめに

このドキュメントの範囲は、パッシブ認証フローの実装と、これがアドビの標準的なシングルサインオンアプローチとどのように連携するかを説明することです。

## 使用事例

Adobe Pass認証により、アプリ/サイト間でのシングルサインオン (SSO) が可能になります。 ユーザーが MVPD 資格情報を使用してログインすると、Adobe Pass Authentication は MVPD の認証セッションを表すセキュリティで保護されたトークンを生成し、デバイス ID を使用してそのトークンをユーザーのデバイスにバインドします。 Adobe Pass認証では、トークン/デバイス ID がサーバーまたはデバイスに保存されます。

トークンがまだ有効である限り、ユーザーは認証済みとして直接表示されます。 これにより、トランザクションのセキュリティを確保しながら、ユーザーは資格情報を頻繁に入力しなくなります。



ここで説明するビジネスユースケースは、非常に具体的な要件です。訪問したサイトごとに少なくとも 1 回、ユーザーを認証する必要があります。 これにより、MVPD はネットワークごとに異なる可能性のある authN セッションに関連するビジネスルールを適用できます。 これは、ユーザーが 1 回だけログインする必要があり、Adobe Pass Authentication エコシステムの一部であるすべてのサイトで認証されるという、現在の TVE の約束と競合します。



ビジネスルールを維持しながら、優れたユーザーエクスペリエンスを維持するために、MVPD は、資格情報を手動で入力する必要がありません。 以前に設定したセッション cookie のメリットを活用し、パッシブフローを使用して自動再認証を試みることができます。ユーザーの観点からは、自動的にログインしているように見えます。



これらを解決するために、ネットワークごとの認証とパッシブ認証のサポートという 2 つの異なる機能を実装しました。 MVPDs は、SAML パッシブ authN をサポートします。この SAML パッシブ authN では、IdP 上に authN セッションが存在する場合、そのセッションが作成されたサイトに関係なく、ユーザーを再認証するだけです。



## ネットワークごとの認証

この機能により、MVPD は訪問されたサイトごとに 1 回認証要求を受け取るようになります。 この機能を使用すると、Adobe Pass認証トークンは requestorID に制限されるので、要求したネットワークに対してのみ有効です。 その結果、ユーザーがサイト「A」で認証し、その後サイト「B」を訪問した場合、認証が必要になります。



MVPD IdP ではユーザーが既に認証されているので、ログイン情報を提供する必要はありませんが、代わりにブラウザーはサイト「B」から MVPD IdP にリダイレクトされ、その後戻ってくるだけです。 同じユーザーが戻っても、サイト「A」で同じユーザーが認証されます。



次のフローは、ネットワークごとの基本認証機能を示しています。





## パッシブ認証

この目的は、ブラウザーの完全なリダイレクトとピッカーの表示を必要とせずに、再認証プロセスをバックグラウンドで実行することです。 その結果、ユーザーがサイト A からサイト B に移動した際に、自動的に認証が行われます。



UX の観点からは、このフローと通常の MVPD で実行されるフローとの間に違いはありません。 ユーザーに表示されるのは、サイト A への訪問の結果として資格情報を入力した後に、サイト B で自動的に認証されるということです。



このフローを実行可能にするには、MVPD がセッションを持たなくなった場合に、隠し iframe がログインページで「スタック」しないように、MVPD がパッシブ認証をサポートする必要があります。 これは、標準の「isPassive」属性を使用しておこなわれます。



次の図は、フローの改善と「バックザーシーン」のパッシブ認証を示しています。





SAML リクエストのサンプルパッシブ authN フローの SAML リクエストのサンプルを次に示します。


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

MVPD には、特定の SSO スコーピングドメイン制限があります。 例えば、一部の MVPD で許可できるドメインは一部のみです（同じメディア会社では SSO ですが、会社をまたぐことはできません）。
一部の MVPD では、異なる認証規則を適用する必要がある場合があります。 例えば、MVPD は、異なるネットワークごとに異なる認証 TTL を持つ場合があります。 また、MVPD は、一部のネットワークに対してホームベースの認証を有効にし、他のネットワークに対しては有効にしない場合があります（親による制御の使用例は、ここで強く表示されます）。


これらのビジネス要件は、主に、MVPD を使用して正常にサインインした後にユーザーが再度サインインする必要がないことに注意する必要があります。

これは、パッシブ authN フラグを持つネットワークごとの認証を使用することで実現できます。



## 既知の制限事項

iOS - iOSローカルストレージの性質上、SSO フローは、異なるベンダーが開発したアプリケーションではiOSで機能しません。 iOS 8 以降の SSO の詳細については、このテクニカルノートを参照してください。


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
