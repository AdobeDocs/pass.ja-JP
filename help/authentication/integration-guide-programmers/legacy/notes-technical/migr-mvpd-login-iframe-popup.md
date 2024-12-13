---
title: MVPD ログインページを iFrame からポップアップに移行する方法
description: MVPD ログインページを iFrame からポップアップに移行する方法
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# （従来）MVPDのログインページを iFrame から Popup に移行する方法 {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## ポップアップと iFrame の比較 {#popup-vs-iframe}

一部のユーザーでは、MVPD ログインページの iFrame 実装でサードパーティ Cookie の問題が発生しました。
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Adobe Pass認証チームは **Firefox および Safari で iFrame バージョンを実装するのではなく** ポップアップ/新しいウィンドウログインページを実装することをお勧めします。  ただし、Internet Explorer のログインページを実装している場合、ポップアップの実装で問題が発生する可能性があります。 IE の問題は、ポップアップウィンドウでMVPDを使用して認証すると、Adobe Pass認証によって親ページのリダイレクトが強制されるという事実が原因で発生します。これは、Internet Explorer ではポップアップブロックとして表示されます。 Adobe Pass Authentication チームは **Internet Explorer に iFrame ログインを実装することをお勧めします**。

このテクニカルノートに記載されているサンプルコードでは、iFrame とポップアップの両方のハイブリッド実装を使用しています。つまり、Internet Explorer で iFrame を開き、他のブラウザーでポップアップを開きます。

iFrame 実装が既に存在することを考慮して、テクニカルノートの最初の部分では iFrame 実装のコードを示し、2 番目の部分ではデフォルトのポップアップ実装に対応する変更を示します。


## iFrame のログインページを使用したMVPD ピッカー {#mvpd-pickr-iframe}

以前のコード例では、iFrame を作成する &lt;div> タグと「iFrame を閉じる」ボタンを含むHTMLページが示されました。

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

関連付けられている **JavaScript** コードを次に示します。

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

**iFrame** はもう使用しないので、HTMLコードには iFrame や、iFrame を閉じるボタンは含まれません。 以前は iFrame を含んでいた div （**mvpddiv**）は保持され、次の目的で使用されます。

* ポップアップフォーカスが失われた場合に、MVPD ログインページが既に開いていることをユーザーに通知するには、次の手順に従います
* ポップアップにフォーカスを取り戻すためのリンクを提供します

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

MVPD のリストは、**picker** という名前の div に select **-mvpdList** として表示されます。

新しい API コールバックが使用されます – **setConfig （configXML）**。 コールバックは、setRequestor （requestorID）関数を呼び出した後にトリガーされます。 このコールバックは、事前に設定された requestorID と統合された MVPD のリストを返します。 コールバックメソッドでは、受信した XML が解析され、MVPD のリストがキャッシュされます。 MVPD ピッカーも作成されますが、表示されません。

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

getAuthentication （）または getAuthorization （）関数が呼び出されると、displayProviderDialog （） コールバックがトリガーされます。 通常、このコールバック内では、MVPDリストが作成され、表示されます。 MVPD ピッカーは既にビルドされているので、後でユーザーに表示するだけです。

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

ピッカーからMVPDを選択したら、ポップアップを作成する必要があります。 about:blank または別のドメインにあるページでポップアップを作成すると、一部のブラウザーでポップアップがブロックされる場合があります。そのため、AccessEnabler がロードされたホスト名を使用してポップアップを開くことをお勧めします。

iFrame の実装では、btnCloseIframe ボタンとJavaScript関数 closeIframeAction （）によって認証フローのリセットが行われていましたが、iFrame をデコレートすることはできなくなりました。 そのため、同じ動作は、（ユーザーによって、または認証フローが完了することによって）ポップアップが閉じられたときを監視することで達成されます。 ユーザーがポップアップのフォーカスを失った場合に役立つコードのスニペットが追加されました。

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

createIFrame （） コールバックで **mvpddiv** div が表示されます。

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
>* サンプルコードには、使用される requestorID - 「REF」のハードコード変数が含まれています。これは、実際のプログラマーのリクエスター ID に置き換える必要があります。
>* サンプルコードは、使用されるリクエスター ID に関連付けられた、許可リストに登録されたドメインからのみ適切に実行されます。
>* コード全体をダウンロードできるので、このテクニカルノートに示されているコードは切り捨てられています。 完全なサンプルについては、**JS iFrame とポップアップのサンプル** を参照してください。
>* 外部のJavaScript ライブラリは、[Googleがホストするサービス ](https://developers.google.com/speed/libraries/) からリンクされていました。
