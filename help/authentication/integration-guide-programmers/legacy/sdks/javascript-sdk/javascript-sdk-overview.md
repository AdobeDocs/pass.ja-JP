---
title: JavaScript SDKの概要
description: JavaScript SDKの概要
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# （従来の）JavaScript SDKの概要 {#javascript-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

## 概要

Adobeでは、最新の JS v4.x の AccessEnabler ライブラリにマイグレーションすることを強くお勧めします。

Adobe Pass Authentication JavaScript統合は、使い慣れた JS web アプリケーション開発環境で、プログラマーに TV-Everywhere ソリューションを提供します。 統合の主なコンポーネントは、「高レベル」のアプリケーション（ユーザー・インタラクション、ビデオ・プレゼンテーション）、Adobeが提供する「低レベル」の AccessEnabler ライブラリです。これらは、使用権限フローへのエントリーを提供し、Adobe Pass認証サーバとの通信を処理します。

次のセクションでは、JavaScript AccessEnabler の統合に関する説明とサンプルを示します。

>[!IMPORTANT]
>
>このドキュメントでは、デスクトップ web ソリューションの実装について説明します。 JavaScript ライブラリは、モバイルプラットフォームではサポートされません（例えば、iOSの Safari、AndroidのChrome）。 モバイルプラットフォーム（iOS、Android、Windows）をターゲットにする場合は、ネイティブ SDK を使用してください。

## MVPD選択ダイアログの作成 {#creating-the-mvpd-selection-dialog}

ユーザーがMVPDにログインして認証されるようにするには、ページまたはプレーヤーが、ユーザーがMVPDを識別する方法を提供する必要があります。 MVPDの選択ダイアログのデフォルトバージョンが、開発用に提供されています。 実稼動で使用するには、独自のMVPD セレクターを実装する必要があります。

顧客のプロバイダーが既にわかっている場合は、ユーザーの操作なしで [&#x200B; プログラムによってMVPDを設定 &#x200B;](/help/authentication/home.md) できます。 方法は同じですが、プロバイダーセレクターダイアログを呼び出して顧客にMVPDを選択するように依頼する手順は省略されます。

## サービスプロバイダーの表示 {#displaying-the-service-provider}

次のコード例は、現在の顧客のサービスプロバイダーを検出して表示する方法を示しています。

**HTML** – このページには、顧客が既にログインしている場合に、顧客が選択したプロバイダーを表示するセクションが追加されます。

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


**JavaScript** このJavaScript ファイルは、ユーザーが既にログインしている場合、現在のプロバイダーのアクセス イネーブラを照会し、その結果をページ セクションに表示します。 また、MVPDのセレクターダイアログも実装されています。

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

`logout()` を呼び出してログアウトプロセスを開始します。 このメソッドは引数を取りません。 現在のユーザーをログアウトし、そのユーザーのすべての認証および認証情報をクリアして、ローカル・システムからすべての AuthN および AuthZ トークンを削除します。

プレーヤーがユーザーのログアウトの処理に責任を負わない場合があります。



- **Adobe Pass認証と統合されていないサイトからログアウトが開始された場合。** この場合、MVPDは、ブラウザーリダイレクトを通じてAdobe Pass Authentication Single Logout サービスを呼び出すことができます。 （バックチャネル呼び出しを使用した SLO の呼び出しは、現在サポートされていません）。

>[!NOTE]
>
>ユーザーがトークンの有効期限が切れるまでマシンをアイドル状態のままにしても、セッションに戻ってログアウトを正常に開始できます。 Adobe Pass認証を使用すると、すべてのトークンが削除され、セッションを削除するようにMVPDに通知されます。

次のJavaScript コードは、現在認証されているユーザーのログアウト（認証の解除）を示しています。

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
