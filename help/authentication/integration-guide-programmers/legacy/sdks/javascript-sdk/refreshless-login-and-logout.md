---
title: 更新なしのログインとログアウト
description: 更新なしのログインとログアウト
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '1817'
ht-degree: 0%
---
# （従来）更新なしのログインとログアウト {#tefresh-less-login-and-logout}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

## 概要 {#overview}

Web アプリケーションの場合は、ユーザーの認証とログアウトに関して、いくつかのシナリオを考慮する必要があります。  MVPDでは、利用者がMVPDのweb ページにログインして認証を行う必要がありますが、その際には次の要素が考慮されます。

- 一部のMVPDでは、サイトからログインページへの完全なリダイレクトが必要です
- 一部のMVPDでは、MVPD ログインページを表示するために、サイトでiFrameを開く必要があります
- 一部のブラウザーではiFrame シナリオが適切に処理されないため、これらのブラウザーでは、iFrameの代わりにポップアップウィンドウを使用することをお勧めします

Adobe Pass Authentication 2.7以前は、ユーザーを認証するためのこれらすべてのシナリオには、プログラマーのページ全体の更新が含まれていました。2.7以降のリリースでは、Adobe Pass認証チームはこれらのフローを改善し、ユーザーがログインおよびログアウト中にアプリでページの更新を経験する必要がないようにしました。


## 詳細な説明 {#detailed_description}

最初に元の認証フローとログアウトフローの概要を説明し、次に認証フローとログアウトフローを改善します。 最初の4つのセクションでは通常のMVPD （TempPass以外）に対応していますが、最後のセクションではTempPassに適用する必要がある特別な実装について説明しています。

- [元の認証フロー](#orig_authn)
- [元のログアウトフロー](#orig_logout)
- [認証フローの改善](#improved_authn)
- [ログアウトフローの改善](#improved_logout)
- [TempPass フロー](#improved_temppass)

</br>

## 元の認証/ログアウトフロー {#orig_authn}

**認証**

Adobe Pass Authentication web クライアントには、MVPDの要件に応じて、次の2つの認証方法があります。

1. **フルページリダイレクト -** プログラマーのweb サイトのMVPD ピッカーからプロバイダー（フルページリダイレクトで設定）を選択すると、AccessEnablerで`setSelectedProvider(<mvpd>)`が呼び出され、MVPDのログインページにリダイレクトされます。 ユーザーが有効な資格情報を提供すると、プログラマーのweb サイトにリダイレクトされます。 AccessEnablerが初期化され、認証トークンが`setRequestor`の間にAdobe Pass Authenticationから取得されます。
1. **iFrame / ポップアップウィンドウ -** ユーザーがプロバイダー（iFrameで設定）を選択すると、`setSelectedProvider(<mvpd>)`がAccessEnablerで呼び出されます。 この操作を行うと、`createIFrame(width, height)` コールバックがトリガーされ、名前`"mvpdframe"`と指定されたディメンションでiFrame （またはブラウザー/環境設定に応じてポップアップ）を作成するようプログラマーに通知されます。 iFrame/ポップアップを作成した後、AccessEnablerはMVPDのログインページをiFrame/ポップアップに読み込みます。 ユーザーが有効な資格情報を指定すると、iFrame/ポップアップがAdobe Pass Authenticationにリダイレクトされ、iFrame/ポップアップを閉じて親ページ（Programmer web サイト）を再読み込みするJS スニペットが返されます。 フロー1と同様に、認証トークンは`setRequestor`中に取得されます。

`displayProviderDialog` コールバック （`getAuthentication`/`getAuthorization`によってトリガー）は、MVPDとその適切な設定のリストを返します。 MVPDの`iFrameRequired` プロパティを使用すると、フロー1またはフロー2をアクティブ化する必要があるかどうかをプログラマが判断できます。 プログラマは、フロー2に対してのみ追加のアクション（iFrame/ポップアップの作成）を実行する必要があることに注意してください。

**認証をキャンセル**

また、ユーザがログインページを閉じることで認証フローを明示的にキャンセルする場合もある。 以下は、プログラマーに対するシナリオと提案された解決策です。

1. **フルページリダイレクト -** ログインページを閉じると、ユーザーは再度プログラマーのweb サイトに移動し、最初からフロー全体を開始する必要があります。 このシナリオでは、プログラマ側で明示的なアクションは必要ありません。
1. **iFrame -** プログラマーは、閉じるボタンがアタッチされた`div` （または類似のUI コンポーネント）内でiFrameをホストすることをお勧めします。 ユーザーが「閉じる」ボタンを押すと、プログラマーは関連するUIと共にiFrameを破棄し、`setSelectedProvider(null)`を実行します。 この呼び出しにより、AccessEnablerは内部状態をクリアでき、ユーザーは後続の認証フローを開始できます。 `setAuthenticationStatus`と`sendTrackingData(AUTHENTICATION_DETECTION...)`がトリガーされ、失敗した認証フローが通知されます（両方とも`getAuthentication`と`getAuthorization`）。
1. **ポップアップ -**&#x200B;一部のブラウザーでは、ウィンドウの閉じるイベントを正確に検出できないため、ここでは別のアプローチを採用する必要があります（上記のiFrame フローとは対照的です）。 Adobeでは、ログインポップアップの存在を定期的に確認するタイマーをプログラマが初期化することをお勧めします。 ウィンドウが存在しない場合、プログラマーは、ユーザーがログインフローを手動でキャンセルしたことを確認でき、プログラマーは`setSelectedProvider(null)`の呼び出しを続行できます。 トリガーされるコールバックは、上記のフロー2と同じです。

</br>

## 元のログアウトフロー {#orig_logout}

AccessEnablerのログアウト APIは、ライブラリのローカルステートをクリアし、現在のタブ/ウィンドウにMVPDのログアウト URLを読み込みます。 ブラウザーはMVPDのログアウトエンドポイントに移動し、プロセスが完了すると、ユーザーはプログラマーのweb サイトにリダイレクトされます。 ユーザーに代わって必要なアクションは、「ログアウト」ボタン/リンクを押してフローを開始することだけです。MVPDのログアウトエンドポイントでユーザーの操作は必要ありません。

**ページの更新による元の認証/ログアウトフロー**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## （リフレッシュレス）認証の向上 {#improved_authn}

>[!NOTE]
>
>改善された更新なしのログインフローとログアウトフローでは、ブラウザーがweb メッセージを含む最新のHTML 5 テクノロジーをサポートしている必要があります。

前述の認証（ログイン）フローとログアウトフローの両方は、各フローが完了した後にメインページを再読み込みすることで、同様のユーザーエクスペリエンスを提供します。  現在の機能は、更新なしの（バックグラウンド）ログインとログアウトを提供することで、ユーザーエクスペリエンスを向上させることを目的としています。 プログラマーは、2つのブール型フラグ （`backgroundLogin`と`backgroundLogout`）を`setRequestor` APIの`configInfo` パラメーターに渡すことにより、バックグラウンドでのログインとログアウトを有効または無効にできます。 デフォルトでは、バックグラウンドログイン/ログアウトは無効になっています（これは以前の実装との互換性を提供します）。

**例：**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**認証**

次の点は、元の認証フローと改善されたフローの間の移行を示しています。

1. ページ全体のリダイレクトは、MVPD ログインが実行される新しいブラウザータブに置き換えられます。 ユーザーがMVPD（`iFrameRequired = false`を含む）を選択すると、プログラマーは、`mvpdwindow`という名前の新しいタブ（`window.open`経由）を作成する必要があります。 その後、プログラマは`setSelectedProvider(<mvpd>)`を実行し、AccessEnablerが新しいタブにMVPD ログイン URLを読み込めるようにします。 ユーザーが有効な資格情報を入力すると、Adobe Pass Authenticationはタブを閉じ、認証フローが終了したことをAccessEnablerに通知するwindow.postMessageをプログラマーのweb サイトに送信します。 次のコールバックがトリガーされます。

   - `getAuthentication`によってフローが開始された場合：`setAuthenticationStatus`と`sendTrackingData(AUTHENTICATION_DETECTION...)`は、認証が成功/失敗したことを示すためにトリガーされます。

   - フローが`getAuthorization`によって開始された場合：`setToken/tokenRequestFailed`と`sendTrackingData(AUTHORIZATION_DETECTION...)`は、認証の成功または失敗を示すためにトリガーされます。

1. iFrame / ポップアップウィンドウのフローはほとんど変更されません。違いは、ユーザーが有効な資格情報を提供した後、親ページが再読み込みされないことです。 ログイン後にiFrame/ポップアップが自動的に閉じ、親ページに`window.postMessage`が送信され、フローが完了したことをAccessEnablerに通知します。 同じコールバックが前のフローと同様にトリガーされ、**に次の新しいコールバックが追加されます。`destroyIFrame`**。 `destroyIFrame` コールバックを使用すると、UI デコレーションなどのiFrame関連/補助コンポーネントをプログラマーが削除できます。 ログイン完了後にAdobe Pass Authenticationがプログラマーのページを再読み込みし、そのページ上のすべてのUI コンポーネントを破棄するため、古い認証フローでは、このコールバックの存在は必要ありませんでした。

</br>

>[!IMPORTANT]
> 
>AccessEnabler インスタンスを含むページの直接の子として、MVPD ログイン iFrameまたはポップアップウィンドウを読み込む必要があります。 MVPD ログイン iFrameまたはポップアップウィンドウが、AccessEnabler インスタンスを含むページの下に2つ以上のレベルでネストされている場合、フローがハングする可能性があります。 例えば、メインページとMVPD iFrame （Page =\> iFrame =\> MVPD iFrame）の間にiFrameがある場合、ログインフローが失敗する可能性があります。

</br>

**認証をキャンセル**

認証をキャンセルするためのフローは次のとおりです。

1. **ブラウザータブ -** タブは基本的に新しいウィンドウなので、閉じるイベントのキャプチャは、シナリオ 3で説明した古い認証フローと同じ制限があります。 さらに、ユーザーが手動で閉じたタブと、ログインフローの最後に自動的に閉じたタブを区別する方法がないため、タイマーのアプローチはここでは不可能です。 ここでの解決策は、ユーザーがフローをキャンセルしたときにAccessEnablerが「サイレント」（コールバックがトリガーされない）のままになることです。 また、プログラマは特定のアクションを実行する必要はありません。 ユーザーは、「複数の認証要求エラー」エラーを受け取ることなく、別の認証フローを開始できます（このエラーは、バックグラウンドログインのAccessEnablerで無効になっています）。

1. **iFrame -** プログラマーは、シナリオ 2で説明したアプローチを古い認証フローから実行できます（iFrameからラッパーUIを作成し、`setSelectedProvider(null)`をトリガーする関連する「閉じる」ボタンをクリックします）。 このアプローチはもはや強力な要件ではありませんが（上記のシナリオ 1で説明したように、バックグラウンドログインには複数の認証フローが許可されています）、Adobeでは引き続きお勧めします。

1. **ポップアップ -**&#x200B;これは、上記の「ブラウザー」タブのフローと同じです。

</br>

## ログアウトフローの改善 {#improved_logout}

新しいログアウトフローは非表示のiFrameで実行されるため、ページ全体のリダイレクトが不要になります。  これは、ユーザーがMVPDのログアウトページで特定のアクションを実行する必要がないためです。

ログアウトフローが完了すると、iFrameがカスタム Adobe Pass認証エンドポイントにリダイレクトされます。 これは、親に`window.postMessage`を実行するJS スニペットを提供し、ログアウトが完了したことをAccessEnablerに通知します。 次のコールバックがトリガーされます：`setAuthenticationStatus()`と`sendTrackingData(AUTHENTICATION_DETECTION ...)`。ユーザーが認証されなくなったことを示します。

次の図は、ユーザーがアプリケーションのメインページを更新せずにMVPDにログインできるようにする、更新なしのフローを示しています。

**改良された（更新なし）認証/ログアウトフロー**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass フロー {#improved_temppas}

リフレッシュレスログインは、TempPass タイプのMVPDに対して異なるアプローチを採用します。

TempPass フローでは、ウィンドウを自動的に作成し、ユーザーが明示的に操作することなく閉じる必要があるため、一部のブラウザー（ポップアップブロッカー）で問題が発生する場合があります。 したがって、AccessEnablerは、プログラマが作成したweb コンテナを必要とせずに、バックグラウンドでログインフェーズを実装します。

リフレッシュレスのログインとログアウトのためにTempPassを実装する際にプログラマーが認識する必要がある側面は次のとおりです。

- 認証を開始する前に、iFrameまたはポップアップウィンドウを、TempPass以外のMVPDに対してのみ作成する必要があります。 プログラマは、MVPD オブジェクトの`tempPass` プロパティを読み取ることで、MVPDがTempPassであるかどうかを検出できます（`setConfig()` / `displayProviderDialog()`によって返されます）。

- `createIFrame()` コールバックにはTempPassのチェックを含める必要があり、MVPDがTempPassでない場合にのみロジックを実行する必要があります。

- `destroyIFrame()` コールバックにはTempPassのチェックを含める必要があり、MVPDがTempPassでない場合にのみロジックを実行する必要があります。

- `setAuthenticationStatus()`および`sendTrackingData()` コールバックは、認証が完了した後に呼び出されます（通常のMVPDのリフレッシュレスフローとまったく同じです）。

>[!NOTE]
>
>このフローは、更新なしTempPassでのみ使用できます。 リフレッシュフローでは、TempPassを明示的に処理する必要があります（TempPassでiFrame / popupが必要な場合）

</br>

次のコードサンプルは、プログラマーのweb サイト（通常のMVPDとTempPassの両方）でMVPD ウィンドウを処理する方法を示しています。

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
