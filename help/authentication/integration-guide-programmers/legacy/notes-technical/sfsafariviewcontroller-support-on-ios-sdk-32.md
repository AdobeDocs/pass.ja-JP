---
title: iOS SDK 3.2 以降での SFSafariViewController のサポート
description: iOS SDK 3.2 以降での SFSafariViewController のサポート
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# （従来の）iOS SDK 3.2 以降での SFSafariViewController のサポート {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeの最新ライセンスが必要です。 無許可の使用は許可されていません。

>[!IMPORTANT]
>
> [&#x200B; 製品のお知らせ &#x200B;](/help/authentication/product-announcements.md) ページに集約された最新のAdobe Pass認証製品のお知らせや廃止予定タイムラインについて、常に情報を提供するようにします。

</br>


**セキュリティ要件により、一部のMVPD ログインページは、web ビューではなく SFSafariViewController で表示する必要があります。**

一部のMVPDでは、ログインページを SFSafariViewController などの安全なブラウザーコントロールに表示する必要があります。 Web ビューはアクティブにブロックされているため、認証を行うためには SVC を使用する必要があります。

## 互換性 {#compatiblity}

iOS SDK バージョン 3.1 以降では、AccessEnabler SDKは、サーバ構成に基づいて SFSafariViewController で特定のMVPDのログイン ページを自動的に表示します。

SDKのバージョン 3.1 では、アプリケーションのルート ビューコントローラから SFSafariViewController が自動的に表示されます。 これにより、実装者のログインページ管理は簡素化されますが、アプリケーションの特別な実装（既に表示されているモーダルコントローラなど）が原因で、ルートビューコントローラから SFSafariViewController を提示できない場合があります。

このような場合、3.2 バージョンでは、プログラマが SVC を手動で管理する機能が導入されています。

## 手動による SVC 管理 {#manual-svc-management}

SVC を手動で管理するには、実装担当者は次の手順を実行する必要があります。


1. accessEnabler の初期化後に **setOptions （[&quot;handleSVC&quot;:true]）** を呼び出します（この呼び出しが実行されていることを確認してから認証が開始されます）。 これにより、SVC を「手動」で管理できるようになります。SDKは SVC を自動的に表示するのではなく、必要に応じて表示します     **navigate （toUrl:*{url}* useSVC:true）** を呼び出します。

1. オプションのコールバック **`navigateToUrl:useSVC:`** を実装内に実装するには、指定された URL で SFSafariViewController インスタンスを使用して svc インスタンスを作成し、それを画面に表示する必要があります。

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***注：***

   - *SFSafariViewController は好きなようにカスタマイズできます。 例えば、iOS 11 以降では、「完了」ラベルを「キャンセル」に変更できます。*
   - *svc を解除するには、svc への参照が必要です。**navigateToUrl の範囲に svc を作成しないでください:useSVC***
   - *「myController」用に独自のビューコントローラを使用*


1. アプリケーションの **application （\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]） -\> Bool** のデリゲート実装で、svc を閉じるコードを追加します。 **accessEnabler.handleExternalURL （）** を呼び出すコードが既に存在しているはずです。 の下に、次を追加します。

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   ここでも、svc は手順 2 で作成した SFSafariViewController への参照です。


1. ユーザーが **完了」ボタンを使用して svc をキャンセルしたときに検出するために、** SFSafariViewControllerDelegate **から** safariViewControllerDidFinish （\_ controller: SFSafariViewController）を実装します。 この関数で、認証がキャンセルされたことをSDKに通知するには、以下を呼び出す必要があります。

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
