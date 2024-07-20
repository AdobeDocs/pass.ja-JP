---
title: Adobe Pass認証とAndroid 6 「Marshmallow」新しい権限モデル
description: Adobe Pass認証とAndroid 6 「Marshmallow」新しい権限モデル
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Adobe Pass認証とAndroid 6 「Marshmallow」新しい権限モデル {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

新しいAndroid 6 Marshmallow リリースでは、権限モデルが更新されており、既存のAdobe Pass Authentication SDK バージョン 1.8 以前を使用するアプリの動作に影響を与える可能性があります。

新しいAndroid OS は新機能として、[ インストール時および実行時にアプリで必要な権限を詳細に制御 ](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html) できます。

>[!IMPORTANT]
>
>以下に説明する変更内容は、**Android 6.0 専用に開発されたアプリケーションにのみ影響します** （targetSdkVersion=23）。 Android 6.0 へのアップグレード時に、ユーザーのデバイスに既にインストールされている古いアプリケーションには影響しません。


特に、Android Studio で [API レベル 23](http://developer.android.com/sdk/api_diff/23/changes.html) を使用して開発され、Adobe Pass Authentication SDK を使用するアプリの場合、開発者はカスタムコード（以下のコードスニペットを参照）を記述して [ 権限の許可/拒否ダイアログをトリガーする ](https://developer.android.com/training/permissions/requesting.html) 必要があります。

次に、デバイス外部ストレージへの書き込みアクセスのリクエストに使用するコード抜粋を示します。

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**ユーザーの視点から見ると** インストール時には、ユーザーにファイルの読み取り/書き込み権限を確認するように促すウィンドウが表示されます（下図 2 を参照）。 これにより、次の 2 つの結果のいずれかが生じます。

1. ユーザーが権限を **確認** した場合、通常の認証フローが保持され、トークンがグローバルストレージに保存されます。 トークンが有効である限り、ユーザーは、Adobe Pass認証を使用してアプリ内およびアプリ間で認証されたままになります。
1. ユーザーが権限を **拒否** すると、ストレージでの書き込みアクションが失敗し、ユーザーはアプリを終了するまで認証されません。 フォアグラウンドとバックグラウンドを切り替えると、一部のアプリケーションが再初期化されるため、このアクションを実行するとユーザーはログアウトされることに注意してください。 トークンは格納されず、ユーザーはアプリを使用するたびに認証する必要があります。


>[!TIP]
>
>ストレージの復元を実現する機能が、現在、Adobe Pass Authentication SDK 1.9 で開発中です。新しい SDK は **10 月最終週にリリースされる予定** です。 一般ストレージを使用できない場合、アプリケーションはアプリケーションのサンドボックスストレージへの書き込みにフォールバックします。 ここでは、API レベル 23 で開発されたアプリケーションの場合、ユーザーがグローバルストレージでの読み取り/書き込み権限を受け付けない場合について説明します。 トークンはアプリごとに個別に保存されるため、Adobe Pass認証を使用したアプリ間のシングルサインオンは無効になります。


![](assets/android-permissions-request.png)

*図：ターゲット設定 API レベル 23 で記述されたアプリの権限リクエストダイアログ*

>[!IMPORTANT]
>
> Adobeは、**パートナーは、認証プロセスで可能な限り最高のユーザーエクスペリエンスを保証するために、API レベル 22 （targetSdkVersion=22）以前を使用してアプリを開発すること** を推奨します。
