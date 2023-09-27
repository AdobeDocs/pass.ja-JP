---
title: 更新なしのログインとログアウト
description: 更新なしのログインとログアウト
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1772'
ht-degree: 0%

---

# 更新なしのログインとログアウト {#tefresh-less-login-and-logout}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

## 概要 {#overview}

Web アプリケーションの場合、ユーザーの認証とログアウトをおこなうために、考えられる様々なシナリオをアカウント化する必要があります。  MVPDs では、次の追加要因を使用して、ユーザーが MVPD の Web ページにログインして認証する必要があります。

- 一部の MVPD では、サイトからログインページへの完全なリダイレクトが必要です。
- 一部の MVPD では、MVPD のログインページを表示するために、サイトで iFrame を開く必要があります。
- 一部のブラウザーでは iFrame のシナリオが正しく処理されないので、これらのブラウザーでは、iFrame の代わりにポップアップウィンドウを使用する方がよい方法です

Adobe Pass認証 2.7 より前は、ユーザーを認証するためのすべてのシナリオは、プログラマーのページの完全な更新を伴いました。 2.7 以降のリリースでは、Adobe Pass認証チームは、ログインとログアウト時にアプリでページを更新する必要がなくなりました。


## 詳細な説明 {#detailed_description}

最初に、元の認証フローとログアウトフローの概要を示し、次に、認証フローとログアウトフローを改善して概要を説明します。 最初の 4 つのセクションは、通常の MVPD (non-TempPass) を扱い、最後のセクションは、TempPass に適用する必要のある特別な実装を示します。

- [元の認証フロー](#orig_authn)
- [元のログアウトフロー](#orig_logout)
- [認証フローの改善](#improved_authn)
- [ログアウトフローの改善](#improved_logout)
- [TempPass フロー](#improved_temppass)

</br>

## 元の認証/ログアウトフロー {#orig_authn}

**認証**

Adobe Pass Authentication Web クライアントは、MVPD の要件に応じて、次の 2 つの認証方法を持ちます。

1. **フルページリダイレクト —** ユーザーがプログラマーの Web サイトの MVPD ピッカーからプロバイダ（全ページリダイレクトで設定）を選択した後、 `setSelectedProvider(<mvpd>)` が AccessEnabler 上で呼び出され、ユーザーは MVPD のログインページにリダイレクトされます。 ユーザーが有効な資格情報を入力すると、プログラマーの Web サイトにリダイレクトされます。 AccessEnabler が初期化され、次の時点でAdobe Pass Authentication から認証トークンが取得されます。 `setRequestor`.
1. **iFrame /ポップアップウィンドウ —** ユーザーが（iFrame で設定された）プロバイダーを選択した後、 `setSelectedProvider(<mvpd>)` が AccessEnabler 上で呼び出されます。 このアクションは、トリガー `createIFrame(width, height)` コールバック、名前を持つ iFrame（またはポップアップ — ブラウザ/環境設定に応じて）を作成するようプログラマーに通知 `"mvpdframe"` と指定されたディメンション。 iFrame/popup が作成された後、AccessEnabler は iFrame/popup に MVPD のログインページを読み込みます。 ユーザーが有効な資格情報を入力すると、iFrame/popup がAdobe Pass Authentication にリダイレクトされ、iFrame/popup を閉じて親ページ（プログラマー Web サイト）をリロードする JS スニペットが返されます。 フロー 1 と同様に、認証トークンは `setRequestor`.

The `displayProviderDialog` コールバック ( `getAuthentication`/`getAuthorization`) は、MVPD のリストとその適切な設定を返します。 The `iFrameRequired` MVPD のプロパティを使用すると、プログラマーは、フロー 1 またはフロー 2 を有効にする必要があるかどうかを知ることができます。 プログラマーは、フロー 2 に対してのみ追加のアクション（iFrame/ポップアップの作成）を実行する必要があることに注意してください。

**認証をキャンセル**

また、ユーザーがログインページを閉じて認証フローを明示的にキャンセルする場合もあります。 以下に、シナリオと、プログラマーに対して提案された解決策を示します。

1. **フルページリダイレクト —** ログインページが閉じられると、ユーザーは再びプログラマーの Web サイトに移動し、最初からフロー全体を開始する必要があります。 このシナリオでは、プログラマー側で明示的なアクションは必要ありません。
1. **iFrame -** プログラマーは、iFrame を `div` （または類似の UI コンポーネント）に「閉じる」ボタンが付いている状態。 ユーザーが [ 閉じる ] ボタンを押すと、プログラマは関連する UI と共に iFrame を破棄し、 `setSelectedProvider(null)`. この呼び出しにより、AccessEnabler は内部状態をクリアし、ユーザーが以降の認証フローを開始できるようになります。 `setAuthenticationStatus` および `sendTrackingData(AUTHENTICATION_DETECTION...)` がトリガーされ、認証フローの失敗を伝えます ( 両方とも `getAuthentication` および `getAuthorization`) をクリックします。
1. **ポップアップ —** 一部のブラウザーでは、ウィンドウの閉じるイベントを正確に検出できないので、（上記の iFrame のフローとは異なり）別の方法を使用する必要があります。 Adobeは、プログラマがログインポップアップの存在を定期的に検証するタイマーを初期化することを推奨します。 ウィンドウが存在しない場合、プログラマは、ユーザが手動でログインフローをキャンセルし、プログラマが呼び出しを続行できることを確認できます `setSelectedProvider(null)`. トリガーされるコールバックは、上記のフロー 2 と同じです。

</br>

## 元のログアウトフロー {#orig_logout}

AccessEnabler のログアウト API は、ライブラリのローカル状態をクリアし、MVPD のログアウト URL を現在のタブ/ウィンドウに読み込みます。 ブラウザが MVPD のログアウトエンドポイントに移動し、プロセスが完了すると、ユーザはプログラマの Web サイトにリダイレクトされます。 ユーザーに代わって必要なアクションは、ログアウトボタン/リンクを押してフローを開始することだけです。MVPD のログアウトエンドポイントでは、ユーザーの操作は必要ありません。

**元の認証/ログアウトフローとページ更新**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## （更新なし）認証の改善 {#improved_authn}

>[!NOTE]
>
>更新なしのログインフローとログアウトフローの改善には、ブラウザーが Web メッセージを含む最新のHTML5 テクノロジーをサポートしている必要があります。

上記の認証（ログイン）フローとログアウトフローの両方で説明した手順は、各フローが完了した後にメインページを再読み込みすることで、同様のユーザーエクスペリエンスを提供します。  現在の機能は、更新なし（バックグラウンド）のログインとログアウトを提供し、ユーザーエクスペリエンスを向上させることを目的としています。 プログラマは、2 つのブール型フラグ (`backgroundLogin` および `backgroundLogout`) を `configInfo` のパラメーター `setRequestor` API. デフォルトでは、バックグラウンドログイン/ログアウトは無効になっています（これは、以前の実装との互換性を提供します）。

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

次のポイントは、元の認証フローと改善されたフローとの移行を示しています。

1. フルページリダイレクトは、MVPD ログインが実行される新しいブラウザタブに置き換えられます。 新しいタブを作成するには、プログラマーが必要です ( `window.open`) という名前 `mvpdwindow` ユーザーが MVPD ( `iFrameRequired = false`) をクリックします。 プログラマーは次に実行します。 `setSelectedProvider(<mvpd>)`を使用して、AccessEnabler が新しいタブで MVPD ログイン URL を読み込めるようにします。 ユーザーが有効な資格情報を入力すると、Adobe Pass Authentication はタブを閉じ、認証フローが完了したことを AccessEnabler に通知する window.postMessage をプログラマーの Web サイトに送信します。 次のコールバックがトリガーされます。

   - フローが次の条件で開始された場合： `getAuthentication`: `setAuthenticationStatus` および `sendTrackingData(AUTHENTICATION_DETECTION...)` 認証の成功/失敗を示すためにトリガーされます。

   - フローが次の条件で開始された場合： `getAuthorization`: `setToken/tokenRequestFailed` および `sendTrackingData(AUTHORIZATION_DETECTION...)` がトリガーされ、認証の成功/失敗を示します。

1. iFrame/ポップアップウィンドウのフローは、ほとんど変更されていません。違いは、ユーザーが有効な資格情報を提供した後、親ページが再読み込みされないという点です。 ログイン後、iFrame/ポップアップが自動的に閉じ、 `window.postMessage` が親ページに送信され、フローが完了したことが AccessEnabler に通知されます。 前のフローと同様に、同じコールバックがトリガーされます。 **さらに、次の新しいコールバックが追加されました。`destroyIFrame`**. The `destroyIFrame` callback を使用すると、UI の装飾など、iFrame に関連付けられた/補助コンポーネントを削除できます。 ログインが完了した後、Adobe Pass Authentication がプログラマーのページを再読み込みし、その上のすべての UI コンポーネントを破棄するので、古い認証フローでは、このコールバックの存在は必要ありませんでした。

</br>

>[!IMPORTANT]
> 
>MVPD ログイン iFrame またはポップアップウィンドウを、AccessEnabler インスタンスを含むページの直接の子として読み込む必要があります。 MVPD ログイン iFrame またはポップアップウィンドウが、AccessEnabler インスタンスを含むページの下の 2 つ以上のレベルにネストされている場合、フローがハングする可能性があります。 例えば、メインページと MVPD iFrame(Page =\> iFrame =\> MVPD iFrame) の間に iFrame が配置されている場合、ログインフローが失敗する可能性があります。

</br>

**認証をキャンセル**

次に、認証をキャンセルするためのフローを示します。

1. **「ブラウザー」タブ —** このタブは基本的に新しいウィンドウなので、クローズイベントをキャプチャする際には、古い認証フローのシナリオ 3 で説明したものと同じ制限事項があります。 また、ユーザが手動で閉じたタブと、ログインフローの最後に自動的に閉じたタブを区別する方法がないので、ここではタイマーアプローチは不可能です。 ここでの解決策は、ユーザーがフローをキャンセルしたときに AccessEnabler が「サイレント」（コールバックがトリガーされない）状態を保つことです。 また、プログラマは特定のアクションを実行する必要はありません。 ユーザは、「複数の認証要求エラー」エラーを受け取らずに、別の認証フローを開始できます（このエラーはバックグラウンド・ログインのために AccessEnabler で無効にされています）。

1. **iFrame -** プログラマーは、シナリオ 2 で説明されているアプローチを、古い認証フローから取り入れることができます (iFrame からラッパー UI を作成し、関連する「閉じる」ボタンをトリガー `setSelectedProvider(null)`. この方法は強い要件ではなくなりましたが（前述のシナリオ 1 で説明したように、複数の認証フローがバックグラウンドログインで許可されます）、Adobeでは引き続きお勧めします。

1. **ポップアップ —** これは、上記の「ブラウザー」タブのフローと同じです。

</br>

## ログアウトフローの改善 {#improved_logout}

新しいログアウトフローは、非表示の iFrame で実行されるので、完全なページリダイレクトが排除されます。  これは、ユーザーが MVPD のログアウトページで特定のアクションを実行する必要がないため可能です。

ログアウトフローが完了したら、iFrame をカスタムAdobe Pass認証エンドポイントにリダイレクトします。 これにより、 `window.postMessage` 親にログアウトが完了したことを AccessEnabler に通知します。 次のコールバックがトリガーされます。 `setAuthenticationStatus()` および `sendTrackingData(AUTHENTICATION_DETECTION ...)`：ユーザーが認証されなくなったことを示す

以下の図に、ユーザーがアプリケーションのメインページを更新せずに MVPD にログインできる、更新のないフローを示します。

**（更新なし）認証/ログアウトフローの改善**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass フロー {#improved_temppas}

リフレッシュレスログインは、TempPass 型の MVPD に対する異なるアプローチに従います。

TempPass フローでは、ウィンドウを自動的に作成し、ユーザーの明示的な操作なしで閉じる必要があるので、一部のブラウザー（ポップアップブロッカー）で問題が発生する可能性があります。 したがって、AccessEnabler は、プログラマが作成した Web コンテナを必要とせずに、ログインフェーズをバックグラウンドで実装します。

次に、プログラマーがリフレッシュレスログインとログアウトの TempPass を実装する際に認識する必要がある要素を示します。

- 認証を開始する前に、iFrame またはポップアップウィンドウを作成する必要があるのは、TempPass 以外の MVPD に対してのみです。 MVPD が TempPass かどうかをプログラマが検出するには、 `tempPass` MVPD オブジェクトのプロパティ ( `setConfig()` / `displayProviderDialog()`) をクリックします。

- The `createIFrame()` callback は、TempPass のチェックを含み、MVPD が NOT TempPass の場合にのみそのロジックを実行する必要があります。

- The `destroyIFrame()` callback は、TempPass のチェックを含み、MVPD が NOT TempPass の場合にのみそのロジックを実行する必要があります。

- The `setAuthenticationStatus()` および `sendTrackingData()` コールバックは、認証が完了した後（通常の MVPD のリフレッシュレスフローと同様）に呼び出されます。

>[!NOTE]
>
>このフローは、リフレッシュレス TempPass でのみ使用できます。 更新フローに対して、TempPass を明示的に処理する必要があります（TempPass に iFrame /ポップアップが必要な場合）

</br>

次のコードサンプルは、プログラマーの Web サイト（通常の MVPD と TempPass の両方）で MVPD ウィンドウを処理する方法を示しています。

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
