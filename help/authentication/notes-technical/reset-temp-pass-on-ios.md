---
title: iOSの一時パスをリセット
description: iOSの一時パスをリセット
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# iOSの一時パスをリセット {#reset-temp-pass-on-ios}

>[!NOTE]
>
>このページのコンテンツは情報提供のみを目的としています。 この API を使用するには、Adobeから現在のライセンスが必要です。 無許可の使用は許可されていません。

</br>

iOS デモアプリには、Temp Pass TTL をリセットするための専用の画面が含まれています。 リセット操作には、次の情報が必要です。

- **Environment:** リセットされた一時パス ネットワーク呼び出しを受け取るAdobeの Pay-TV パス サーバーエンドポイントを指定します。 使用可能な値：**Prequal** （*mgmt-prequal.auth-staging.adobe.com*）、**リリース** （*mgmt.auth.adobe.com*）または **カスタム** （Adobeの内部テスト用に予約済み）。
- **OAuth2 ベアラートークン：** Adobeの有料テレビ認証用にプログラマーを認証するには、OAuth2 トークンが必要です。 このようなトークンは、専用の有料テレビ認証 OAuth2 エンドポイントから取得できます（例：*curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*）。
- **要求者 ID:** 現在のプログラマーの一意の ID。 この値は、デモアプリのメイン画面（リクエスターフィールド）から読み取られます。
- **一時パス ID:** 一時パス MVPD の一意の ID。
- **デバイス ID:** デモアプリによって計算された、ハッシュ化されたデバイス ID。
- **汎用キー：** 一部の一時パス MVPD （つまり、次に拡張可能な一時パス機能）は、（デバイス ID と共に）一時パスをリセットするための汎用キーをサポートしています。

上記のパラメーターはすべて（（汎用キー *を除く）必須* す。 以下に、デモアプリによって実行されるパラメーターと関連するネットワーク呼び出しの例を示します（例は*curl *コマンドの形式です）。

- **環境：** リリース（*mgmt.auth.adobe.com*）
- **OAuth2 ベアラートークン：** H4j7cF3GtJX81BrsgDa10GwSizVz
- **プログラマ ID:** 参照
- **一時パス ID:** TempPassREF
- **デバイス ID:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **汎用キー：** null （値が指定されていません）

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

DELETEHTTP リクエストが **/reset** エンドポイントに対して行われ、認証ヘッダーに *OAuth2 ベアラートークン* が渡され、*デバイス ID*、*リクエスター ID* および *一時渡し ID （MVPD ID）* がパラメーターとして渡されます。

プログラマーが *Generic Key* の値を指定すると、別の HTTP 呼び出しが実行され（今回は **/reset/generic** エンドポイントに対して）、*key* リクエストパラメーター内の *Generic Key* を渡します。

例えば、*汎用キー* を電子メールアドレスハッシュに設定します（例：
この種の機能をサポートする Temp Pass MVPD）は、
http 呼び出しの後（メールは SHA-256`user@domain.com` 送信されます）
ハッシュは `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`）:

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

デモアプリで Temp Pass をリセットしても、同じデバイス上のプログラマーアプリで同じ効果が得られない可能性があることを強調することが重要です。 これは、（デモアプリと AccessEnabler によって計算される）デバイス ID が、デバイス上のすべてのアプリケーションで同じではない可能性があるためです。

- iOS 6 以下：デバイス ID はMAC アドレス（すべてのアプリで一意）を使用して計算されるため、デモアプリで一時パスをリセットすると、デバイス上の他のすべてのプログラマーアプリで一時パスがリセットされます。

- iOS 7 以降：デバイス ID は、IDFV （ベンダーの ID）の値に基づいて計算されます。この値は、同じバンドル ID プレフィックスを持つすべてのアプリケーション（最後を除くすべてのコンポーネント）ごとに一意になります。 デモアプリとプログラマーアプリには異なるバンドル ID があるので、デモアプリで一時パスをリセットしても、プログラマーアプリには影響しません。

最後のユースケース（iOS 7 以降）が最も一般的なので、この状況でプログラマーがアプリの Temp Pass をどのようにリセットできるかを見てみましょう。 次のいくつかのオプションがあります。

1. コードをデモアプリからプログラマーアプリに移植します。 *TempPassResetViewController* クラスと *DeviceIdDemoApp* クラスには、一時パスをリセットするためのコアロジックが含まれており、それらを簡単に変更してプログラマーアプリに含めることができます。

1. *curl* で一時渡しをリセットするための HTTP リクエストを実行します。 device\_Id パラメーターは、プログラマーアプリの IDFV を計算し、その上に SHA-256 ハッシュを適用することで取得できます（*DeviceIdDemoApp* クラスのサンプルコード）。

1. プログラマーのアプリのハッシュ化された IDFV を *汎用キー* として指定することで、デモアプリからリセットを実行するだけです。 これにより、2 つのネットワーク呼び出しが発生します。1 つはデモアプリの一時パスのリセット（プログラマーとは無関係）用、もう 1 つはプログラマーアプリの一時パスのリセット用です。

上記のオプションはすべて似ています。実装の容易さに応じて、プログラマーが選択する必要があります。
