---
title: JavaScript SDKの概要
description: JavaScript SDKの概要
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%
---
# （レガシー） JavaScript SDKの概要 {#javascript-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要

Adobeでは、AccessEnabler ライブラリの最新のJS v4.xに移行することを強くお勧めします。

Adobe Pass Authentication JavaScriptの統合により、プログラマーは、使い慣れたJS web アプリケーション開発環境でTV-Everywhere ソリューションを利用できます。 統合の主なコンポーネントは、「上位レベル」アプリケーション（ユーザーインタラクション、ビデオのプレゼンテーション）と、Adobeが提供する「下位レベル」のAccessEnabler ライブラリです。このライブラリは、使用権限フローへのエントリを提供し、Adobe Pass認証サーバーとの通信を処理します。

次の節では、JavaScript AccessEnabler統合に固有の説明とサンプルを示します。

>[!IMPORTANT]
>
>このドキュメントでは、デスクトップ web ソリューションの実装について説明します。 JavaScript ライブラリは、モバイルプラットフォーム（iOSのSafari、AndroidのChromeなど）ではサポートされていません。 モバイルプラットフォーム（iOS、Android、Windows）をターゲットにする場合は、アドビのネイティブ SDKをご利用ください。

## MVPD選択ダイアログの作成 {#creating-the-mvpd-selection-dialog}

利用者がMVPDにログインして認証を受けるには、ページまたはプレーヤーが、利用者がMVPDを識別する方法を提供する必要があります。 MVPDの選択ダイアログのデフォルトバージョンが開発用に用意されています。 実稼動用には、独自のMVPD セレクターを実装する必要があります。

お客様のプロバイダーが誰であるか既にご存知の場合は、ユーザーの操作なしで[MVPDをプログラムで](/help/authentication/home.md)設定できます。 この方法は同じですが、プロバイダーセレクターダイアログを呼び出し、お客様にMVPDを選択するように依頼する手順は省略します。

## サービスプロバイダーの表示 {#displaying-the-service-provider}

次のコードサンプルは、現在の顧客のサービスプロバイダーを検出して表示する方法を示しています。

**HTML** – このページには、お客様が選択したプロバイダーが既にログインしている場合に表示されるセクションが追加されます。

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript**&#x200B;このJavaScript ファイルは、ユーザーが既にログインしている場合に、現在のプロバイダーのAccess Enablerをクエリし、その結果を、そのプロバイダー用に予約されたページセクションに表示します。 また、次のMVPD セレクターダイアログも実装されています。

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## ログアウト {#logout}

`logout()`を呼び出して、ログアウトプロセスを開始します。 このメソッドは引数を取りません。 現在のユーザーをログアウトし、そのユーザーのすべての認証情報と認証情報を消去し、ローカルシステムからすべてのAuthNおよびAuthZ トークンを削除します。

ユーザーのログアウトを処理する責任をプレイヤーが負わない場合があります。



- **Adobe Pass Authenticationと統合されていないサイトからログアウトが開始されたとき。** この場合、MVPDは、ブラウザーリダイレクトを介してAdobe Pass Authentication Single Logout サービスを呼び出すことができます。 （バックチャネルコールを介したSLOの呼び出しは現在サポートされていません）。

>[!NOTE]
>
>ユーザーがトークンの有効期限が切れるほどマシンをアイドル状態のままにした場合、セッションに戻り、ログアウトを正常に開始できます。 Adobe Pass認証では、すべてのトークンが削除され、MVPDに対してセッションの削除も通知されます。

次のJavaScript コードは、現在認証されているユーザーのログアウト（認証解除）を示しています。

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
