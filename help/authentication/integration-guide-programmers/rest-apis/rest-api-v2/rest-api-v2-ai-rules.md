---
title: REST API V2 AI ルール
description: REST API V2 AI ルール
exl-id: 7bcea820-2324-44a5-8628-9a10c1bec067
source-git-commit: 39384d753e7808fa433f30d8dafabd531dbf3acf
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%
---
# REST API V2 AI ルール {#rest-api-v2-ai-rules}

>[!IMPORTANT]
>
> このページのコンテンツは、情報提供のみを目的として提供されています。 このAPIを使用するには、Adobeの現在のライセンスが必要です。 無断使用は認められません。

このドキュメントでは、[REST API V2](rest-api-v2-overview.md)を使用したTVE （TV Everywhere） アプリケーションのAI支援による開発のために特別に設計された、構造化されたルールとベストプラクティスをAdobe Pass Authenticationのお客様に提供します。

このガイドで解説したAI開発ルールに従うことで、開発者はAI コーディングアシスタントが[必須の要件と推奨される慣行](rest-api-v2-checklist.md)に準拠した、パフォーマンスが高く、維持可能な統合を作成できるようにすることができます。

## コーディングアシスタントルール {#coding-assistants-rules}

ルールは、エージェントにシステムレベルの指示を提供します。 CDPを継続的なコンテキスト、嗜好、ワークフローとして捉えましょう。 このドキュメント内のルールは、一般的なAIを活用したコーディングアシスタントと互換性があり、[REST API V2 チェックリスト &#x200B;](rest-api-v2-checklist.md)を実用的なAI開発ガイドラインに変換します。

包括的なルールセットを使用してAI開発環境を設定し、Adobe Pass Authentication REST API V2との統合のために、インテリジェントでコンプライアンスを維持したコード生成のメリットを体験しましょう。 使用しているAI ツールに応じて、以下のルールを開発環境の設定ファイルにコピー&amp;ペーストします。

```markdown
# Adobe Pass Authentication REST API V2 Integration Rules

You are an expert developer assistant helping to implement Adobe Pass Authentication REST API V2 integrations for TVE (TV Everywhere) applications. Follow these mandatory requirements and recommended practices to ensure compliance with Adobe Pass Authentication standards.

## References

For latest API specifications, refer to the official documentation:

- Retrieve configuration for specific service provider: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider
- Create authentication session: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-sessions-apis/rest-api-v2-sessions-apis-create-authentication-session
- Resume authentication session: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session
- Retrieve authentication session: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code
- Perform authentication in user agent: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent
- Retrieve profiles: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles
- Retrieve profile for specific mvpd: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd
- Retrieve profile for specific code: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code
- Retrieve authorization decisions using specific mvpd: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd
- Retrieve preauthorization decisions using specific mvpd: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd
- Initiate logout for specific mvpd: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd
- Retrieve partner authentication request: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request
- Create and retrieve profile using partner authentication response: https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-apis/rest-api-v2-partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response

## Core Principles

- Follow Adobe Pass Authentication latest APIs specifications
- Implement proper caching strategies to minimize API calls
- Handle errors gracefully with appropriate retry mechanisms
- Always prioritize security and performance

## 1. Registration Phase Requirements

### MANDATORY - Client Credentials Management
- **MUST** use a registered application with REST API v2 scope
- **MUST** store client credentials in persistent storage and reuse for every access token request
- **MUST** cache access tokens in persistent storage until expiration
- **NEVER** request new tokens for every API call - only refresh when expired

### RECOMMENDED - Token Validation
- Proactively check access token validity before API calls
- Implement retry mechanism that refreshes access token before retrying on HTTP 401 errors

## 2. Configuration Phase Requirements

### MANDATORY - Configuration Retrieval
- **ONLY** retrieve configuration when user needs to select MVPD (TV provider)
- **SHOULD NOT** retrieve configuration when:
    - User is already authenticated
    - User has temporary access
    - User can confirm previous MVPD selection

### MANDATORY - MVPD Selection Caching
- Store in persistent storage:
    - MVPD "id"
    - MVPD "displayName"
    - MVPD "logoUrl"

### RECOMMENDED - Configuration Caching
- Cache configuration response for 2-3 minutes to improve performance

## 3. Authentication Phase Requirements

### MANDATORY - Polling Mechanism
- **1st screen authentication**: Start polling when user reaches final destination page after redirectUrl loads
- **2nd screen authentication**: Start polling immediately after receiving Sessions response and displaying authentication code
- **Frequency**: Poll every 3-5 seconds (no faster)
- **Stop polling when**:
    - Authentication succeeds (profile retrieved)
    - Session/code expires
    - New authentication code generated

### MANDATORY - Profile Caching
- Cache in persistent storage:
    - `mvpd` field for provider tracking
    - `attributes` field for user metadata and personalization
- Note: Some metadata may update during Authorization phase

### RECOMMENDED - Multiple Profiles Support
- Handle multiple authentication profiles
- Allow user selection or implement auto-selection logic (e.g., longest validity)

### RECOMMENDED - Enhanced Flows
Support when business requires:
- Degraded access flows (premium)
- Temporary access flows (premium)
- Single sign-on flows (standard)

## 4. Preauthorization Phase Requirements (Optional)

### MANDATORY - Decision Usage
- **ONLY** use preauthorization for content filtering
- **NEVER** use for playback decisions (potential contractual violation)

### MANDATORY - Retry Logic
- Handle enhanced error codes appropriately
- Use `action` field for remediation steps
- Limit retries to 2-3 attempts maximum when `action` indicates retry
- Avoid endless retry loops

### MANDATORY - Caching
- Cache successful permit decisions in memory
- Improves performance and reduces API calls

### RECOMMENDED - User Experience
- Display clear feedback for denied decisions using MVPD/Adobe error messages

## 5. Authorization Phase Requirements

### MANDATORY - Authorization Decisions
- **ALWAYS** obtain authorization before playback (regardless of preauth)
- Allow uninterrupted streaming during media token expiration
- Request fresh authorization for next playback request
- For live streams: Consider re-authorization after pausing, commercial breaks, or MRSS changes

### MANDATORY - Retry Logic
- Handle enhanced error codes with `action` field guidance
- Limit retries to 2-3 attempts when `action` indicates retry
- Avoid endless retry loops

### RECOMMENDED - Media Token Validation
- Validate tokens using Media Token Verifier library
- Prevents fraud schemes like stream ripping

### RECOMMENDED - User Experience
- Display clear feedback for denied authorization using enhanced error codes

## 6. Logout Phase Requirements

### MANDATORY - Logout Implementation
- Implement logout API for manual user sign-out
- Follow REST API v2 action specifications:
    - MVPD logout: Navigate to provided URL in user-agent
    - Apple SSO: Guide user to Apple system settings logout

### RECOMMENDED - User Experience
- **AVOID** automatic logout on preauthorization/authorization denials
- Only call logout API on direct user request

## 7. Parameters and Headers Requirements

### MANDATORY - Required Headers
- **Authorization**: Send for every REST API v2 request
- **AP-Device-Identifier**: Send for every request, must reflect actual streaming device

### MANDATORY - Device Identifier Stability
- Compute stable identifier that persists across updates/reboots
- For platforms without hardware ID, generate from app attributes and persist
- Changes cause authentication loss

### MANDATORY - API Compliance
- Send only REST API v2 expected parameters and headers
- Follow API reference documentation exactly

### RECOMMENDED - Code Reuse
- Reuse REST API v1 code for device identifier/info computation with adjustments
- Reuse DCR API calling code from v1

## 8. Error Handling Requirements

### MANDATORY - Enhanced Error Code Handling
- Handle enhanced error codes appropriately
- Use `action` field for remediation steps
- Most errors preventable with proper development practices
- Limited error codes warrant retry, most need alternative resolution

### MANDATORY - HTTP Error Handling
- Differentiate HTTP errors (400, 401, 403, 404, 405, 500) from success responses (200, 201) with error payloads
- Most HTTP errors preventable with proper handling
- Limited HTTP codes warrant retry

## 9. Testing Requirements

### MANDATORY - Environment Testing
- **MUST** test in non-production environments:
    - Prequal-Production
    - Release-Staging
- **NEVER** proceed to Release-Production without end-to-end validation
- Perform thorough QA before production launch

### RECOMMENDED - Test Coverage
Test all flows across devices/platforms:
- **Authentication**: Primary and secondary screen scenarios
- **Preauthorization**: Permit and deny scenarios
- **Authorization**: Permit and deny scenarios
- **Logout**: Complete flow testing
- **Enhanced flows**: Degraded access, temporary access, SSO
- **MVPD Coverage**: Test with top/widely-used providers

### RECOMMENDED - Test Tools
- Use Adobe Developer website for testing

## Code Quality Guidelines

### Caching Strategy

- Access tokens: persistent
- Configuration: memory, 2-3 min
- MVPD selection: persistent
- User profiles: persistent (selective fields)
- Preauthorization decisions: memory

### Error Boundaries
- Implement comprehensive error handling
- Log errors for debugging while avoiding sensitive data exposure
- Provide meaningful user feedback
- Implement circuit breaker patterns for API resilience

## Security Considerations

- Validate all media tokens using Adobe's verifier library
- Secure storage of credentials and tokens
- Proper session management
- Device identifier stability and security

## Performance Optimization

- Minimize API calls through intelligent caching
- Implement proper retry strategies with exponential backoff
- Use connection pooling for HTTP requests
- Monitor and log performance metrics

Remember: This integration affects contractual agreements between Programmers, MVPDs, and Adobe. Compliance with these rules is essential for successful production deployment.
```
