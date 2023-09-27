---
title: 一時パス
description: 一時パス
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2229'
ht-degree: 0%

---

# 一時パス {#temp-pass}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 機能の概要 {#tempass-featur-summary}

Temp Pass を使用すると、MVPD のアカウント資格情報を持たないユーザーに対して、プログラマーは、保護されたコンテンツへの一時的なアクセスを提供できます。  Temp Pass には次の機能が含まれます。

* 一時パスは、次のような様々なシナリオに対応するための一時的なアクセスを提供するように設定できます。
   * プログラマは、自分のサイトの 1 つを日々短く（例えば、10 分のプレビュー）提供することができます。
   * プログラマーは、オリンピックや NCAA 3 月の狂気などの主要なスポーツイベントの 1 つの長いプレゼンテーション（4 時間など）を提供することができます。
   * プログラマは、前の 2 つのシナリオの組み合わせを提供できます。たとえば、最初の、長い期間を 1 日表示し、その後、数日間毎日繰り返す短い期間の連続を提供します。
* プログラマーは、Temp Pass の期間（Time-To-Live、または TTL）を指定します。
* Temp Pass はリクエスト元ごとに動作します。  たとえば、NBC は要求元「NBCOlympics」に対して 4 時間の Temp Pass を設定できます。
* プログラマは、特定のリクエスト元に付与されたすべてのトークンをリセットできます。  Temp Pass の実装に使用する「一時 MVPD」は、「要求元ごとの認証」が有効になっている状態で設定する必要があります。
* **Temp Pass アクセスは、特定のデバイス上の個々のユーザーに付与されます。**. あるユーザーの Temp Pass アクセスが期限切れになると、そのユーザーは、そのユーザーの期限切れになるまで、同じデバイス上の一時的なアクセス権を取得できなくなります [認証トークン](/help/authentication/glossary.md#authz-token) Adobe Pass Authentication Server からクリアされます。


>[!NOTE]
>
>Temp Pass は Premium ワークフローパッケージの一部です。 この機能の使用に関心がある場合は、Adobe Passの営業担当にお問い合わせください。

## 機能の詳細 {#tempass-featur-details}

* **表示時間の計算方法** - Temp Pass が有効なままの時間は、ユーザーがプログラマーのアプリケーションでコンテンツを表示している時間とは関係ありません。  Temp Pass を介した初期ユーザーの認証要求時に、初期現在の要求時間をプログラマが指定した TTL に追加することで、有効期限が計算されます。 この有効期限は、ユーザーのデバイス ID とプログラマーの要求元 ID に関連付けられ、Adobe Pass Authentication データベースに保存されます。 ユーザーが同じデバイスから Temp Pass を使用してコンテンツにアクセスしようとするたびに、Adobe Pass Authentication は、サーバーリクエスト時間と、ユーザーのデバイス ID およびプログラマーの要求元 ID に関連付けられた有効期限とを比較します。 サーバー要求時間が有効期限より短い場合は、承認が許可され、それ以外の場合は、承認が拒否されます。
* **設定パラメーター**  — プログラマーは、次の Temp Pass パラメータを指定して、Temp Pass ルールを作成できます。
   * **トークン TTL**  — ユーザーが MVPD にサインインせずに視聴できる時間。 この時間は時計ベースで、ユーザーがコンテンツを視聴しているかどうかに関わらず期限が切れます。
  >[!NOTE]
  >リクエスト元 ID には、複数の Temp Pass ルールを関連付けることはできません。
* **認証/承認**  — 一時パスフローで、MVPD を「一時パス」に指定します。  Adobe Pass認証は、Temp Pass フロー内の実際の MVPD と通信しないので、「Temp Pass」MVPD はリソースを許可します。 プログラマーは、サイト上の残りのリソースと同様に、Temp Pass を使用してアクセス可能なリソースを指定できます。 Media Verifier Library は、通常どおりに使用して、Temp Pass ショートメディアトークンを検証し、再生前にリソースチェックを実施できます。
* **一時パスフローのデータの追跡**  — 一時パスエンタイトルメントフロー中のデータの追跡に関する 2 つのポイント：
   * Adobe Pass認証からに渡されるトラッキング ID **sendTrackingData()** callback は、デバイス ID のハッシュです。
   * 一時パスフローで使用される MVPD ID は「Temp Pass」なので、同じ MVPD ID がに戻されます。 **sendTrackingData()**. ほとんどのプログラマーは、Temp Pass 指標を実際の MVPD 指標とは異なる方法で扱いたいと考えるでしょう。 これには、Analytics 実装での追加作業が必要です。

次の図に、一時パスのフローを示します。

![一時パスフロー](assets/temp-pass-flow.png)

*図：一時パスフロー*

## 一時パスの実装 {#implement-tempass}

Adobe Pass認証側では、Temp Pass は、参加するプログラマーのサーバー設定に「TempPass」という名前の擬似 MVPD を追加して実装されます。  この擬似 MVPD は、プログラマーの保護されたコンテンツへのアクセスを一時的に許可する実際の MVPD のように動作します。

プログラマー側では、MVPD が認証に使用する 2 つのシナリオに対して、Temp Pass は次のように実装されます。

* **プログラマーのページの iFrame**. Temp Pass は、MVPD の認証タイプに関係なく機能しますが、iFrame シナリオでは、現在の認証フローをキャンセルし、Temp Pass で認証するには、追加の手順が必要です。 これらの手順は、 [iFrame ログイン](/help/authentication/temp-pass.md) 下
* **MVPD ログインページにリダイレクト**. MVPD で認証を開始する前に、Temp Pass をトリガーする UI が提示される従来のケースでは、特別な手順は実行されません。 Temp Pass は通常の MVPD と同様に扱う必要があります。

次の点が両方の実装シナリオに当てはまります。

* 「Temp Pass」は、Temp Pass 認証をまだリクエストしていないユーザーに対してのみ、MVPD ピッカーに表示される必要があります。 後続のリクエストでの表示をブロックするには、Cookie にフラグを保持します。 ユーザーがブラウザーのキャッシュをクリアしない限り、この機能は動作します。 ユーザーがブラウザーのキャッシュをクリアした場合は、ピッカーに「Temp Pass」が再度表示され、ユーザーは再度リクエストできます。 「Temp Pass」時間がまだ期限切れになっていない場合にのみ、アクセス権が付与されます。
* ユーザーが Temp Pass 経由でアクセスを要求すると、Adobe Pass Authentication Server は、認証プロセス中に通常の Security Assertion Markup Language(SAML) 要求を実行しません。 代わりに、認証エンドポイントは、デバイスに対してトークンが有効な状態で、呼び出されるたびに成功を返します。
* 一時パスの有効期限が切れると、そのユーザーは認証されなくなります。これは、一時パスフローで認証トークンと認証トークンの有効期限が同じになるからです。 Temp Pass の期限が切れたことをユーザーに説明するために、プログラマーは、を呼び出した直後に、選択した MVPD を取得する必要があります。 `setRequestor()`を呼び出してから、を呼び出します。 `checkAuthentication()` いつも通り Adobe Analytics の `setAuthenticationStatus()` コールバック認証状態が 0 かどうかを確認し、選択した MVPD が「TempPass」の場合に、一時パスセッションが期限切れになったことをユーザーに伝えるメッセージを表示できます。
* 有効期限が切れる前にユーザーが一時パストークンを削除した場合、以降の権限付与チェックでは、残り時間と同じ TTL を持つトークンが生成されます。
* 有効期限が切れた後に Temp Pass トークンを削除した場合、後続のエンタイトルメントチェックでは「ユーザーが許可されていません」と返されます。

詳しくは、 [サンプルコード](/help/authentication/temp-pass.md#tempass-sample-code) この節で説明する実装の詳細をコード化する方法の例を以下に示します。

## サンプルコード {#tempass-sample-code}

以下の節では、Adobe Pass Authentication API を呼び出して Temp Pass フローを実装する方法を示します。

* [iFrame ログインの例](/help/authentication/temp-pass.md#iframe-login-sample)
* [自動ログインの例](/help/authentication/temp-pass.md#auto-login-sample)

### iFrame ログインの例 {#iframe-login-sample}

この例では、MVPDs が iFrame 統合をサポートする場合に Temp Pass を実装する方法を示します。

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### iFrame ログインの使用例 {#iframe-login-use-cases}

**初めて一時パスを要求するには、次の手順に従います。**

1. ユーザーがプログラマーのページにアクセスし、サインインリンクをクリックします。
1. MVPD ピッカーが開き、ユーザーがリストから MVPD を選択します。
1. 認証 iFrame が表示されます。 この iFrame には「Temp Pass」リンクが含まれています。
1. ユーザーが「Temp Pass」をクリックすると、プログラマーは cookie にフラグを追加し、その後のページ訪問でユーザーが「Temp Pass」リンクを表示できないようにします。
1. Temp Pass 認証リクエストがAdobe Pass認証サーバーに到達し、認証トークンを生成します。 TTL は、プログラマーが一時パスに設定した期間と等しくなります。
1. 一時パス認証リクエストがAdobe Pass認証サーバーに到達します。
1. Adobe Pass認証サーバーは、デバイス ID とリクエスト元の ID を抽出し、有効期限と共にデータベースに保存します。 有効期限は、初期の一時パス要求時間と TTL （プログラマーが指定）として計算されます。
1. Adobe Pass認証サーバーが認証トークンを生成します。
1. ユーザは、保護されたコンテンツにアクセスする。

**一時パスを返すユーザーがブラウザーの Cookie を削除した後に、一時パスを再度リクエストするには、次の手順を実行します。**

1. ユーザは、プログラマのページにアクセスし、サインインリンクをクリックします。
1. MVPD ピッカーが開き、ユーザーがリストから MVPD を選択します。
1. 認証 iFrame が表示されます。 この iFrame には「Temp Pass」リンクが含まれています（ユーザーが元の cookie を削除したので、ユーザーが以前に「Temp Pass」リンクをクリックしたかどうかはプログラマーは知りません）。
1. ユーザーが再び「Temp Pass」をクリックするので、プログラマーは cookie にフラグを追加し、ユーザーがその後のページ訪問で「Temp Pass」リンクを見るのを防ぎます。
1. 一時パス認証リクエストがAdobe Pass認証サーバーに到達し、認証トークンが生成されます。 TTL は、Temp Pass の残り時間になりました（現在の時間と、デバイス ID に関連付けられた有効期限との差）。
1. 一時パス認証リクエストがAdobe Pass認証サーバーに到達します。
1. Adobe Pass認証サーバーは、リクエストからデバイスとリクエスト元の ID を抽出し、それらを使用してAdobe Pass認証データベースから有効期限を取得します。 現在の時刻と有効期限が比較されます。
1. ユーザーの Temp Pass が期限切れになっていない場合、Adobe Pass認証サーバーは認証トークンを生成します。
1. ユーザーの Temp Pass が期限切れでない場合、ユーザーは保護されたコンテンツにアクセスできます。

### 自動ログインの例 {#auto-login-sample}

以下の例は、ユーザーがサイトを訪問する際に TempPass で自動的にログインする場合を示しています。 ユーザーは、常に通常の MVPD を使用してログインすることを選択でき、TempPass が期限切れになった場合に警告を受けます。

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## 複数の一時パスを使用 {#use-mult-tempass}

特定のイベントでは、コンテンツへの段階的な無料アクセスが必要です。例えば、初期の無料アクセス間隔（4 時間など）と、その後の日別の無料アクセス（翌日 10 分など）が必要です。  プログラマがこのシナリオを実装するには、プログラマに対して 2 つの一時的な MVPD を設定するために、Adobeの連絡先と一緒にこのシナリオを配置する必要があります。

この例のシナリオ（最初の 4 時間の空きセッション、その後の 10 分の空きセッション）では、Adobeは、TempPass1 という MVPD を 4 時間の Time-To-Live(TTL)、TTL が 10 分の TempPass2 を後続の期間に設定します。  どちらも、プログラマーのリクエスト元 ID に関連付けられます。

### プログラマーの実装 {#mult-tempass-prog-impl}

Adobeが 2 つの TempPass インスタンスを設定すると、2 つの追加の MVPD（TempPass1 と TempPass2）がプログラマの MVPD リストに表示されます。  プログラマーは、複数の一時パスを実装するために、次の手順を実行する必要があります。

1. ユーザーの初回サイト訪問時に、TempPass1 を使用して自動的にログインします。 上記の自動ログインの例を、このタスクの出発点として使用できます。
1. TempPass1 の有効期限が切れたことを検出したら、（cookie/ローカルストレージに）ファクトを保存し、標準の MVPD ピッカーを使用してユーザーに提示します。 **そのリストから TempPass1 と TempPass2 を必ず除外してください。**.
1. その後の各日に、TempPass1 の有効期限が切れた場合は、TempPass2 を使用してユーザーを自動ログインします。
1. TempPass2 の有効期限が切れたら、その日のファクトを（cookie/ローカルストレージに）保存し、標準の MVPD ピッカーをユーザーに提示します。 繰り返しになりますが、TempPass1 と TempPass2 をそのリストから除外してください。
1. 新しい日に 1 日 0 時 00 分に、TempPass2 のすべての一時パスを [TempPass Web API をリセット](/help/authentication/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**プログラミングに関する注意：** Adobe Pass認証には、10 分が経過した後に無料ストリーミングを停止するメカニズムが組み込まれていません。  TempPass2 の有効期限が切れると、アクセスを制限するのはプログラマー次第です。 これを達成するために、プログラマは自分のサイト/アプリに、X 分ごとに&quot;checkAuthorization&quot;呼び出しを実装できます。X は、プログラマがアプリに対して意味を持つと判断した期間です。

## すべての一時パスをリセット {#reset-all-tempass}

特定のビジネスルールでは、Temp Pass を定期的にパージするか、特定の Requestor ID および MVPD ID に対して発行されるすべての Temp Pass をアドホックにリセットする必要があります。 この機能は、次のような使用例をサポートします。

* 1 日 10 分の一時パス（一時パスは、当日の初めにリセットする必要があります）
* ニュースの速報中にすべてのユーザーが利用できる一時パス。 （速報が始まったら、すべてのデバイスで Temp Pass をリセットする必要があります）。
* 複数の一時パスシナリオ。ある長さの初期表示期間と、その後に続く別の長さの 1 日の期間を組み合わせて指定します。

すべての一時パスをリセットするために、Adobe Pass Authentication はプログラマーに *公開* web API:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>上記の URL は、以前のリセット API に代わるものです。 古いリセット API(v1) はサポートされなくなりました。

* **プロトコル：** HTTPS
* **ホスト：**
   * リリース — mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **パス：** /reset-tempass/v2/reset
* **クエリパラメーター：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **ヘッダー：** ApiKey - 1232293681726481
* **応答：**
   * 成功 — HTTP 204
   * 失敗：
      * HTTP 400 が正しくないリクエストを示しています
      * HTTP 401（ApiKey が指定されていない場合）
      * API キーが無効な場合の HTTP 403

例：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## サポートされるクライアント {#supp-clients}

プラットフォームによる一時パスとリセットツールのサポート：

| Adobe Pass Authentication Clients | 一時パス | ツールをリセット |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | はい | はい |
| ネイティブクライアントiOS | はい | はい |
| ネイティブのクライアント tvOS | はい | はい |
| ネイティブクライアント Android | はい | はい |
| ネイティブクライアントの FireTV | はい | はい |
| クライアントレス API | はい | はい |

## 制限事項と既知の問題 {#limitations}

この項では、現在の一時パスの実装に適用される制限について説明します。

**JavaScript SDK**：バージョンからの一時パスのリセット機能をサポート **3.X 以降**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
