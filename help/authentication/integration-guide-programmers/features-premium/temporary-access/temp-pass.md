---
title: 一時パス
description: 一時パス
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# 一時パス {#temp-pass}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 機能の概要 {#tempass-featur-summary}

Temp Pass を使用すると、MVPD のアカウント資格情報を持たないユーザーに対して、保護されたコンテンツへの一時的なアクセスを提供できます。  一時パスには、次の機能が含まれます。

* 一時パスを設定して、次のような様々なシナリオに一時的にアクセスすることができます。
   * プログラマーは、自社サイトの 1 つを毎日、短い時間（例：10 分のプレビュー）で提供できます。
   * プログラマーは、オリンピックや NCAA マーチマッドネスなどの主要なスポーツイベントの単一の長いプレゼンテーション（例えば、4 時間）を提供することができます。
   * プログラマーは、前述の 2 つのシナリオを組み合わせて提供できます。例えば、1 日の最初のより長い閲覧期間と、その後に続く数日間、毎日繰り返される一連の短い期間です。
* プログラマーは、一時パスの期間（有効期間（TTL）を指定します。
* 一時パスはリクエスターごとに動作します。  例えば、NBC は、リクエスター「NBCOlympics」に対して 4 時間の一時パスを設定できます。
* プログラマーは、特定のリクエスターに付与されたすべてのトークンをリセットできます。  一時パスの実装に使用される「一時 MVPD」は、「要求者ごとの認証」が有効になっている必要があります。
* **特定のデバイスの個々のユーザーに一時パスアクセスが付与されます**。 ユーザーの Temp Pass アクセスの有効期限が切れると、そのユーザーの有効期限が切れた [ 認証トークン ](/help/authentication/kickstart/glossary.md#authz-token) がAdobe Pass認証サーバーからクリアされるまで、そのユーザーは同じデバイスで一時的なアクセスを取得できなくなります。


>[!NOTE]
>
>Temp Pass は Premium ワークフローパッケージの一部です。 この機能の使用に興味がある場合は、Adobe Passの営業担当にお問い合わせください。

## 機能の詳細 {#tempass-featur-details}

* **表示時間の計算方法** – 一時パスが有効なままである時間は、ユーザーがプログラマーのアプリケーションでコンテンツの表示に費やした時間とは相関しません。  Temp Pass を介して承認を求める最初のユーザーリクエスト時に、プログラマーが指定した TTL に最初の現在のリクエスト時間を追加することで、有効期限が計算されます。 この有効期限は、ユーザーのデバイス ID とプログラマーのリクエスター ID に関連付けられ、Adobe Pass Authentication データベースに保存されます。 ユーザーが同じデバイスから Temp Pass を使用してコンテンツにアクセスしようとするたびに、Adobe Pass Authentication はサーバーリクエスト時間を、ユーザーのデバイス ID およびプログラマーのリクエスター ID に関連付けられた有効期限と比較します。 サーバーリクエストの時間が有効期限よりも短い場合は、認証が付与され、それ以外の場合は、認証が拒否されます。
* **設定パラメーター** – 一時パスルールを作成するために、プログラマーが次の一時パスパラメーターを指定できます。
   * **トークン TTL** - ユーザーが MVPD にログインせずに視聴できる時間。 この時間は時計ベースで、ユーザーがコンテンツを視聴しているかどうかに関係なく期限切れになります。
  >[!NOTE]
  >要求者 ID に複数の一時パス ルールを関連付けることはできません。
* **認証/認証**：一時パス・フローでは、MVPD を「一時パス」として指定します。  Adobe Pass Authentication は、Temp Pass フロー内の実際の MVPD と通信しないので、「Temp Pass」 MVPD はあらゆるリソースを許可します。 プログラマーは、サイトの残りのリソースと同様に、一時パスを使用してアクセス可能なリソースを指定できます。 メディア検証者ライブラリは、通常どおり使用して、一時パスの短いメディアトークンを検証し、再生前にリソースのチェックを実施できます。
* **一時パスフローでのトラッキングデータ** – 一時パス使用権限フロー中のトラッキングデータに関する 2 つの点：
   * Adobe Pass Authentication から **sendTrackingData （）** コールバックに渡されるトラッキング ID は、デバイス ID のハッシュです。
   * 一時パスフローで使用される MVPD ID は「一時パス」なので、同じ MVPD ID が **sendTrackingData （）** に返されます。 ほとんどのプログラマーは、Temp Pass 指標を実際の MVPD 指標とは異なる方法で扱う必要があります。 これには、Analytics の実装で追加の作業が必要です。

次の図に、一時通過フローを示します。

![ 一時通過フロー ](../../../assets/temp-pass-flow.png)

*図：一時パスフロー*

## 一時パスの実装 {#implement-tempass}

Adobe Pass認証側では、TempPass を実装し、TempPass という名前の擬似 MVPD を実装します。  この疑似 MVPD は、プログラマの保護されたコンテンツへのアクセスを一時的に許可する実際の MVPD のように機能します。

プログラマー側では、MVPD が認証に使用する 2 つのシナリオに対して、一時パスが次のように実装されます。

* **プログラマーのページの iFrame**。 一時パスは MVPD の認証タイプに関係なく機能しますが、iFrame シナリオの場合、現在の認証フローをキャンセルして一時パスで認証するには、追加の手順が必要です。 これらの手順については、以下の [iFrame ログイン ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) を参照してください。
* **MVPD ログインページにリダイレクトし直します**。 MVPD で認証を開始する前に Temp Pass をトリガーする UI が提示される従来のケースでは、特別な手順は必要ありません。 一時パスは、通常の MVPD と同様に扱われます。

両方の実装シナリオに次の点が適用されます。

* 「Temp Pass」は、Temp Pass 認証をまだリクエストしていないユーザーに対してのみ、MVPD ピッカーに表示されます。 以降のリクエストに対する表示をブロックするには、cookie にフラグを設定します。 これは、ユーザーがブラウザーのキャッシュをクリアしない限り機能します。 ユーザーがブラウザーのキャッシュをクリアすると、ピッカーに「一時パス」が再び表示され、ユーザーは再びリクエストできるようになります。 「一時パス」の期限がまだ切れていない場合にのみ、アクセスが許可されます。
* ユーザーが Temp Pass を介してアクセスをリクエストした場合、Adobe Pass認証サーバーは、認証プロセス中、通常の Security Assertion Markup Language （SAML）リクエストを実行しません。 代わりに、認証エンドポイントは、トークンがデバイスに対して有効な間、呼び出されるたびに成功を返します。
* Temp Pass フローでは、認証トークンと認証トークンの有効期限が同じであるため、Temp Pass の有効期限が切れると、そのユーザーは認証されなくなります。 Temp Pass の有効期限が切れたことをユーザーに説明するために、プログラマーは `setRequestor()` を呼び出した直後に選択した MVPD を取得し、通常どおり `checkAuthentication()` を呼び出す必要があります。 `setAuthenticationStatus()` コールバックでは、認証ステータスが 0 かどうかをチェックできます。これにより、選択した MVPD が「TempPass」の場合、Temp Pass セッションの有効期限が切れたことを示すメッセージをユーザーに表示できます。
* ユーザーが有効期限前に Temp Pass トークンを削除すると、後続の使用権限チェックにより、残り時間と等しい TTL を持つトークンが生成されます。
* 有効期限が切れた後にユーザーが Temp Pass トークンを削除した場合、後続の使用権限チェックでは「user not authorized」が返されます。

この節で説明する実装の詳細をコーディングする方法の例については、以下の [ サンプルコード ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) のサンプルを参照してください。

## サンプルコード {#tempass-sample-code}

以下の節では、Adobe Pass認証 API を呼び出して Temp Pass フローを実装する方法を示します。

* [iFrame ログインのサンプル](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [自動ログインのサンプル](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### iFrame ログインのサンプル {#iframe-login-sample}

次の例は、MVPD が iFrame 統合をサポートする場合に Temp Pass を実装する方法を示しています：

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

**Temp Pass を初めてリクエストするには：**

1. ユーザーがプログラマーのページにアクセスし、ログインリンクをクリックします。
1. MVPD ピッカーが開き、ユーザーがリストから MVPD を選択します。
1. 認証 iFrame が表示されます。 この iFrame には「Temp Pass」リンクが含まれています。
1. ユーザーが「Temp Pass」をクリックすると、プログラマーは Cookie にフラグを追加して、その後のページへの訪問で「Temp Pass」リンクが表示されないようにします。
1. Temp Pass 認証リクエストは、Adobe Pass認証サーバーに到達し、認証トークンを生成します。 TTL は、プログラマーが一時パスに設定した期間と等しくなります。
1. Temp Pass 認証リクエストがAdobe Pass認証サーバーに到達します。
1. Adobe Pass Authentication Server は、リクエストからデバイスとリクエスタ ID を抽出し、有効期限と共にデータベースに保存します。 有効期限は、最初の一時パスリクエスト時間に TTL （プログラマーが指定）を加算して計算されます。
1. Adobe Pass認証サーバーは、認証トークンを生成します。
1. ユーザーが保護されたコンテンツにアクセスします。

**再び Temp Pass ユーザーがブラウザー Cookie を削除した後に Temp Pass を再びリクエストするには：**

1. ユーザーがプログラマーのページにアクセスし、ログインリンクをクリックします。
1. MVPD ピッカーが開き、ユーザーがリストから MVPD を選択します。
1. 認証 iFrame が表示されます。 この iFrame には「Temp Pass」リンクが含まれています（ユーザーが元の Cookie を削除したので、プログラマーはユーザーが以前に「Temp Pass」リンクをクリックしたかどうかを知りません）。
1. ユーザーが「Temp Pass」を再度クリックすると、プログラマーはフラグを再度 Cookie に追加して、その後のページへの訪問で「Temp Pass」リンクが表示されるのを防ぎます。
1. Temp Pass 認証リクエストがAdobe Pass認証サーバーに到達し、サーバーが認証トークンを生成します。 TTL は、Temp Pass の残り時間（現在の時間とデバイス ID に関連付けられている有効期限の差）になりました。
1. Temp Pass 認証リクエストがAdobe Pass認証サーバーに到達します。
1. Adobe Pass認証サーバーは、リクエストからデバイス ID とリクエスター ID を抽出し、それらを使用してAdobe Pass認証データベースから有効期限を取得します。 現在の時間は有効期限と比較されます。
1. ユーザーの Temp Pass の有効期限が切れていない場合、Adobe Pass認証サーバーは認証トークンを生成します。
1. ユーザーの一時パスの有効期限が切れていない場合、ユーザーは保護されたコンテンツにアクセスできます。

### 自動ログインのサンプル {#auto-login-sample}

以下のサンプルは、ユーザーがサイトにアクセスしたときに TempPass で自動的にログインするケースを示しています。 ユーザーはいつでも通常の MVPD でログインすることを選択でき、TempPass の有効期限が切れると警告されます。

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

特定のイベントでは、コンテンツへの段階的な無料アクセスが必要です。無料アクセスの初期間隔（例：4 時間）とそれに続く毎日の無料アクセス（例：翌日ごとに 10 分）が必要です。  プログラマーがこのシナリオを実装するには、Adobeの担当者と調整して、プログラマー用に 2 つの一時的な MVPD を設定する必要があります。

この例では（最初の 4 時間のフリーセッション、その後の 1 日 10 分のフリーセッション）、Adobeは、TempPass1 という MVPD を 4 時間の Time-To-Live （TTL）で、TempPass2 を 10 分の TTL で、それ以降の期間に設定します。  どちらも、プログラマーのリクエスター ID に関連付けられています。

### プログラマ実装 {#mult-tempass-prog-impl}

Adobeが 2 つの TempPass インスタンスを設定すると、2 つの追加の MVPD （TempPass1 と TempPass2）がプログラマの MVPD リストに表示されます。  複数の一時パスを実装するには、プログラマーが次の手順を実行する必要があります。

1. ユーザーのサイトへの初回訪問時に、TempPass1 で自動的にログインします。 上記の自動ログインサンプルを、このタスクの出発点として使用できます。
1. TempPass1 の有効期限が切れていることが検出されたら、その事実を（cookie/ローカルストレージに）保存し、標準の MVPD ピッカーを使用してユーザーに表示します。 **そのリストから TempPass1 と TempPass2 を除外してください**。
1. TempPass1 の有効期限が切れた場合は、そのユーザーを TempPass2 で自動ログインします。
1. TempPass2 の有効期限が切れたら、その日のファクトを（Cookie/ローカルストレージに）保存し、標準の MVPD ピッカーを使用してユーザーに表示します。 ここでも、そのリストから TempPass1 と TempPass2 を除外してください。
1. 新しい日の 00:00 時点で、[TempPass Web API のリセット ](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass) を使用して、TempPass2 のすべての一時パスをリセットします。

>[!NOTE]
>**プログラミングメモ：** Adobe Pass認証には、10 分後にフリーストリーミングを停止するメカニズムは組み込まれていません。  TempPass2 の有効期限が切れたら、アクセスを制限するのはプログラマー次第です。 これを実現するために、プログラマーは、X 分ごとに「checkAuthorization」呼び出しをサイト/アプリに実装できます。X は、プログラマーがアプリで意味があると判断する期間です。

## すべての一時パスをリセット {#reset-all-tempass}

一部のビジネス・ルールでは、Temp Pass の定期的なパージ、または特定のリクエスタ ID と MVPD ID に対して発行されたすべての Temp Pass のアドホック・リセットが必要です。 この機能では、次のようなユースケースをサポートしています。

* 1 日 10 分間の一時パス（一時パスは 1 日の初めにリセットする必要があります）
* Temp Pass は、ニュース速報ですべてのユーザーが利用できます。 （Temp Pass は、ニュース速報が開始するとすぐに、すべてのデバイスについてリセットする必要があります）。
* 複数の一時パスのシナリオでは、ある長さの初期表示期間と、その後に続く別の長さの日別期間を組み合せます。

すべての一時パスをリセットするために、Adobe Pass認証ではプログラマーに次の *公開* web API を提供しています。

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>上記の URL は、以前のリセット API より優先されます。 古いリセット API （v1）はサポートされなくなりました。

* **プロトコル：** HTTPS
* **ホスト：**
   * リリース - mgmt.auth.adobe.com
   * Prequal - mgmt-prequal.auth.adobe.com
* **パス：** /reset-tempass/v2/reset
* **クエリパラメーター：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Headers:** ApiKey - 1232293681726481
* **応答：**
   * 成功 – HTTP 204
   * 失敗：
      * 誤ったリクエストに対する HTTP 400
      * ApiKey が指定されていない場合は HTTP 401
      * ApiKey が無効な場合は HTTP 403

例：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## サポートされるクライアント {#supp-clients}

プラットフォームによる一時パスおよびリセットツールのサポート：

| Adobe Pass認証クライアント | 一時パス | リセットツール |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | はい | はい |
| ネイティブクライアント iOS | はい | はい |
| ネイティブクライアント tvOS | はい | はい |
| ネイティブクライアント Android | はい | はい |
| ネイティブクライアント fireTV | はい | はい |
| クライアントレス API | はい | はい |

## 制限事項と既知の問題 {#limitations}

ここでは、Temp Pass の現在の実装に適用される制限事項について説明します。

**JavaScript SDK**: バージョン **3.X 以降** の一時的パスのリセット機能をサポートしています。

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
