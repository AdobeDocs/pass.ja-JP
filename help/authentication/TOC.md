---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 認証
user-guide-description: Adobe Pass は、TV Everywhere の使用権限管理ソリューションです。リソースへのアクセスをリクエストするユーザーにそのリソースへの権限が付与されているかどうかを判断するためのモジュール型フレームワークを提供します。
source-git-commit: d59afc0384a1c3617143efcef4ab5fb1a323e511
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 2%

---


# Adobe Pass認証ヘルプ {#authentication}

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
   + [プログラマーの使用権限フロー](entitlement-flow.md)
   + [プログラマーのユースケース](programmer-use-cases.md)
   + [クライアント情報（デバイス、接続、アプリケーション）を渡す](passing-client-information-device-connection-and-application.md)
   + [スロットルメカニズム](throttling-mechanism.md)
   + REST API V1 {#rest-api-v1}
      + [REST API の概要](rest-api-overview.md)
      + [REST API クックブック（サーバー間）](rest-api-cookbook-servertoserver.md)
      + [REST API クックブック（クライアントからサーバー）](rest-api-cookbook-clienttoserver.md)
      + Rest API リファレンス {#rest-api-reference}
         + [REST API リファレンス](rest-api-reference.md)
         + [登録コードのリクエスト](registration-code-request.md)
         + [登記記録を返還する](return-registration-record.md)
         + [登録レコードの削除](delete-registration-record.md)
         + [MVPD リストの提供](provide-mvpd-list.md)
         + [認証の開始](initiate-authentication.md)
         + [認証トークンを確認](check-authentication-token.md)
         + [認証トークンの取得](retrieve-authentication-token.md)
         + [認証の開始](initiate-authorization.md)
         + [認証トークンの取得](retrieve-authorization-token.md)
         + [ショートメディアトークンの取得](obtain-short-media-token.md)
         + [2 番目の画面の Web アプリによる認証フローの確認](check-authentication-flow-by-second-screen-web-app.md)
         + [事前承認済みリソースのリストの取得](retrieve-list-of-preauthorized-resources.md)
         + [2 番目の画面の Web アプリによる事前認証済みリソースのリストの取得](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [ログアウトの開始](initiate-logout.md)
         + [ユーザーメタデータ](user-metadata.md)
         + [profile-request の取得](retrieve-profilerequest.md)
         + [トークン交換](token-exchange.md)
         + [Temp Pass とプロモーション Temp Pass の無料プレビュー](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + REST API V2 {#rest-api-v2}
      + API の {#rest-api-v2-apis}
         + [REST API V2 - API – 概要 ](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + 設定 {#rest-api-v2-configuration-apis}
            + [特定のサービスプロバイダーの設定の取得](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + Sessions {#rest-api-v2-sessions-apis}
            + [認証セッションの作成](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [認証セッションの再開](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [認証セッションの取得](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [ユーザーエージェントでの認証の実行](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + Profiles {#rest-api-v2-profiles-apis}
            + [プロファイルの取得](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [特定の mvpd のプロファイルの取得](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [特定のコードのプロファイルの取得](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + 決定 {#rest-api-v2-decisions-apis}
            + [特定の mvpd を使用した認証決定の取得](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [ 特定の mvpd を使用した事前認証決定の取得 ](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + ログアウト {#rest-api-v2-logout-apis}
            + [特定の mvpd に対するログアウトの開始](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + パートナーシングル サインオン {#rest-api-v2-partner-single-sign-on-apis}
            + [パートナー認証要求の取得](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [パートナー認証応答を使用したプロファイルの取得](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + フロー {#rest-api-v2-flows}
         + [REST API V2 - フロー – 概要](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + の基本的なアクセスフロ {#rest-api-v2-basic-access-flows}
            + [プライマリアプリケーション内で実行される基本プロファイルフロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [セカンダリアプリケーション内で実行される基本プロファイルフロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [プライマリアプリケーション内で実行される基本認証フロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [セカンダリ・アプリケーション内で実行される基本認証フロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [プライマリアプリケーション内で実行される基本認証フロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [プライマリアプリケーション内で実行される基本的な事前認証フロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [プライマリアプリケーション内で実行される基本的なログアウトフロー](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + アクセス フローの低下 {#rest-api-v2-degraded-access-flows}
            + [アクセスフローの低下](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + 一時的なアクセスフロー {#rest-api-v2-temporary-access-flows}
            + [ 一時的なアクセスフロー ](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + シングルサインオンアクセスフロー {#rest-api-v2-single-sign-on-access-flows}
            + [パートナーフローを使用したシングルサインオン](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [プラットフォーム ID フローを使用したシングルサインオン](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [サービストークンフローを使用したシングルサインオン](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [シングルログアウトフロー](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + 付録 {#rest-api-v2-appendix}
         + ヘッダー {#rest-api-v2-appendix-headers}
            + [ヘッダー – AD-Service-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [Header - Adobeの件名トークン](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [ヘッダー – AP デバイス識別子](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [ヘッダー – AP-Partner-Framework-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [ヘッダー – AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
            + [ヘッダー – X-Device-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
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
         + [iOS/tvOS SDK API リファレンス ](iostvos-sdk-api-reference.md)
         + ガイドライン {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS アプリケーションの登録](iostvos-application-registration.md)
            + 移行ガイドライン {#migration-guidelines}
               + [iOS/tvOS v3.x 移行ガイド](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS ストレージの整合性チェック](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [ 事前認証 ](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK の概要](android-sdk-overview.md)
         + [Android SDK クックブック](android-sdk-cookbook.md)
         + [Android SDK API リファレンス](android-sdk-api-reference.md)
         + ガイドライン {#androidguidelines}
            + [Android アプリケーションの登録](android-application-registration.md)
            + [Dynamic Client Registration を使用したAndroid SDK](android-sdk-with-dynamic-client-registration.md)
         + Android API{#androidapi}
            + [事前認証](preauthorize-android.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO - プログラマー向けキックオフガイド](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [クライアントレス API クックブックを使用したAmazon FireOS SSO](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS 技術概要](amazon-fireos-technical-overview.md)
         + [Amazon FireOS 統合クックブック](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API リファレンス](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS アプリケーションの登録](amazon-fireos-application-registration.md)
         + [FireOS SDK と Dynamic Client Registration](fireos-sdk-with-dynamic-client-registration.md)
   + Platform SSO {#platform-sso}
      + Apple SSO {#apple-sso}
         + [Apple SSO の概要](apple-sso-overview.md)
         + [Apple SSO クックブック（REST API）](apple-sso-cookbook-rest-api.md)
         + [Apple SSO クックブック（iOS/tvOS SDK）](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + コンテンツメタデータ {#content-metadata}
      + [保護されたリソースの識別](identify-protected-resources.md)
   + Content server integration {#content-serv-int}
      + [メディアトークンベリファイアの統合方法](media-token-verifier-int.md)
   + 付録 {#appendices}
      + [デバッグのヒント](appendix-b-debugging-tips.md)
+ MVPD 統合ガイド {#mvpd-int-guide}
   + [統合機能](mvpd-integr-features.md)
   + [認証](authn-usecase.md)
   + [OAuth 2.0 プロトコルを使用した認証](authn-oauth2-protocol.md)
   + [認証](authz-usecase.md)
   + [プリフライト認証](mvpd-preflight-authz.md)
   + [MVPD ログアウト](usecase-mvpd-logout.md)
   + [コンテンツメタデータの交換](mvpd-content-metadata-exchange.md)
   + [ユーザーメタデータの交換](mvpd-user-metadata-exchng.md)
   + [プロキシ MVPD Web サービス](proxy-mvpd-webserv.md)
   + [プロキシ MVPD SAML 統合](proxy-mvpd-saml-int.md)
   + [サービスプロバイダーの範囲](serv-provider-scoping.md)
   + [MVPD 許可 IP アドレス](mvpd-listing-ip-addres.md)
+ Adobe Pass認証機能 {#auth-features}
   + Adobe Analytics統合 {#analytics-int}
      + [Adobe Pass Authentication サーバーサイドのデータのAdobe Analyticsへの統合](integrate-authn-servr-data-analytics.md)
      + [Adobe Pass認証でのExperience CloudID の使用](exp-cloud-id-authn.md)
   + 使用権限サービスの監視 {#entitlement-service-monitoring}
      + [使用権限サービスの監視の概要](entitlement-service-monitoring-overview.md)
      + [使用権限サービスモニタリング API](entitlement-service-monitoring-api.md)
      + [サーバーサイド指標](understanding-serverside-metrics.md)
   + 一時パス {#temp-pass}
      + [一時パス](temp-pass.md)
      + [プロモーションの一時パス](promotional-temp-pass.md)
      + [ 一時パスのリセット ](reset-temp-pass.md)
   + シングルサインオン {#sso}
      + [シングルサインオンサポート](sso-support.md)
      + [受動認証を介した SSO](sso-passive-authn.md)
   + ホームベースの認証 {#home-based-auth}
      + [どこでもテレビを利用できるホームベースの認証](home-based-authn-tve.md)
      + [MVPD の HBA ステータス](hba-status-mvpds.md)
   + ユーザーメタデータ {#user-metadat}
      + [ユーザーメタデータ](user-metadata-feature.md)
   + [プリフライト認証](preflight-authz.md)
   + エラー報告 {#error-reportn}
      + [エラーレポート](error-reporting.md)
      + [拡張エラーコード](enhanced-error-codes.md)
   + クライアント登録 {#client-regn}
      + [動的なクライアント登録](dynamic-client-registration.md)
      + [Dynamic Client Registration API](dynamic-client-registration-api.md)
      + [動的なクライアント登録管理](dynamic-client-registration-management.md)
   + サービス {#degrn-service} の低下
      + [低下 API の概要](degradation-api-overview.md)
   + プライバシー対応 {#privacy-readiness}
      + [プライバシーサポートの概要](privacy-supp-overview.md)
      + [プライバシーリクエストの作成方法](make-privacy-req.md)
+ ヒントとトラブルシューティ {#tips-troubleshoot} グ
   + [選択ダイアログの MVPD を許可](allow-mvpd-selectn-dialog.md)
   + [MVPD が選択ダイアログに表示されないようにする](prevent-mvpd-selectn-dialog.md)
+ サポート {#support}
   + [エスカレーション手順](escalation-procedures.md)
   + [Adobe Pass Adobeの PayTV パスのモニタリング](monitoring-adobe-pay-tv-pass.md)
   + [必要なシステム構成](minimum-system-requirements.md)
+ リリースノート {#release-notes}
   + [Adobe Pass Authentication 2.70 リリースノート](auth-rn-270.md)
   + [Adobe Pass Authentication 2.69 リリースノート](auth-rn-269.md)
   + [Adobe Pass Authentication 2.68 リリースノート](auth-rn-268.md)
   + [Adobe Pass Authentication 2.67 リリースノート](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66 リリースノート](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1 リリースノート](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65 リリースノート](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1 リリースノート](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64 リリースノート](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63 リリースノート](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1 リリースノート](auth-rn-2621.md)
   + JavaScript SDK リリースノート {#release-notes-javascript}
      + [Adobe Pass Authentication JavaScript 4.7.0 リリースノート](authn-rn-javascript-470.md)
      + [Adobe Pass Authentication JavaScript 4.6.0 リリースノート](authn-rn-javascript-460.md)
      + [Adobe Pass Authentication JavaScript 4.4.0 リリースノート](authn-rn-javascript-440.md)
      + [Adobe Pass Authentication JavaScript 4.2.0 リリースノート](authn-rn-javascript-420.md)
      + [Adobe Pass Authentication JavaScript 4.1.1 リリースノート](authn-rn-javascript-411.md)
      + [Adobe Pass Authentication JavaScript 4.1.0 リリースノート](authn-rn-javascript-410.md)
      + [Adobe Pass Authentication JavaScript 4.0.0 リリースノート](authn-rn-javascript-400.md)
      + [Adobe Pass Authentication JavaScript 3.5.0 リリースノート](authn-rn-javascript-350.md)
   + iOS/tvOS SDK リリースノート {#release-notes-ios}
      + [Adobe Pass認証iOS/tvOS 3.9.2 リリースノート](authn-rn-ios-tvos-392.md)
      + [Adobe Pass認証iOS/tvOS 3.8.4 リリースノート](authn-rn-ios-tvos-384.md)
      + [Adobe Pass認証iOS/tvOS 3.8.3 リリースノート](authn-rn-ios-tvos-383.md)
      + [Adobe Pass認証iOS/tvOS 3.8.2 リリースノート](authn-rn-ios-tvos-382.md)
      + [Adobe Pass認証iOS/tvOS 3.8.1 リリースノート](authn-rn-ios-tvos-381.md)
      + [Adobe Pass認証iOS/tvOS 3.7.0 リリースノート](authn-rn-ios-tvos-370.md)
   + Android SDK リリースノート {#release-notes-android}
      + [Adobe Pass Authentication Android 3.7.3 リリースノート](authn-rn-android-373.md)
+ テクニカルノート {#tech-notes}
   + Adobe Pass Authentication SDKs {#primetime-authentication-sdks}
      + [証明書に関する Q&amp;A](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [トラッキング防止の評価 – Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [トラッキング防止評価 – Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Cookie の更新 – SameSite およびセキュアフラグ](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10 アプリケーションでの Enabler Android SDK Single Sign-On （SSO）](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass認証とAndroid 6 「Marshmallow」新しい権限モデル](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [iOS SDK 3.1 以降での WKWebView のサポート](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2 以降での SFSafariViewController のサポート](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [Adobe Pass認証アクセスイネーブラを使用する場合のiOS上の SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS認証エラー – adobepass.ios.app が見つかりません](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [iOSの一時パスをリセット](reset-temp-pass-on-ios.md)
         + [コンソールアプリログを使用した AccessEnabler iOS/tvOS SDK のデバッグ](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 アップグレード パス ](accessenabler-iostvos-370-upgrade-path.md)
   + Pass Authentication Environments {#primetime-authentication-environments}
      + [Adobe環境について](understanding-the-adobe-environments.md)
      + [環境の設定と事前テスト](setting-up-your-environment-and-testing-in-prequal.md)
      + [AdobeAPI テストサイトを使用して認証フローと承認フローをテストする方法](test-authn-authz-flows-using-adobes-api-test-site.md)
   + クライアントレス API {#clientless-api}
      + [クライアントレス API の実装 – エラーコード/考えられる理由/原因を含むメッセージ](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [デバイス ID がない場合のクライアントレス API フロー](clientless-api-flow-in-the-absence-of-device-id.md)
      + [クライアントレス：/authenticate リクエストで「&amp;&#39;reg_code」を使用しないでください](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Xbox 360 および XboxOne クライアントレスでのプログラマーに対するAdobe Pass使用権サービスの有効化](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [クライアントレスのデバイスタイプと指標](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + User experience {#user-exp}
      + [MVPD ログインページを iFrame からポップアップに移行する方法](migr-mvpd-login-iframe-popup.md)
      + [プリフライト機能：問題を有効化、トラブルシューティング、または特定する方法](preflight-feature.md)
   + ツールとユーティリティ {#tools-and-utilities}
      + [Charles プロキシの使用](using-charles-proxy.md)
   + 概念 {#concepts}
      + [ ユーザー ID について ](understanding-user-ids.md)
+ [TVE ダッシュボードユーザーガイド](tve-dashboard-user-guide.md)
+ 新しい TVE ダッシュボードユーザーガイド {#user-guide}
   + [TVE ダッシュボードの概要](/help/authentication/tve-dashboard-overview.md)
   + [環境](/help/authentication/tve-dashboard-environments.md)
   + [変更のレビューとプッシュ](/help/authentication/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/tve-dashboard-home.md)
   + [チャネル](/help/authentication/tve-dashboard-channels.md)
   + [プログラマー](/help/authentication/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/tve-dashboard-mvpds.md)
   + [統合](/help/authentication/tve-dashboard-integrations.md)
   + [レポート](/help/authentication/tve-dashboard-reports.md)
   + [変更ログ](/help/authentication/tve-dashboard-changes-log.md)
+ [用語集](glossary.md)

