---
title: 更新なしのログインとログアウト
description: 更新なしのログインとログアウト
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# 更新なしのログインとログアウト {#tefresh-less-login-and-logout}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

## 概要 {#overview}

Web アプリケーションの場合、ユーザーの認証およびログアウトに使用できる様々なシナリオを考慮する必要があります。  MVPD では、ユーザーが MVPD の web ページにログインして認証を行う必要がありますが、次の追加要因が生じます。

- 一部の MVPD は、サイトからログインページへの完全なリダイレクトを必要とします
- 一部の MVPD では、MVPD のログインページを表示するために、サイトで iFrame を開く必要があります
- 一部のブラウザーでは、iFrame シナリオが適切に処理されないので、これらのブラウザーに対しては、iFrame の代わりにポップアップウィンドウを使用する方が良い方法です

Adobe Pass Authentication 2.7 以前は、ユーザーを認証するこれらのシナリオはすべて、プログラマーのページの完全な更新を伴っていました。2.7 以降のリリースでは、Adobe Pass Authentication チームは、ユーザーがログインおよびログアウト中にアプリのページを更新する必要がなくなるよう、これらのフローを改善しました。


## 詳細な説明 {#detailed_description}

最初に、元の認証フローとログアウトフローの概要を説明し、次に、改善された認証フローとログアウトフローを説明します。 最初の 4 つのセクションは通常の MVPD （TempPass 以外）を対象としていますが、最後のセクションは TempPass に適用する必要がある特別な実装について説明しています。

- [オリジナル認証フロー](#orig_authn)
- [元のログアウトフロー](#orig_logout)
- [認証フローの向上](#improved_authn)
- [ログアウトフローの改善](#improved_logout)
- [TempPass フロー](#improved_temppass)

</br>

## 元の認証/ログアウトフロー {#orig_authn}

**認証**

Adobe Pass認証 Web クライアントには、MVPD の要件に応じて、2 つの認証方法があります。

1. **全ページリダイレクト -** ユーザーがプロバイダーを選択した後    の MVPD ピッカーから（フルページリダイレクトで設定）    AccessEnabler 上で起動され `setSelectedProvider(<mvpd>)` プログラマの Web サイト。ユーザーは MVPD のログイン・ページにリダイレクトされます。 ユーザーが有効な資格情報を指定すると、プログラマーの web サイトにリダイレクトされます。 AccessEnabler が初期化され、`setRequestor` の実行中にAdobe Pass Authentication から認証トークンが取得されます。
1. **iFrame / ポップアップウィンドウ -** ユーザーがプロバイダー（iFrame で設定）を選択し `setSelectedProvider(<mvpd>)` 後、AccessEnabler で呼び出されます。 この操作により、`createIFrame(width, height)` コールバックがトリガーされ、名前 `"mvpdframe"` と指定されたサイズを使用して iFrame （またはブラウザーや環境設定に応じてポップアップ）を作成するようにプログラマーに通知されます。 iFrame/popup の作成後、AccessEnabler は iFrame/popup に MVPD のログイン ページをロードします。 ユーザーは有効な資格情報を提供し、iFrame/popup はAdobe Pass Authentication にリダイレクトされます。これにより、iFrame/popup を閉じて親ページ（プログラマーの web サイト）をリロードする JS スニペットが返されます。 フロー 1 と同様に、認証トークンは `setRequestor` 信中に取得されます。

`displayProviderDialog` コールバック（`getAuthentication`/`getAuthorization` によってトリガー）は、MVPD のリストと適切な設定を返します。 MVPD の `iFrameRequired` プロパティを使用すると、プログラマはフロー 1 またはフロー 2 をアクティブにする必要があるかどうかを知ることができます。 プログラマーは、フロー 2 に対してのみ追加のアクション（iFrame/ポップアップの作成）を実行する必要があります。

**認証のキャンセル**

また、ユーザーがログインページを閉じることで、認証フローを明示的にキャンセルする場合もあります。 次に、プログラマーに対して提案するシナリオと解決策を示します。

1. **フルページリダイレクト -** ログインページが閉じられると、ユーザーはプログラマーの web サイトに再度移動し、最初からフロー全体を開始する必要があります。 このシナリオでは、プログラマー側での明示的なアクションは必要ありません。
1. **iFrame -** プログラマーは、「閉じる」ボタンがアタッチされた `div` （または同様の UI コンポーネント）内で iFrame をホストすることをお勧めします。 ユーザーが「閉じる」ボタンを押すと、プログラマーは関連する UI と共に iFrame を破棄し、`setSelectedProvider(null)` を実行します。 この呼び出しを使用すると、AccessEnabler は内部状態をクリアし、その後の認証フローを開始できるようになります。 `setAuthenticationStatus` と `sendTrackingData(AUTHENTICATION_DETECTION...)` は、（`getAuthentication` と `getAuthorization` の両方で）失敗した認証フローを知らせるためにトリガーされます。
1. **ポップアップ** 一部のブラウザーではウィンドウの閉じるイベントを正確に検出できないので、ここでは別の方法を採用する必要があります（上記の iFrame フローとは対照的です）。 Adobeでは、ログインポップアップの有無を定期的に確認するタイマーをプログラマーが初期化することをお勧めします。 ウィンドウが存在しない場合、プログラマーはユーザーが手動でログインフローをキャンセルしたことを確認でき、プログラマーは `setSelectedProvider(null)` の呼び出しを続行できます。 トリガーされたコールバックは、上記のフロー 2 と同じです。

</br>

## 元のログアウトフロー {#orig_logout}

AccessEnabler のログアウト API は、ライブラリのローカル状態をクリアし、現在のタブ/ウィンドウに MVPD のログアウト URL をロードします。 ブラウザーは MVPD のログアウトエンドポイントに移動し、プロセスが完了すると、ユーザーはプログラマーの web サイトにリダイレクトされます。 ユーザーの代わりに必要なアクションは、ログアウトボタン/リンクを押してフローを開始することのみです。MVPD のログアウトエンドポイントではユーザーの操作は必要ありません。

**ページ更新を含む元の認証/ログアウトフロー**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## 認証の向上（更新なし） {#improved_authn}

>[!NOTE]
>
>更新のないログインとログアウトのフローが改善されたので、ブラウザーが web メッセージングを含む最新のHTML5 テクノロジーをサポートする必要があります。

前述の認証（ログイン）フローとログアウトフローは、どちらも、各フローが完了した後にメインページをリロードすることで、同様のユーザーエクスペリエンスを提供します。  現在の機能は、更新なしのログイン（バックグラウンド）とログアウトを提供することで、ユーザーエクスペリエンスの向上を目的としています。 プログラマーは、2 つのブール値フラグ（`backgroundLogin` と `backgroundLogout`）を `setRequestor` API の `configInfo` パラメーターに渡すことで、バックグラウンドでのログインとログアウトを有効/無効にできます。 デフォルトでは、バックグラウンドのログイン/ログアウトは無効になっています（これにより、以前の実装との互換性が確保されます）。

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

次のポイントは、元の認証フローと改善されたフローの間の移行を説明します。

1. フルページリダイレクトは、MVPD ログインが実行される新しいブラウザータブに置き換えられます。 プログラマーは、ユーザーが（`iFrameRequired = false` を使用して） MVPD を選択すると、`mvpdwindow` という名前の新しいタブを（`window.open` を使用して）作成する必要があります。 次に、プログラマは `setSelectedProvider(<mvpd>)` を実行し、AccessEnabler が MVPD ログイン URL を新しいタブにロードできるようにします。 ユーザーが有効な認証情報を指定すると、Adobe Pass Authentication はタブを閉じ、window.postMessage をプログラマーの Web サイトに送信し、認証フローが完了したことを AccessEnabler に知らせます。 次のコールバックがトリガーされます。

   - フローが `getAuthentication` によって開始された場合：`setAuthenticationStatus` および `sendTrackingData(AUTHENTICATION_DETECTION...)` がトリガーされて、認証の成功/失敗が通知されます。

   - フローが `getAuthorization` によって開始された場合：`setToken/tokenRequestFailed` および `sendTrackingData(AUTHORIZATION_DETECTION...)` がトリガーされて、承認の成功/失敗を知らせます。

1. iFrame/ポップアップウィンドウのフローはほとんど変更されません。違いは、ユーザーが有効な資格情報を指定した後、親ページが再読み込みされないことです。 ログイン後に iFrame/ポップアップが自動的に閉じ、`window.postMessage` が親ページに送信されて、フローが完了したことを AccessEnabler に通知します。 前のフローと同じコールバックがトリガーされます **さらに、新しいコールバックとして`destroyIFrame`** がトリガーされます。 `destroyIFrame` コールバックを使用すると、プログラマーは iFrame に関連付けられたコンポーネントや、UI 装飾などの補助コンポーネントを削除できます。 ログインが完了すると、Adobe Pass認証によってプログラマーのページが再読み込みされ、その上のすべての UI コンポーネントが破棄されるので、このコールバックは古い認証フローでは必要ありませんでした。

</br>

>[!IMPORTANT]
> 
>MVPD のログイン iFrame またはポップアップ・ウィンドウを、AccessEnabler インスタンスを含むページの直接の子としてロードする必要があります。 MVPD のログイン iFrame またはポップアップ・ウィンドウが、AccessEnabler インスタンスを含むページの下の 2 つ以上のレベルにネストされている場合、フローがハングする可能性があります。 たとえば、メイン ページと MVPD iFrame （Page =\> iFrame =\> MVPD iFrame）の間に iFrame がある場合、ログイン フローは失敗する可能性があります。

</br>

**認証のキャンセル**

認証をキャンセルするためのフローを次に示します。

1. **ブラウザータブ -** タブは基本的に新しいウィンドウなので、クローズイベントのキャプチャには、古い認証フローからのシナリオ 3 で説明したのと同じ制限があります。 さらに、ユーザーが手動で閉じたタブと、ログインフローの最後に自動的に閉じたタブを区別する方法がないため、ここではタイマーのアプローチはできません。 この場合の解決策は、ユーザーがフローをキャンセルしても AccessEnabler が「サイレント」（コールバックがトリガーされない）状態を維持することです。 また、プログラマーは特定のアクションを実行する必要はありません。 ユーザーは、「Multiple Authentication Requests Error」エラーを受け取ることなく、別の認証フローを開始できます（このエラーは、バックグラウンド・ログイン用の AccessEnabler で無効化されています）。

1. **iFrame**：プログラマーは、シナリオ 2 で説明したアプローチを古い認証フローから実行できます（iFrame および関連する [ 閉じる ] ボタンからラッパー UI を作成し、トリガーが `setSelectedProvider(null)` きます）。 このアプローチは強力な要件ではなくなりましたが（前述のシナリオ 1 で説明したように、バックグラウンドログインには複数の認証フローを使用できます）、Adobeでは引き続きお勧めします。

1. **ポップアップ -** 上記の「ブラウザー」タブのフローと同じです。

</br>

## ログアウトフローの改善 {#improved_logout}

新しいログアウトフローは非表示の iFrame で実行されるので、ページ全体のリダイレクトが不要になります。  これは、ユーザーが MVPD のログアウトページで特定のアクションを実行する必要がないために可能です。

ログアウトフローが完了すると、カスタム Adobe Pass認証エンドポイントに iFrame がリダイレクトされます。 これにより、親に対して `window.postMessage` を実行する JS スニペットが提供され、ログアウトが完了したことが AccessEnabler に通知されます。 次のコールバックがトリガーされます。`setAuthenticationStatus()` および `sendTrackingData(AUTHENTICATION_DETECTION ...)` は、ユーザーが認証されなくなったことを示します。

次の図は、アプリケーションのメインページを更新せずに MVPD にログインできる更新なしのフローを示しています。

**改善された（更新のない）認証/ログアウトフロー**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass フロー {#improved_temppas}

リフレッシュレスログインは、TempPass タイプの MVPD に対する別のアプローチに従います。

TempPass フローでは、ウィンドウを自動的に作成し、明示的なユーザーの操作なしに閉じる必要があるので、一部のブラウザー（ポップアップブロッカー）では問題が発生する可能性があります。 したがって、AccessEnabler は、プログラマが作成した Web コンテナを必要とせずに、バックグラウンドでログイン段階を実装します。

更新なしログインおよびログアウト用に TempPass を実装する際に、プログラマーが認識する必要がある側面を次に示します。

- 認証を開始する前に、iFrame またはポップアップウィンドウは TempPass 以外の MVPD に対してのみ作成する必要があります。 プログラマは、MVPD オブジェクトの `tempPass` プロパティ （`setConfig()` / `displayProviderDialog()` で返される）を読み取ることで、MVPD が TempPass かどうかを検出できます。

- `createIFrame()` コールバックは TempPass のチェックを含み、MVPD が TempPass でない場合にのみロジックを実行する必要があります。

- `destroyIFrame()` コールバックは TempPass のチェックを含み、MVPD が TempPass でない場合にのみロジックを実行する必要があります。

- `setAuthenticationStatus()` コールバックと `sendTrackingData()` コールバックは、認証が完了した後に呼び出されます（通常の MVPD のリフレッシュレスフローとまったく同じ）。

>[!NOTE]
>
>このフローは、更新なしの TempPass でのみ使用できます。 更新フローでは、TempPass を明示的に処理する必要があります（TempPass が iFrame / ポップアップを必要とする場合）

</br>

次のコード例は、プログラマーの web サイト上で MVPD ウィンドウを処理する方法を示しています（通常の MVPD の場合と TempPass の場合の両方）。

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
