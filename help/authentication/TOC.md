---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 認証
user-guide-description: Adobe Pass は、TV Everywhere の使用権限管理ソリューションです。リソースへのアクセスをリクエストするユーザーにそのリソースへの権限が付与されているかどうかを判断するためのモジュール型フレームワークを提供します。
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1154'
ht-degree: 3%

---


# Adobe Pass認証ヘルプ {#authentication}

+ [Adobe Pass 認証](home.md)
+ Kickstart {#kickstart}
   + [テクニカルペーパー](kickstart/technical-paper.md)
   + [プログラマーの概要](kickstart/programmer-overview.md)
   + [MVPD の概要](kickstart/mvpd-overview.md)
   + [プログラマー向けキックスタートガイド](kickstart/programmer-kickstart-guide.md)
   + [MVPD キックスタートガイド](kickstart/mvpd-kickstart-guide.md)
   + [エスカレーション手順](notes-technical/escalation-procedures.md)
   + [用語集](kickstart/glossary.md)
+ プログラマ向け統合ガイド {#integration-guide-programmers}
   + REST API {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [動的なクライアント登録の概要](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + API の {#rest-api-dcr-apis}
            + [クライアント資格情報の取得](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [アクセストークンの取得](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + フロー {#rest-api-dcr-flows}
            + [動的なクライアント登録フロー](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [REST API V2 の概要](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [REST API V2 の用語集](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [REST API V2 の FAQ](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API の {#rest-api-v2-apis}
            + [REST API V2 API の概要](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + 設定 {#rest-api-v2-configuration-apis}
               + [特定のサービスプロバイダーの設定の取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessions {#rest-api-v2-sessions-apis}
               + [認証セッションの作成](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [認証セッションの再開](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [認証セッションの取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [ユーザーエージェントでの認証の実行](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Profiles {#rest-api-v2-profiles-apis}
               + [プロファイルの取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [特定の mvpd のプロファイルの取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [特定のコードのプロファイルの取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + 決定 {#rest-api-v2-decisions-apis}
               + [特定の mvpd を使用した認証決定の取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [特定の mvpd を使用した事前認証決定の取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + ログアウト {#rest-api-v2-logout-apis}
               + [特定の mvpd に対するログアウトの開始](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + パートナーシングル サインオン {#rest-api-v2-partner-single-sign-on-apis}
               + [パートナー認証要求の取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [パートナー認証応答を使用したプロファイルの取得](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + フロー {#rest-api-v2-flows}
            + [REST API V2 フローの概要](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + の基本的なアクセスフロ {#rest-api-v2-basic-access-flows}
               + [プライマリアプリケーション内で実行される基本プロファイルフロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [セカンダリアプリケーション内で実行される基本プロファイルフロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [プライマリアプリケーション内で実行される基本認証フロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [セカンダリ・アプリケーション内で実行される基本認証フロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [プライマリアプリケーション内で実行される基本認証フロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [プライマリアプリケーション内で実行される基本的な事前認証フロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [プライマリアプリケーション内で実行される基本的なログアウトフロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + アクセス フローの低下 {#rest-api-v2-degraded-access-flows}
               + [アクセスフローの低下](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + 一時的なアクセスフロー {#rest-api-v2-temporary-access-flows}
               + [一時的なアクセスフロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + シングルサインオンアクセスフロー {#rest-api-v2-single-sign-on-access-flows}
               + [パートナーフローを使用したシングルサインオン](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [プラットフォーム ID フローを使用したシングルサインオン](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [サービストークンフローを使用したシングルサインオン](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [シングルログアウトフロー](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + クックブックス {#rest-api-v2-cookbooks}
            + [REST API V2 クックブック（クライアントからサーバー）](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2 クックブック（サーバー間）](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + 付録 {#rest-api-v2-appendix}
            + ヘッダー {#rest-api-v2-appendix-headers}
               + [ヘッダー – 認証](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [ヘッダー – AP デバイス識別子](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [ヘッダー – X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [ヘッダー – AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Header - Adobeの件名トークン](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [ヘッダー – AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [ヘッダー – AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + 標準機能 {#standard-features}
      + 使用権限 {#entitlements}
         + [保護されたリソースの識別](integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)
         + [プリフライト認証](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [メディアトークンベリファイアの統合方法](integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)
         + [ユーザーメタデータ](integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)
         + [暗号化用のユーザーメタデータ証明書](integration-guide-programmers/features-standard/entitlements/user-metadata-certificate.md)
      + エラー報告 {#error-reporting}
         + [拡張エラーコード](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
         + [エラーレポート](integration-guide-programmers/features-standard/error-reporting/error-reporting.md)
      + シングル サインオン アクセス {#sso-access}
         + パートナーシングル サインオン {#partner-sso}
            + Appleのシングルサインオン {#apple-sso}
               + [Apple SSO の概要](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO クックブック（REST API V2）](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
               + [Apple SSO クックブック（REST API V1）](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v1.md)
               + [Apple SSO クックブック（iOS/tvOS SDK）](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-iostvos-sdk.md)
         + Platform シングルサインオン {#platform-sso}
            + Amazonのシングルサインオン {#amazon-sso}
               + [Amazon SSO クックブック（REST API V2）](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
               + [Amazon SSO クックブック（REST API V1）](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
            + Roku のシングルサインオン {#roku-sso}
               + [Roku SSO の概要](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
         + [シングルサインオンサポート](integration-guide-programmers/features-standard/sso-access/sso-support.md)
         + [受動認証を介した SSO](integration-guide-programmers/features-standard/sso-access/sso-passive-authn.md)
      + ホーム ベースの認証アクセス {#hba-access}
         + [どこでもテレビを利用できるホームベースの認証](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [MVPD の HBA ステータス](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + プライバシーサポート {#privacy-support}
         + [プライバシーサポートの概要](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [プライバシーリクエストの作成方法](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Premium の機能 {#features-premium}
      + 一時アクセス {#temporary-access}
         + [一時パス](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [プロモーションの一時パス](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [一時パスをリセット](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + アクセス {#degraded-access} が低下しました
         + [Degradation API の概要 ](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [使用権限サービスの監視の概要](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [使用権限サービスモニタリング API](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [サーバーサイド指標](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Adobe Pass Authentication サーバーサイドのデータのAdobe Analyticsへの統合](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Adobe Pass認証でのExperience CloudID の使用](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + レガシ {#legacy}
      + （レガシー） REST API V1 {#rest-api-v1}
         + [REST API V1 の概要](integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
         + [REST API V1 リファレンス](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + （レガシー） API {#rest-api-v1-apis}
            + [登録コードのリクエスト](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [登記記録を返還する](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [登録レコードの削除](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [MVPD リストの提供](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [認証の開始](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [認証トークンを確認](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [認証トークンの取得](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [認証の開始](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [認証トークンの取得](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [ショートメディアトークンの取得](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [2 番目の画面の Web アプリによる認証フローの確認](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [事前承認済みリソースのリストの取得](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [2 番目の画面の Web アプリによる事前認証済みリソースのリストの取得](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [ログアウトの開始](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [ユーザーメタデータ](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [profile-request の取得](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [トークン交換](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [Temp Pass とプロモーション Temp Pass の無料プレビュー](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + （レガシー）クックブックス {#rest-api-v1-cookbooks}
            + [REST API V1 クックブック（クライアントからサーバー）](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [REST API V1 クックブック（サーバー間）](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + （レガシー） SDK {#sdks}
         + （従来の）JavaScript SDK {#javascript-sdk}
            + [JavaScript SDK の概要](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [JavaScript SDK クックブック](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [JavaScript SDK API リファレンス](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [JavaScript SDK API の事前認証](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + （従来）ガイドライン {#javascript-sdk-guidelines}
               + [更新なしのログインとログアウト](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + （従来の）iOS/tvOS SDK {#ios-tvos-sdk}
            + [iOS/tvOS SDK の概要](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [iOS/tvOS SDK クックブック](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [iOS/tvOS SDK API リファレンス](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [iOS/tvOS SDK API の事前認証](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + （従来）ガイドライン {#ios-tvos-sdk-guidelines}
               + [iOS/tvOS アプリケーションの登録](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [iOS/tvOS v3.x 移行ガイド](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [iOS/tvOS ストレージの整合性チェック](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + （従来の）Android SDK {#android-sdk}
            + [Android SDK の概要](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Android SDK クックブック](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [Android SDK API リファレンス](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [Android SDK API の事前認証](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + （従来）ガイドライン {#android-sdk-guidelines}
               + [Android アプリケーションの登録](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [Dynamic Client Registration を使用したAndroid SDK](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + （レガシー） FireOS SDK {#fireos-sdk}
            + [Amazon FireOS 技術概要](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Amazon FireOS 統合クックブック](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [Amazon FireOS API リファレンス](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Amazon FireOS アプリケーションの登録](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [FireOS SDK と Dynamic Client Registration](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [Amazon FireOS SSO - プログラマー向けキックオフガイド](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
   + [プログラマー統合ガイドの概要](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [スロットルメカニズム](integration-guide-programmers/throttling-mechanism.md)
   + [必要なシステム構成](integration-guide-programmers/minimum-system-requirements.md)
   + [プログラマーの使用権限フロー](integration-guide-programmers/entitlement-flow.md)
   + [プログラマーのユースケース](integration-guide-programmers/programmer-use-cases.md)
   + [クライアント情報（デバイス、接続、アプリケーション）を渡す](integration-guide-programmers/passing-client-information-device-connection-and-application.md)
+ MVPD の統合ガイド {#integration-guide-mvpds}
   + [統合機能](integration-guide-mvpds/mvpd-integr-features.md)
   + [認証](integration-guide-mvpds/authn-usecase.md)
   + [OAuth 2.0 プロトコルを使用した認証](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [認証](integration-guide-mvpds/authz-usecase.md)
   + [プリフライト認証](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD ログアウト](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [コンテンツメタデータの交換](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [ユーザーメタデータの交換](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [プロキシ MVPD Web サービス](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [プロキシ MVPD SAML 統合](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [サービスプロバイダーの範囲](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD 許可 IP アドレス](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ TVE ダッシュボード {#user-guide-tve-dashboard} のユーザーガイド
   + [TVE ダッシュボードの概要](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [環境](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [変更のレビューとプッシュ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Dashboard](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [チャネル](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [プログラマー](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPD](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [統合](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [レポート](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [変更ログ](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
   + [TVE ダッシュボードユーザーガイド](user-guide-tve-dashboard/tve-dashboard-user-guide.md)
+ リリースノート {#release-notes}
   + 2024 年 {#release-notes-2024}
      + [Adobe Pass Authentication 3.0.3 リリースノート](notes-releases/auth-rn-303.md)
      + [Adobe Pass Authentication 3.0 リリースノート](notes-releases/auth-rn-300.md)
      + [Adobe Pass Authentication 2.70 リリースノート](notes-releases/auth-rn-270.md)
      + [Adobe Pass Authentication 2.69 リリースノート](notes-releases/auth-rn-269.md)
      + [Adobe Pass Authentication JavaScript 4.7.0 リリースノート](notes-releases/authn-rn-javascript-470.md)
      + [Adobe Pass認証iOS/tvOS 3.9.2 リリースノート](notes-releases/authn-rn-ios-tvos-392.md)
      + [Adobe Pass認証iOS/tvOS 3.8.4 リリースノート](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 年 {#release-notes-2023}
      + [Adobe Pass Authentication 2.68 リリースノート](notes-releases/auth-rn-268.md)
      + [Adobe Pass Authentication 2.67 リリースノート](notes-releases/auth-rn-267.md)
      + [Adobe Pass Authentication 2.66 リリースノート](notes-releases/auth-rn-266.md)
      + [Adobe Pass Authentication 2.65.1 リリースノート](notes-releases/auth-rn-2651.md)
      + [Adobe Pass Authentication 2.65 リリースノート](notes-releases/auth-rn-265.md)
      + [Adobe Pass Authentication 2.64.1 リリースノート](notes-releases/auth-rn-2641.md)
      + [Adobe Pass認証iOS/tvOS 3.8.3 リリースノート](notes-releases/authn-rn-ios-tvos-383.md)
      + [Adobe Pass認証iOS/tvOS 3.8.2 リリースノート](notes-releases/authn-rn-ios-tvos-382.md)
      + [Adobe Pass認証iOS/tvOS 3.8.1 リリースノート](notes-releases/authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication Android 3.7.3 リリースノート](notes-releases/authn-rn-android-373.md)
   + {#release-notes-2022} 年
      + [Adobe Pass Authentication 2.64 リリースノート](notes-releases/auth-rn-264.md)
      + [Adobe Pass Authentication 2.63 リリースノート](notes-releases/auth-rn-263.md)
      + [Adobe Pass Authentication 2.62.1 リリースノート](notes-releases/auth-rn-2621.md)
      + [Adobe Pass Authentication JavaScript 4.6.0 リリースノート](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#release-notes-2021}
      + [Adobe Pass Authentication JavaScript 4.4.0 リリースノート](notes-releases/authn-rn-javascript-440.md)
      + [Adobe Pass認証iOS/tvOS 3.7.0 リリースノート](notes-releases/authn-rn-ios-tvos-370.md)
+ テクニカルノート {#tech-notes}
   + 環境 {#tech-notes-environments}
      + [Adobe環境について](notes-technical/understanding-the-adobe-environments.md)
      + [環境の設定と事前テスト](notes-technical/setting-up-your-environment-and-testing-in-prequal.md)
      + [AdobeAPI テストサイトを使用して認証フローと承認フローをテストする方法](notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + ユーザーエクスペリエンス {#tech-notes-user-experience}
      + [MVPD ログインページを iFrame からポップアップに移行する方法](notes-technical/migr-mvpd-login-iframe-popup.md)
      + [プリフライト機能：問題を有効化、トラブルシューティング、または特定する方法](notes-technical/preflight-feature.md)
      + [選択ダイアログの MVPD を許可](notes-technical/allow-mvpd-selectn-dialog.md)
      + [MVPD が選択ダイアログに表示されないようにする](notes-technical/prevent-mvpd-selectn-dialog.md)
   + REST API V1 {#tech-notes-rest-api-v1}
      + [クライアントレス API の実装 – エラーコード/考えられる理由/原因を含むメッセージ](notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [デバイス ID がない場合のクライアントレス API フロー](notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
      + [クライアントレス：/authenticate リクエストで「&amp;&#39;reg_code」を使用しないでください](notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Xbox 360 および XboxOne クライアントレスでのプログラマーに対するAdobe Pass使用権サービスの有効化](notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [クライアントレスのデバイスタイプと指標](notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + SDK {#tech-notes-sdks}
      + [証明書に関する Q&amp;A](notes-technical/certificates-qa.md)
      + [ユーザー ID について](notes-technical/understanding-user-ids.md)
      + JavaScript SDK {#tech-notes-javascript-sdk}
         + [トラッキング防止の評価 – Apple Safari](notes-technical/tracking-prevention-assessment-apple-safari.md)
         + [トラッキング防止評価 – Google Chrome](notes-technical/tracking-prevention-assessment-google-chrome.md)
         + [Cookie の更新 – SameSite およびセキュアフラグ](notes-technical/cookies-updates-samesite-and-secure-flags.md)
         + [デバッグのヒント](notes-technical/appendix-b-debugging-tips.md)
      + Android SDK {#tech-notes-android-sdk}
         + [Android 10 アプリケーションでの Enabler Android SDK Single Sign-On （SSO）](notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass認証とAndroid 6 「Marshmallow」新しい権限モデル](notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#tech-notes-ios-tvos-sdk}
         + [iOS SDK 3.1 以降での WKWebView のサポート](notes-technical/wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2 以降での SFSafariViewController のサポート](notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [Adobe Pass認証アクセスイネーブラを使用する場合のiOS上の SSO](notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS認証エラー – adobepass.ios.app が見つかりません](notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [iOSの一時パスをリセット](notes-technical/reset-temp-pass-on-ios.md)
         + [コンソールアプリログを使用した AccessEnabler iOS/tvOS SDK のデバッグ](notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0 アップグレード・パス](notes-technical/accessenabler-iostvos-370-upgrade-path.md)
   + {#tech-notes-troubleshooting} のトラブルシューティング
      + [Charles プロキシの使用](notes-technical/using-charles-proxy.md)
      + [Adobe Pass Adobeの PayTV パスのモニタリング](notes-technical/monitoring-adobe-pay-tv-pass.md)
