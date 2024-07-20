---
title: iOS SDK 3.2 以降での SFSafariViewController のサポート
description: iOS SDK 3.2 以降での SFSafariViewController のサポート
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# iOS SDK 3.2 以降での SFSafariViewController のサポー {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>


**セキュリティ要件により、一部の MVPD のログインページは、Web ビューではなく SFSafariViewController で表示する必要があります。**

一部の MVPD では、ログインページを SFSafariViewController のような安全なブラウザ制御で表示する必要があります。 Web ビューはアクティブにブロックされているため、認証を行うためには SVC を使用する必要があります。

## 互換性 {#compatiblity}

iOS SDK Version 3.1 以降、AccessEnabler SDK は、サーバの構成に基づいて、SFSafariViewController 内の特定の MVPD のログイン・ページを自動的に表示します。

SDK のバージョン 3.1 では、アプリケーションのルート ビューコントローラから SFSafariViewController が自動的に表示されます。 これにより、実装者のログインページ管理は簡素化されますが、アプリケーションの特別な実装（既に表示されているモーダルコントローラなど）が原因で、ルートビューコントローラから SFSafariViewController を提示できない場合があります。

このような場合、3.2 バージョンでは、プログラマが SVC を手動で管理する機能が導入されています。

## 手動による SVC 管理 {#manual-svc-management}

SVC を手動で管理するには、実装担当者は次の手順を実行する必要があります。


1. accessEnabler の初期化後に **setOptions （[&quot;handleSVC&quot;:true]）** を呼び出します（この呼び出しが実行されていることを確認してから認証が開始されます）。 これにより、「手動」の SVC 管理が可能になります。SDK は SVC を自動的に表示するのではなく、必要に応じて表示します     **navigate （toUrl:*{url}* useSVC:true）** を呼び出します。

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
   - *svc を解除するには、svc への参照が必要です。**navigateToUrl:useSVC のスコープ内に svc を作成しないでください***
   - *「myController」用に独自のビューコントローラを使用*


1. アプリケーションの **application （\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]） -\> Bool** のデリゲート実装で、svc を閉じるコードを追加します。 **accessEnabler.handleExternalURL （）** を呼び出すコードが既に存在しているはずです。 の下に、次を追加します。

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   ここでも、svc は手順 2 で作成した SFSafariViewController への参照です。


1. ユーザーが **完了」ボタンを使用して svc をキャンセルしたときに検出するために、** SFSafariViewControllerDelegate **から** safariViewControllerDidFinish （\_ controller: SFSafariViewController）を実装します。 この関数で、認証がキャンセルされたことを SDK に通知するには、次を呼び出す必要があります。

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
