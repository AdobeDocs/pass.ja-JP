---
title: IOS SDK 3.2以降でのSFSafariViewController サポート
description: IOS SDK 3.2以降でのSFSafariViewController サポート
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%
---
# （レガシー） iOS SDK 3.2以降でのSFSafariViewController サポート {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

>[!IMPORTANT]
>
> [製品のお知らせ](/help/authentication/product-announcements.md) ページに集計されている最新のAdobe Pass認証製品のお知らせと廃止予定について、常に情報を得てください。

</br>


**セキュリティ要件により、一部のMVPDのログインページは、Web ビューではなくSFSafariViewControllerで表示する必要があります。**

一部のMVPDでは、ログインページをSFSafariViewControllerのような安全なブラウザーコントロールで表示する必要があります。 Web ビューを積極的にブロックしているため、認証するにはSVCを使用する必要があります。

## 互換性 {#compatiblity}

IOS SDK バージョン 3.1以降、AccessEnabler SDKは、サーバ設定に基づいて、SFSafariViewControllerに特定のMVPDのログインページを自動的に表示します。

SDKのバージョン 3.1では、アプリケーションのルートビューコントローラーからSFSafariViewControllerが自動的に表示されます。 これにより、実装者のログインページ管理が簡素化されますが、アプリの特定の実装（既に表示されているモーダルコントローラーなど）により、ルートビューコントローラーからSFSafariViewControllerを表示できない場合があります。

そのような場合、3.2 バージョンでは、プログラマがSVCを手動で管理する機能が導入されます。

## 手動SVC管理 {#manual-svc-management}

SVCを手動で管理するには、実装者は次の手順を実行する必要があります。


1. accessEnablerの初期化の後、**setOptions （[&quot;handleSVC&quot;:true]）**&#x200B;を呼び出します（認証を開始する前にこの呼び出しを実行してください）。 これにより、「手動」 SVC管理が有効になり、SDKはSVCを自動的に表示しませんが、必要に応じて&#x200B;**navigate （toUrl:*{url}* useSVC:true）**&#x200B;を呼び出します。

1. 実装の内部にオプションのコールバック **`navigateToUrl:useSVC:`**&#x200B;を実装します。指定されたURLを使用してSFSafariViewController インスタンスを使用してsvc インスタンスを作成し、画面に表示する必要があります。

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***メモ：***

   - *必要に応じてSFSafariViewControllerをカスタマイズできます。 例えば、iOS 11以降では、「完了」ラベルを「キャンセル」に変更できます。*
   - *svcを却下するには、そのsvcを参照する必要があります。**navigateToUrl:useSVC***の範囲で作成しないでください
   - *「myController」に独自のビューコントローラーを使用*


1. アプリケーションの&#x200B;**application （\_app: UIApplication, open url: URL, options: \[UIApplicationOpenURLOptionsKey: Any\]） -\> Bool**&#x200B;のデリゲート実装で、svcを閉じるコードを追加します。 **accessEnabler.handleExternalURL （）**&#x200B;を呼び出すコードを既に用意しておく必要があります。 以下に追加します。

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   繰り返しますが、svcは、手順2で作成したSFSafariViewControllerへの参照です。


1. **SFSafariViewControllerDelegate**&#x200B;から&#x200B;**safariViewControllerDidFinish （\_ コントローラ：SFSafariViewController）**&#x200B;を実装して、ユーザーが「完了」ボタンを使用してsvcをキャンセルしたタイミングを把握します。 この関数では、認証が取り消されたことをSDKに通知するには、次を呼び出す必要があります。

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
