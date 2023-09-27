---
title: iOS SDK 3.2 以降での SFSafariViewController のサポート
description: iOS SDK 3.2 以降での SFSafariViewController のサポート
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# iOS SDK 3.2 以降での SFSafariViewController のサポート {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>


**セキュリティ要件のため、一部の MVPD のログインページは、Web ビューではなく SFSafariViewController で提示する必要があります。**

一部の MVPD では、SFSafariViewController のような安全なブラウザコントロールでログインページを提示する必要があります。 Web ビューをアクティブにブロックしているため、認証を行うには SVC を使用する必要があります。

## 互換性 {#compatiblity}

iOS SDK バージョン 3.1 以降、AccessEnabler SDK は、サーバの設定に基づいて、SFSafariViewController 内の特定の MVPD のログインページを自動的に表示します。

SDK のバージョン 3.1 では、アプリケーションのルートビューコントローラから SFSafariViewController が自動的に表示されます。 これにより実装者のログインページ管理が簡単になりますが、アプリの特殊な実装（既に表示されているモーダルコントローラーなど）が原因で、ルートビューコントローラーから SFSafariViewController を提示できない場合があります。

このような場合、3.2 バージョンでは、プログラマが SVC を手動で管理する機能が導入されます。

## 手動による SVC 管理 {#manual-svc-management}

SVC を手動で管理するには、実装者が次の手順を実行する必要があります。


1. 呼び出し **setOptions([&quot;handleSVC&quot;:true])** AccessEnabler の初期化後（認証を開始する前に、この呼び出しを必ず実行してください）。 これにより、「手動」の SVC 管理が有効になり、SDK は自動的に SVC を提示しませんが、必要に応じてを呼び出します **navigate(toUrl:*{url}* useSVC:true)**.

1. オプションのコールバックの実装 **navigateToUrl:useSVC:** 実装内で、指定された url を使用して SFSafariViewController インスタンスを作成し、画面に表示する必要があります。

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***メモ：***

   - *SFSafariViewController は、任意の方法でカスタマイズできます。 例えば、iOS 11 以降では、「完了」ラベルを「キャンセル」に変更できます。*
   - *svc を解除するには、その svc への参照が必要です。**navigateToUrl:useSVC***
   - *「myController」に対して独自のビューコントローラを使用する*


1. アプリケーションの委任実装で、 **application(\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]) -\> Bool**、 svc を閉じるコードを追加します。 を呼び出すコードが既に存在するはずです。 **accessEnabler.handleExternalURL()**. 追加のすぐ下：

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   この場合も、svc は、手順 2 で作成した SFSafariViewController への参照です。


1. 実装方法 **safariViewControllerDidFinish(\_ controller: SFSafariViewController)** から **SFSafariViewControllerDelegate** ユーザが「完了」ボタンを使用して svc をキャンセルした際にキャッチするために。 この関数では、認証がキャンセルされたことを SDK に通知するために、を呼び出す必要があります。

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
