---
title: MVPD ログインページをiFrameからポップアップに移行する方法
description: MVPD ログインページをiFrameからポップアップに移行する方法
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: b51ac004765a8617347ac2ddadbfe60adff8ea3a
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 0%

---

# （従来）MVPD ログインページをiFrameからポップアップに移行する方法 {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## ポップアップとiFrameの比較 {#popup-vs-iframe}

一部のユーザーで、MVPD ログインページのiFrameの実装に関するサードパーティ Cookieの問題が発生しました。
<!--
These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)
-->

Adobe Pass認証チーム **では、FirefoxとSafariのiFrame バージョンではなく、ポップアップ/新しいウィンドウのログインページ**&#x200B;を実装することをお勧めします。  ただし、Internet Explorerのログインページを実装している場合は、ポップアップ実装で問題が発生する可能性があります。 IEの問題は、ユーザーがポップアップウィンドウでMVPDを使用して認証した後、Adobe Pass Authenticationが親ページリダイレクトを強制するという事実によって引き起こされます。これは、Internet Explorerでポップアップブロッカーと見なされます。 Adobe Pass Authentication Team **では、Internet Explorer**&#x200B;にiFrame ログインを実装することをお勧めします。

このテクニカルノートに記載されているサンプルコードでは、iFrameとポップアップの両方をハイブリッド実装して、Internet ExplorerでiFrameを開き、他のブラウザーでポップアップを開きます。

iFrame実装が既に存在することを考慮すると、テクニカルノートの最初の部分にはiFrame実装用のコードが表示され、2番目の部分にはポップアップ実装に対応する変更がデフォルトとして表示されます。


## iFrameのログインページ付きMVPD ピッカー {#mvpd-pickr-iframe}

以前のコード例では、「iFrameを閉じる」ボタンと共にiFrameを作成する&lt;div> タグが含まれているHTML ページを示しています。

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

関連する&#x200B;**JavaScript** コードを次に示します。

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## ポップアップウィンドウにログインページが表示されたMVPD ピッカー {#mvpd-pickr-popup}

**iFrame**&#x200B;はもう使用しないため、HTML コードにはiFrameやiFrameを閉じるボタンは含まれません。 以前iFrameを含んでいたdiv - **mvpddiv** – は保持され、次の場合に使用されます。

* ポップアップフォーカスが失われた場合に、MVPD ログインページが既に開いていることをユーザーに通知するには
* ポップアップに再び注目するためのリンクを提供します

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

MVPDのリストは、**ピッカー**&#x200B;というdivにselect **-mvpdList**&#x200B;として表示されます。

新しいAPI コールバックが使用されます – **setConfig （configXML）**。 コールバックは、setRequestor （requestorID）関数を呼び出した後にトリガーされます。 このコールバックは、以前に設定したrequestorIDと統合されたMVPDのリストを返します。 コールバックメソッドでは、受信XMLが解析され、キャッシュされたMVPDのリストが解析されます。 MVPD ピッカーも作成されますが、表示されません。

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

getAuthentication （）またはgetAuthorization （）関数が呼び出された後、displayProviderDialog （） コールバックがトリガーされます。 通常、このコールバック内では、MVPD リストが作成され、表示されます。 MVPD ピッカーは既に構築されているので、残された作業はユーザーに表示することだけです。

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

ユーザーがピッカーからMVPDを選択したら、ポップアップを作成する必要があります。 一部のブラウザーでは、約:blankまたは別のドメイン上のページで作成された場合、ポップアップがブロックされる可能性があります。そのため、AccessEnablerが読み込まれるホスト名でポップアップを開くことをお勧めします。

iFrameの実装では、btnCloseIframe ボタンとJavaScript関数closeIframeAction （）によって認証フローのリセットが行われていましたが、iFrameをデコレートすることはできなくなりました。 したがって、ポップアップが閉じられるときを監視することによって（ユーザーまたは認証フローを終了することによって）同じ動作が達成されます。 ユーザーがポップアップのフォーカスを失った場合にも役立つコードのスニペットが追加されました。

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

createIFrame （） コールバックに、**mvpddiv** divが表示されます。

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* サンプルコードには、使用するrequestorID - &#39;REF&#39;のハードコードされた変数が含まれており、これは実際のプログラマの依頼者IDに置き換える必要があります。
>* サンプルコードは、使用する依頼者IDに関連付けられたホワイトリストドメインからのみ適切に実行されます。
>* コード全体がダウンロード可能であるため、このテクニカルノートに記載されているコードは切り捨てられています。 完全なサンプルについては、**JS iFrame vs Popup Sample**&#x200B;を参照してください。
>* 外部JavaScript ライブラリは、[Google Hosted Services](https://developers.google.com/speed/libraries/)からリンクされました。
