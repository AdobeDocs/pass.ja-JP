---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 認証
user-guide-description: Adobe Pass は、TV Everywhere の使用権限管理ソリューションです。リソースへのアクセスをリクエストするユーザーにそのリソースへの権限が付与されているかどうかを判断するためのモジュール型フレームワークを提供します。
source-git-commit: c8b596f156c1a992b31101302412b2d52213a5ef
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 4%

---


# Adobe Pass 認証 ヘルプ {#authentication}

+ [Adobe Pass認証の概要](home.md)
+ Adobe Pass認証の概念 {#authentication-concepts}
   + [テクニカルペーパー](technical-paper.md)
   + [プログラマー向けの概要](programmer-overview.md)
   + [MVPD の概要](mvpd-overview.md)
+ キックスタートガイド {#kickstart-guides}
   + [プログラマー向けキックスタートガイド](programmer-kickstart-guide.md)
   + [MVPD キックスタートガイド](mvpd-kickstart-guide.md)
+ プログラマー統合ガイド {#programmer-integration-guide}
   + [プログラマー統合ガイドの概要](programmer-integration-guide-overview.md)
   + [プログラマーのエンタイトルメントフロー](entitlement-flow.md)
   + [プログラマーの使用例](programmer-use-cases.md)
   + [クライアント情報（デバイス、接続、およびアプリケーション）を渡す](passing-client-information-device-connection-and-application.md)
   + REST API {#restapi}
      + [REST API の概要](rest-api-overview.md)
      + [REST API クックブック（サーバー間）](rest-api-cookbook-servertoserver.md)
      + [REST API クックブック（クライアント/サーバー間）](rest-api-cookbook-clienttoserver.md)
      + Rest API リファレンス {#rest-api-reference}
         + [REST API リファレンス](rest-api-reference.md)
         + [登録コードのリクエスト](registration-code-request.md)
         + [登録レコードを返す](return-registration-record.md)
         + [登録レコードを削除](delete-registration-record.md)
         + [MVPD リストを提供](provide-mvpd-list.md)
         + [認証の開始](initiate-authentication.md)
         + [認証トークンを確認](check-authentication-token.md)
         + [認証トークンの取得](retrieve-authentication-token.md)
         + [認証を開始](initiate-authorization.md)
         + [認証トークンを取得](retrieve-authorization-token.md)
         + [ショートメディアトークンの取得](obtain-short-media-token.md)
         + [2 画面目の Web アプリによる認証フローの確認](check-authentication-flow-by-second-screen-web-app.md)
         + [事前に認証されたリソースのリストを取得](retrieve-list-of-preauthorized-resources.md)
         + [2 画面目の Web アプリで事前に認証されたリソースのリストを取得](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [ログアウトを開始](initiate-logout.md)
         + [ユーザーメタデータ](user-metadata.md)
         + [プロファイルリクエストの取得](retrieve-profilerequest.md)
         + [トークン交換](token-exchange.md)
         + [一時パスとプロモーション一時パスのフリープレビュー](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK の概要](javascript-sdk-overview.md)
         + [JavaScript SDK クックブック](javascript-sdk-cookbook.md)
         + [JavaScript SDK API リファレンス](javascript-sdk-api-reference.md)
         + ガイドライン {#js-sdk-guidelines}
            + [更新なしのログインとログアウト](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [事前認証](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK の概要](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK クックブック](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API リファレンス](iostvos-sdk-api-reference.md)
         + ガイドライン {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS のアプリ登録](iostvos-application-registration.md)
            + 移行のガイドライン {#migration-guidelines}
               + [iOS/tvOS v3.x 移行ガイド](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS のストレージの整合性チェック](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [事前認証](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK の概要](android-sdk-overview.md)
         + [Android SDK クックブック](android-sdk-cookbook.md)
         + [Android SDK API リファレンス](android-sdk-api-reference.md)
         + ガイドライン {#androidguidelines}
            + [Android アプリケーションの登録](android-application-registration.md)
            + [動的クライアント登録を使用する Android SDK](android-sdk-with-dynamic-client-registration.md)
         + Android API{#androidapi}
            + [事前認証](preauthorize-android.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO — プログラマーキックオフガイド](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [クライアントレス API クックブックを使用したAmazon FireOS SSO](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS 技術概要](amazon-fireos-technical-overview.md)
         + [Amazon FireOS 統合クックブック](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API リファレンス](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS アプリケーションの登録](amazon-fireos-application-registration.md)
         + [動的クライアント登録を使用する FireOS SDK](fireos-sdk-with-dynamic-client-registration.md)
   + Platform SSO {#platform-sso}
      + Apple SSO {#apple-sso}
         + [Apple SSO の概要](apple-sso-overview.md)
         + [Apple SSO クックブック (REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO クックブック (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + コンテンツメタデータ {#content-metadata}
      + [保護されたリソースの識別](identify-protected-resources.md)
   + コンテンツサーバーの統合 {#content-serv-int}
      + [メディアトークン検証ツールの統合方法](media-token-verifier-int.md)
   + 付録 {#appendices}
      + [デバッグのヒント](appendix-b-debugging-tips.md)
+ MVPD 統合ガイド {#mvpd-int-guide}
   + [統合の機能](mvpd-integr-features.md)
   + [認証](authn-usecase.md)
   + [OAuth 2.0 プロトコルを使用した認証](authn-oauth2-protocol.md)
   + [認証](authz-usecase.md)
   + [プリフライト認証](mvpd-preflight-authz.md)
   + [MVPD ログアウト](usecase-mvpd-logout.md)
   + [コンテンツメタデータ交換](mvpd-content-metadata-exchange.md)
   + [ユーザーメタデータ交換](mvpd-user-metadata-exchng.md)
   + [プロキシ MVPD Web サービス](proxy-mvpd-webserv.md)
   + [プロキシ MVPD SAML 統合](proxy-mvpd-saml-int.md)
   + [サービスプロバイダー範囲](serv-provider-scoping.md)
   + [MVPD 許可 IP アドレス](mvpd-listing-ip-addres.md)
+ Adobe Pass認証機能 {#auth-features}
   + Adobe Analytics統合 {#analytics-int}
      + [Adobe Pass Authentication Server 側のデータのAdobe Analyticsへの統合](integrate-authn-servr-data-analytics.md)
      + [Adobe Pass認証でのExperience CloudID の使用](exp-cloud-id-authn.md)
   + エンタイトルメントサービスの監視 {#entitlement-service-monitoring}
      + [エンタイトルメントサービスの監視の概要](entitlement-service-monitoring-overview.md)
      + [エンタイトルメントサービス監視 API](entitlement-service-monitoring-api.md)
      + [サーバーサイド指標](understanding-serverside-metrics.md)
   + 一時パス {#temp-pass}
      + [一時パス](temp-pass.md)
      + [プロモーション一時パス](promotional-temp-pass.md)
      + [一時パスをリセット](reset-temp-pass.md)
   + シングルサインオン {#sso}
      + [シングルサインオンのサポート](sso-support.md)
      + [パッシブ認証による SSO](sso-passive-authn.md)
   + ホームベースの認証 {#home-based-auth}
      + [TV Everywhere のホームベースの認証](home-based-authn-tve.md)
      + [MVPD の HBA ステータス](hba-status-mvpds.md)
   + ユーザーメタデータ {#user-metadat}
      + [ユーザーメタデータ](user-metadata-feature.md)
   + [プリフライト認証](preflight-authz.md)
   + エラーレポート {#error-reportn}
      + [エラーレポート](error-reporting.md)
      + [エラーコードの強化](enhanced-error-codes.md)
   + クライアントの登録 {#client-regn}
      + [動的クライアントの登録](dynamic-client-registration.md)
      + [動的クライアント登録 API](dynamic-client-registration-api.md)
      + [Dynamic Client Registration Management](dynamic-client-registration-management.md)
   + 劣化サービス {#degrn-service}
      + [劣化 API の概要](degradation-api-overview.md)
   + プライバシー対応 {#privacy-readiness}
      + [プライバシーサポートの概要](privacy-supp-overview.md)
      + [プライバシーリクエストの作成方法](make-privacy-req.md)
+ ヒントとトラブルシューティング {#tips-troubleshoot}
   + [選択ダイアログで MVPD を許可](allow-mvpd-selectn-dialog.md)
   + [MVPD が選択ダイアログに表示されないようにする](prevent-mvpd-selectn-dialog.md)
+ サポート {#support}
   + [エスカレーション手順](escalation-procedures.md)
   + [Adobe PassAdobePayTV パスの監視](monitoring-adobe-pay-tv-pass.md)
   + [最小必要システム構成](minimum-system-requirements.md)
+ リリースノート {#release-notes}
   + [Adobe Pass Authentication 2.68 リリースノート](auth-rn-268.md)
   + [Adobe Pass Authentication 2.67 リリースノート](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66 リリースノート](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1リリースノート](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65 リリースノート](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1リリースノート](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64 リリースノート](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63 リリースノート](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1リリースノート](auth-rn-2621.md)
   + iOS/tvOS SDK リリースノート  {#release-notes-ios}
      + [Adobe Pass Authentication iOS/tvOS 3.8.2 リリースノート](authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS/tvOS 3.8.1 リリースノート](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS/tvOS 3.7.0 リリースノート](authn-rn-ios-tvos-370.md)
   + Android SDK リリースノート {#release-notes-android}
      + [Adobe Pass Authentication Android 3.7.3 リリースノート](authn-rn-android-373.md)
+ テクニカルノート {#tech-notes}
   + Adobe Pass認証 SDK {#primetime-authentication-sdks}
      + [証明書 Q&amp;A](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [Safari ブラウザーに対する JS SDK の制限](js-sdk-limitations-for-safari-browser.md)
         + [Cookie の更新 — SameSite および Secure フラグ](cookies-updates--samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10 アプリでの Enabler Android SDK シングルサインオン (SSO) へのアクセス](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass認証と Android 6 の「Marshmallow」新しい権限モデル](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [iOS SDK 3.1 以降での WKWebView のサポート](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2 以降での SFSafariViewController のサポート](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [Adobe Pass Authentication Access Enabler を使用する場合のiOSでの SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS認証エラー — adobepass.ios.app が見つかりません](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [iOSで一時パスをリセット](reset-temp-pass-on-ios.md)
         + [コンソールアプリログを使用した AccessEnabler iOS/tvOS SDK のデバッグ](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 のアップグレードパス](accessenabler-iostvos-370-upgrade-path.md)
   + 認証環境を渡す {#primetime-authentication-environments}
      + [Adobe環境について](understanding-the-adobe-environments.md)
      + [事前に実行する環境とテストの設定](setting-up-your-environment-and-testing-in-prequal.md)
      + [認証 API テストサイトを使用した認証フローと認証フローのAdobe方法](test-authn-authz-flows-using-adobes-api-test-site.md)
   + クライアントレス API {#clientless-api}
      + [クライアントレス API 実装 — エラーコード/推定理由/原因を含むメッセージ](clientless-api-implementation-error-codes--messages-with-probable-reason--cause.md)
      + [デバイス ID がない場合のクライアントレス API フロー](clientless-api-flow-in-the-absence-of-device-id.md)
      + [クライアントレス： /authenticate Request での&#39;&amp;&#39;reg_code の使用を避ける](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Xbox 360 および XboxOne クライアントレスで、プログラマー向けのAdobe Passエンタイトルメントサービスを有効化](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [クライアントレスのデバイスタイプと指標](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + ユーザーエクスペリエンス {#user-exp}
      + [MVPD ログインページを iFrame からポップアップに移行する方法](migr-mvpd-login-iframe-popup.md)
      + [プリフライト機能：問題を有効にする、トラブルシューティングする、または特定する方法](preflight-feature.md)
   + ツールとユーティリティ {#tools-and-utilities}
      + [Charles プロキシの使用](using-charles-proxy.md)
   + 概念 {#concepts}
      + [ユーザー ID について](understanding-user-ids.md)
+ [TVE ダッシュボードユーザガイド](tve-dashboard-user-guide.md)
+ [用語集](glossary.md)
