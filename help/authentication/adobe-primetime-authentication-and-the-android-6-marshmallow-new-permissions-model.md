---
title: Adobe Pass認証と Android 6 の「Marshmallow」新しい権限モデル
description: Adobe Pass認証と Android 6 の「Marshmallow」新しい権限モデル
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Adobe Pass認証と Android 6 の「Marshmallow」新しい権限モデル {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

新しい Android 6 Marshmallow リリースでは、既存のAdobe Pass Authentication SDK バージョン 1.8 以前を使用するアプリの動作に影響する、権限モデルが更新されました。

新機能として、新しい Android OS オファーが提供されます。 [アプリのインストール時と実行時に必要となる権限の詳細な制御](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html).

>[!IMPORTANT]
>
>以下に説明する変更は次のとおりです。 **Android 6.0 専用に開発されたアプリケーションにのみ影響します。** (targetSdkVersion=23)。 Android 6.0 にアップグレードする際に、ユーザーのデバイスに既にインストールされている古いアプリケーションには影響しません。


特に、Android Studio で開発されたアプリの場合は、 [API レベル 23](http://developer.android.com/sdk/api_diff/23/changes.html) Adobe Pass Authentication SDK を使用する場合、開発者はカスタムコードを作成する必要があります（以下のコードスニペットを参照）。 [許可/拒否権限ダイアログをトリガーするには](https://developer.android.com/training/permissions/requesting.html).

次に、デバイスの外部ストレージへの書き込みアクセスをリクエストする際に使用するコードの抜粋を示します。

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




**ユーザーの観点から**&#x200B;をクリックすると、インストール時に、ユーザーに対してファイルの読み取り/書き込み権限を確認するウィンドウが表示されます（下図 2 を参照）。 これにより、次の 2 つの結果のいずれかになります。

1. ユーザーが **確認** 権限、通常の認証フローが保持され、トークンはグローバルストレージに保存されます。 トークンが有効である限り、ユーザーは、アプリ内およびAdobe Pass認証を使用してアプリ全体で認証を受けます。
1. ユーザーが **拒否** 権限、ストレージでの書き込みアクションは失敗し、ユーザーがアプリを終了するまで認証されません。 フォアグラウンドとバックグラウンドを切り替えると、一部のアプリケーションが再初期化され、このアクションを実行するとユーザーがログアウトされます。 トークンは保存されず、ユーザーはアプリを使用するたびに認証が必要になります。


>[!TIP]
>
>現在、Adobe Pass Authentication SDK 1.9 で、ストレージの回復性を紹介する機能が開発中です。この新しい SDK は **10 月の最後の週に発売される**. 一般的なストレージを使用できない場合は、アプリケーションはアプリケーションのサンドボックスストレージへの書き込みにフォールバックします。 これは、API レベル 23 で開発されたアプリケーションで、ユーザーがグローバルストレージで読み取り/書き込み権限を受け入れない場合に対して行われます。 トークンはアプリごとに個別に保存されます。つまり、Adobe Pass認証を使用するアプリ間でのシングルサインオンが無効になります。


![](assets/android-permissions-request.png)

*図：ターゲット API レベル 23 で作成されたアプリの権限リクエストダイアログ*

>[!IMPORTANT]
>
> Adobe情報 **認証プロセスで最高のユーザーエクスペリエンスを保証するために、API レベル 22(targetSdkVersion=22) 以前を使用してアプリを開発するパートナー**.
