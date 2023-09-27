---
title: iOSで一時パスをリセット
description: iOSで一時パスをリセット
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# iOSで一時パスをリセット {#reset-temp-pass-on-ios}

>[!NOTE]
>
>このページのコンテンツは、情報提供の目的でのみ提供されます。 この API を使用するには、Adobeの現在のライセンスが必要です。 不正な使用は許可されていません。

</br>

iOS Demo App には、Temp Pass TTL をリセットするための専用画面が含まれています。 リセット操作には次の情報が必要です。

- **環境：** reset Temp PassAdobe呼び出しを受け取るネットワーク Pay-TV パスサーバーエンドポイントを指定します。 可能な値： **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **リリース** (*mgmt.auth.adobe.com*) または **カスタム** (Adobe内部テスト用に予約 )。
- **OAuth2 Bearer トークン：** OAuth2 トークンは、AdobePay-TV 認証用のプログラマーを認証するために必要です。 このようなトークンは、専用の Pay-TV 認証 OAuth2 エンドポイント ( 例： *curl -u &quot;\&lt;consumer _key=&quot;&quot;>:\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*) をクリックします。
- **要求者 ID :** 現在のプログラマーの一意の ID。 この値は、Demo App のメイン画面（requestor フィールド）から読み取られます。
- **一時パス ID :** 一時パス MVPD の一意の ID。
- **デバイス ID :** デモアプリで計算されるハッシュ化されたデバイス ID。
- **汎用キー：** 一部の Temp Pass MVPD（次に拡張可能な Temp Pass 機能）は、（デバイス ID と共に）Temp Pass をリセットするための汎用キーをサポートしています。

上記のパラメーター ( *汎用キー*) は必須です。 次に、Demo App によって実行されるパラメーターと関連するネットワーク呼び出しの例を示します（例は*curl *コマンドの形式です）。

- **環境：** リリース (*mgmt.auth.adobe.com*)
- **OAuth2 Bearer トークン：** H4j7cF3GtJX81BrsgDa10GwSizVz
- **プログラマー ID:** REF
- **一時パス ID :** TempPassREF
- **デバイス ID :** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **汎用キー：** null （値が指定されていません）

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

DELETEHTTP リクエストが **/reset** エンドポイント、を渡す *OAuth2 Bearer トークン* を設定し、 *デバイス ID*, *要求者 ID* および *一時パス ID (MVPD ID)* をパラメーターとして使用します。

プログラマが *汎用キー*&#x200B;を呼び出すと、別の HTTP 呼び出しが実行されます ( 今回は **/reset/generic** エンドポイント )、を渡す *汎用キー* 内側 *key* リクエストパラメーター。

例えば、 *汎用キー* を（この種の機能をサポートする Temp Pass MVPDs 用に）電子メールアドレスハッシュに対して送信すると、次の HTTP 呼び出しが行われます ( 電子メールは `user@domain.com` その SHA-256 ハッシュは `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

重要な点は、Demo App で Temp Pass をリセットすると、同じデバイス上のプログラマーアプリで同じ効果が得られない場合があることです。 これは、デバイス上のすべてのアプリケーションで、（Demo App と AccessEnabler で計算された）デバイス ID が同じでない可能性があるためです。

- iOS 6 以下：デバイス ID は、（すべてのアプリごとに一意の）MACアドレスを使用して計算されるので、Demo App で Temp Pass をリセットすると、デバイス上の他のすべての Programmer アプリでリセットされます。

- iOS 7 以降：デバイス ID は、 IDFV（ベンダーの場合は ID）値に基づいて計算されます。この値は、同じバンドル ID プレフィックスを持つすべてのアプリケーション（つまり、最後を除くすべてのコンポーネント）ごとに一意です。 Demo App と Programmer アプリは異なるバンドル ID を持つので、Demo App で Temp Pass をリセットしても、プログラマーアプリには影響しません。

最後の使用例 (iOS 7 以降 ) が最も一般的なので、プログラマーがこの状況でアプリの Temp Pass をリセットする方法を見てみましょう。 次のオプションがあります。

1. Demo App から Programmer アプリにコードを移植します。 The *TempPassResetViewController* および *DeviceIdDemoApp* クラスには、Temp Pass をリセットするためのコアロジックが含まれており、簡単に変更して Programmer アプリに組み込むことができます。

1. 次を使用して、一時パスをリセットするための HTTP リクエストを実行します。 *curl*. device\_Id パラメータは、プログラマーアプリの IDFV を計算し、それに対して SHA-256 ハッシュを適用することで取得できます ( *DeviceIdDemoApp* クラス ) です。

1. プログラマーのアプリのハッシュ化された IDFV を *汎用キー*. これは、2 つのネットワーク呼び出しになります。1 つは、デモアプリ（プログラマーに関係ない）の Temp Pass をリセットするためのもので、もう 1 つはプログラマーアプリの Temp Pass をリセットするためのものです。

上記のオプションはすべて似ています。簡単な実装に応じて 1 つを選択するのはプログラマー次第です。
