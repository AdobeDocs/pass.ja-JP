---
title: ユースケース
description: 同時実行性モニタリングのユースケース
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---

# ユースケース {#use-cases}

ストリームカウントサービスの主なユースケースは、ユーザーが視聴した同時ビデオストリームの数をカウントし、同じアカウント ID の同時な使用に関する決定を提供することです。

加入者の利用状況を監視するためには、プログラマーの Web サイトやアプリケーション、MVPD のコンテンツポータル、または同時配信プロパティのいずれで発生したかに関係なく、ユーザーアクティビティを集計できる一元化されたサービスが必要です。

この一元化されたサービスでサポートされる主なユースケースは、次のとおりです。

1. 購読者がビデオの視聴を開始するとすぐに、アプリケーションは **ストリーミングセッションを初期化** し、**レポートアクティビティ** データを開始できます。
1. 同じ中央サービスで、別のインスタンスが ***CM 決定*** を受け取ります。アプリケーションが 1 つ以上のポリシーを CM サービスに登録している場合、サービスは、現在のアクティビティに基づいてアクセス決定で応答します。


## セッションの作成 {#create-session}

この API 呼び出しを使用すると、ユーザーが「再生」ボタンを押してコンテンツを視聴する際に、クライアントが新しい CM セッションを作成できます。 サーバー応答には、ストリームを維持するための新しいストリーム URL （ストリーム ID を含む）と、ストリームがタイムアウトする時間が含まれます。 これにより、クライアントアプリケーションが、ハートビートを通じてアクティビティをレポートすることが期待されます。 セッション初期化呼び出しでは、フォームデータ（またはクエリ文字列パラメーター）として送信されるキーと値のペアの形式でメタデータを含める必要があります。 さらに、応答には、再生が「ポリシーに準拠」しているかどうかを示すフラグも含まれます。 ない場合、再生は許可されません。

## レポートアクティビティ {#reporting-activity}

セッションを作成した後、そのストリームをアクティブなままにするには、アプリケーションでハートビートを定期的に送信する必要があります。 また、ユーザーが再生を停止するとクライアントアプリがストリームを停止し、タイムアウトまでストリームがアクティブとカウントされないようにすることをお勧めします。

ハートビート呼び出しの応答により、クライアントアプリケーションは（ポリシーに準拠している場合は）ビデオ再生を続行したり、ビデオ再生を停止するように指示したりできます。 ビデオストリームが準拠していない場合、クライアントアプリケーションはビデオストリームを停止する必要があります。 応答には、クライアントアプリケーションにエラーメッセージを表示するための情報や、ユーザーが再生を続行するために使用できるアクションが表示されます。
