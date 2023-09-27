---
title: MVPD ログインページを iFrame からポップアップに移行する方法
description: MVPD ログインページを iFrame からポップアップに移行する方法
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---

# MVPD ログインページを iFrame から Popup に移行する方法 {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## ポップアップと iFrame {#popup-vs-iframe}

一部のユーザーでは、MVPD ログインページの iFrame 実装に関するサードパーティ cookie の問題が発生しています。
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Adobe Pass Authentication チーム **ポップアップ/新しいウィンドウのログインページの実装を推奨** Firefox および Safari 上の iFrame バージョンではなく  ただし、Internet Explorer のログインページを実装する場合、ポップアップの実装で問題が発生する可能性があります。 IE の問題は、ユーザーがポップアップウィンドウで MVPD で認証された後、Adobe Pass Authentication によって親ページのリダイレクトが強制され、Internet Explorer によってポップアップブロッカーと見なされます。 Adobe Pass Authentication チーム **は、Internet Explorer に iFrame ログインを実装することを推奨します。**.

このテクニカルノートに示すサンプルコードは、iFrame とポップアップの両方のハイブリッド実装（Internet Explorer で iFrame を開く、他のブラウザーでポップアップを開く）を使用しています。

iFrame の実装が既に存在する場合、テクニカルノートの最初の部分は iFrame 実装用のコードを表示し、2 番目の部分はポップアップ実装に対応する変更をデフォルトとして表示します。


## iFrame 内のログインページを持つ MVPD ピッカー {#mvpd-pickr-iframe}

前のコードの例では、HTMLページに &lt;div> タグ内の iFrame を閉じるボタンと共に作成する場所を指定します。

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

以下が関連する **JavaScript** コード：

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


## ポップアップウィンドウにログインページが表示された MVPD ピッカー {#mvpd-pickr-popup}

これは、 **iFrame** これ以降、HTMLコードには、iFrame や iFrame を閉じるボタンが含まれなくなります。 以前 iFrame を含んだ div - **mvpddiv**  — が保持され、次の場合に使用されます。

* ポップアップフォーカスが失われた場合に MVPD ログインページが既に開いていることをユーザーに通知するため。
* ポップアップに再び焦点を当てるためのリンクを提供する

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

MVPD のリストは、名前が付いた div に表示されます。 **ピッカー** 厳選して **-mvpdList**.

新しい API コールバックが使用されます。 **setConfig(configXML)**. このコールバックは、 setRequestor(requestorID) 関数を呼び出した後にトリガーされます。 このコールバックは、以前に設定された requestorID と統合された MVPD のリストを返します。 コールバックメソッドでは、受信 XML が解析され、MVPD のリストがキャッシュされます。 MVPD ピッカーも作成されますが、表示されません。

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

getAuthentication() 関数または getAuthorization() 関数が呼び出されると、 displayProviderDialog() コールバックがトリガーされます。 通常、このコールバック内では、MVPD リストが構築され、表示されていました。 MVPD ピッカーは既に構築されているので、ユーザーに表示するだけで済みます。

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

ユーザーがピッカーから MVPD を選択した後、ポップアップを作成する必要があります。 about:blank で作成されたり、別のドメイン上のページで作成された場合、一部のブラウザではポップアップがブロックされる場合があるので、AccessEnabler の読み込み元のホスト名で開くことをお勧めします。

iFrame の実装では、 btnCloseIframe ボタンと JavaScript 関数 closeIframeAction() によって認証フローのリセットがおこなわれていましたが、iFrame の修飾はできなくなりました。 そのため、ポップアップが閉じられるのを監視する（ユーザーによるか、認証フローが終了する）ことで、同じ動作を実現できます。 ユーザーがポップアップのフォーカスを失った場合にも役立つ、コードのスニペットが追加されました。

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

createIFrame() コールバックで、 **mvpddiv** div が表示されます。

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
>* このサンプルコードには、使用する requestorID - &#39;REF&#39;のハードコードされた変数が含まれています。この変数は、実際のプログラマーの要求元 ID に置き換える必要があります。
>* サンプルコードは、使用されている要求者 ID に関連付けられたホワイトリストに登録されたドメインからのみ正しく実行されます。
>* コード全体をダウンロードできるので、このテクニカルノートに表示されるコードは切り捨てられています。 完全なサンプルについては、 **JS iFrame とポップアップのサンプル**.
>* 外部 JavaScript ライブラリは、次の場所からリンクされました： [Google Hosted Services](https://developers.google.com/speed/libraries/).
