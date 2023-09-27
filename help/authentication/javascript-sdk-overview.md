---
title: JavaScript SDK の概要
description: JavaScript SDK の概要
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# JavaScript SDK の概要 {#javascript-sdk-overview}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## はじめに

Adobeでは、AccessEnabler ライブラリの最新の JS v4.x に移行することを強くお勧めします。

Adobe Pass認証 JavaScript 統合により、使い慣れた JS Web アプリケーション開発環境で、プログラマー向けに TV-Everywhere ソリューションを提供します。 統合の主な構成要素は、「高レベル」のアプリケーション（ユーザーインタラクション、ビデオ表示）と、権限フローへのエントリを提供し、Adobe Pass Authentication Server との通信を処理するAdobeが提供する「低レベル」の AccessEnabler ライブラリです。

一般的なAdobe Pass認証の使用権限付与フローについては、 [プログラマーエンタイトルメントフロー](/help/authentication/entitlement-flow.md)を参照し、「 JavaScript 統合クックブック」では実装の手順を説明します。 次のセクションでは、JavaScript AccessEnabler 統合に固有の説明とサンプルを示します。

>[!IMPORTANT]
>
>このドキュメントでは、デスクトップ Web ソリューションの実装について説明します。 JavaScript ライブラリは、モバイルプラットフォーム ( 例えば、iOSの Safari、Android の Chrome) ではサポートされません。 モバイルプラットフォーム (iOS、Android、Windows) をターゲットにしたい場合は、アドビのネイティブ SDK を使用してください。

## MVPD 選択ダイアログの作成 {#creating-the-mvpd-selection-dialog}

ユーザーが MVPD にログインして認証を受けるには、ページまたはプレーヤーがユーザーが MVPD を識別する手段を提供する必要があります。 開発用に、MVPD 選択ダイアログのデフォルトバージョンが提供されます。 実稼動環境で使用する場合は、独自の MVPD セレクターを実装する必要があります。

顧客のプロバイダーが既にわかっている場合は、 [MVPD をプログラムで設定する](/help/authentication/home.md)（ユーザーの操作なし） この方法は同じですが、プロバイダセレクタダイアログを呼び出し、顧客に MVPD の選択を求める手順を回避します。

## サービスプロバイダーの表示 {#displaying-the-service-provider}

次のコードサンプルは、現在の顧客のサービスプロバイダーを検出して表示する方法を示しています。

**HTML**  — このページには、顧客が既にログインしている場合に、選択したプロバイダーを表示するセクションがページに追加されます。

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


**JavaScript** この JavaScript ファイルは、ユーザーが既にログインしている場合に、現在のプロバイダに対して Access Enabler を照会し、その結果を、そのために予約されているページ・セクションに表示します。 また、MVPD セレクターダイアログも実装します。

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

## ログアウト中 {#logout}

通話 `logout()` ログアウトプロセスを開始する場合。 このメソッドは引数を取りません。 現在のユーザーをログアウトし、そのユーザーのすべての認証および認証情報を消去し、ローカルシステムからすべての AuthN および AuthZ トークンを削除します。

プレーヤーがユーザーログアウトの処理を担当しない場合があります。



- **Adobe Pass Authentication と統合されていないサイトからログアウトが開始されたとき。** この場合、MVPD は、ブラウザーのリダイレクトを通じてAdobe Pass Authentication Single Logout サービスを呼び出すことができます。 （backchannel 呼び出しを使用した SLO の呼び出しは、現在サポートされていません。）

>[!NOTE]
>
>ユーザーが、トークンの期限が切れるまでマシンをアイドル状態のままにした場合でも、セッションに戻ってログアウトを正常に開始できます。 Adobe Pass認証では、すべてのトークンが削除され、セッションも削除するよう MVPD に通知します。

次の JavaScript コードは、現在認証されているユーザーのログアウト（非認証）の例です。

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
