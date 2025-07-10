# AIドキュメント生成プロンプト集（モバイルアプリ開発チーム向け）

## ドキュメント初期作成プロンプト

以下のプロンプトをそのままAIにコピー＆ペーストして使用してください。

---

### 📋 初期ドキュメント生成プロンプト（モバイルアプリ版）

```
あなたは優秀なテクニカルライターです。このモバイルアプリプロジェクトの完全な技術ドキュメントを作成してください。

【タスク】
バックエンドAPI、iOSアプリ、Androidアプリ、管理画面を徹底的に分析し、以下の6つのドキュメントを生成してください。時間とトークンは気にせず、可能な限り詳細に分析してください。

【生成するドキュメント】

1. **docs/API_REFERENCE.md**
   以下の形式で全エンドポイントを網羅的に記載：
   ```
   ## [機能名]
   
   ### POST /api/v1/users/register
   ユーザーを新規登録する
   
   **リクエスト**
   - Headers: 
     - `Content-Type: application/json`
     - `X-Platform: ios|android` (必須)
     - `X-App-Version: 1.0.0` (必須)
   - Body:
     ```json
     {
       "email": "user@example.com",
       "password": "securepassword",
       "name": "山田太郎",
       "device_token": "fcm_or_apns_token"
     }
     ```
   
   **レスポンス**
   - 200 OK:
     ```json
     {
       "user": {
         "id": "123",
         "email": "user@example.com",
         "name": "山田太郎"
       },
       "access_token": "eyJ...",
       "refresh_token": "eyJ..."
     }
     ```
   - 400 Bad Request: バリデーションエラー
     ```json
     {
       "error_code": "INVALID_EMAIL",
       "message": "メールアドレスの形式が正しくありません"
     }
     ```
   - 409 Conflict: メールアドレス重複
   
   **実装場所**: 
   - Backend: `src/controllers/AuthController.java#L45`
   - iOS: `iOS/App/Services/AuthService.swift#register()`
   - Android: `app/src/main/java/com/app/auth/AuthRepository.kt#register()`
   
   **関連DB**: users, user_devices テーブル
   **認証**: 不要（新規登録のため）
   **プラットフォーム別の違い**: 
   - iOS: device_tokenはAPNsトークン
   - Android: device_tokenはFCMトークン
   ```

2. **docs/DATABASE_SCHEMA.md**
   モバイルアプリ特有の情報を含む形式：
   ```
   ## users テーブル
   
   ユーザーアカウント情報を格納
   
   | カラム名 | 型 | 制約 | 説明 | デフォルト |
   |---------|-----|------|------|-----------|
   | id | UUID | PRIMARY KEY | ユーザーID | gen_random_uuid() |
   | email | VARCHAR(255) | UNIQUE, NOT NULL | メールアドレス | - |
   | name | VARCHAR(100) | NOT NULL | 表示名 | - |
   | password_hash | VARCHAR(255) | NOT NULL | BCryptハッシュ | - |
   | last_login_at | TIMESTAMP | NULL | 最終ログイン日時 | - |
   | last_login_platform | VARCHAR(20) | NULL | 最終ログインOS (ios/android/web) | - |
   | created_at | TIMESTAMP | NOT NULL | 作成日時 | CURRENT_TIMESTAMP |
   
   ## user_devices テーブル
   
   ユーザーのデバイス情報（プッシュ通知用）
   
   | カラム名 | 型 | 制約 | 説明 | デフォルト |
   |---------|-----|------|------|-----------|
   | id | UUID | PRIMARY KEY | デバイスID | gen_random_uuid() |
   | user_id | UUID | NOT NULL, FK | ユーザーID | - |
   | platform | VARCHAR(20) | NOT NULL | ios/android | - |
   | device_token | VARCHAR(500) | NOT NULL | FCM/APNsトークン | - |
   | app_version | VARCHAR(20) | NOT NULL | アプリバージョン | - |
   | os_version | VARCHAR(20) | NULL | OS バージョン | - |
   | device_model | VARCHAR(100) | NULL | 端末モデル | - |
   | is_active | BOOLEAN | NOT NULL | 有効フラグ | true |
   | created_at | TIMESTAMP | NOT NULL | 登録日時 | CURRENT_TIMESTAMP |
   
   **インデックス**
   - idx_user_devices_user_id: user_id
   - idx_user_devices_token: device_token (UNIQUE)
   ```

3. **docs/MOBILE_FEATURE_MATRIX.md**
   モバイルアプリの機能実装マトリックス：
   ```
   # モバイル機能実装マトリックス
   
   ## 認証機能
   
   ### ユーザー登録
   - **概要**: メールアドレスとパスワードで新規アカウント作成
   - **API**: POST /api/v1/users/register
   - **Backend実装**:
     - Controller: `src/controllers/AuthController.java#register()`
     - Service: `src/services/AuthService.java#createUser()`
     - Validation: `src/validators/AuthValidator.java`
   - **iOS実装**:
     - View: `iOS/App/Views/Auth/RegisterView.swift`
     - ViewModel: `iOS/App/ViewModels/RegisterViewModel.swift`  
     - Service: `iOS/App/Services/AuthService.swift`
     - Model: `iOS/App/Models/User.swift`
   - **Android実装**:
     - Activity: `app/.../auth/RegisterActivity.kt`
     - ViewModel: `app/.../auth/RegisterViewModel.kt`
     - Repository: `app/.../auth/AuthRepository.kt`
     - Model: `app/.../models/User.kt`
   - **管理画面**:
     - Page: `admin/pages/users/index.tsx`
     - Component: `admin/components/UserList.tsx`
   - **DB操作**:
     - INSERT INTO users
     - INSERT INTO user_devices
   - **プッシュ通知**: 
     - ウェルカム通知を送信
   - **アナリティクス**:
     - Event: "user_registered"
     - Parameters: platform, app_version
   ```

4. **docs/UI_TEXT_CATALOG.md**
   アプリ内の表示文言カタログ：
   ```
   # UI文言カタログ
   
   ## 認証画面
   
   ### ログイン画面
   | キー | 日本語 | 英語 | 実装場所 |
   |-----|--------|------|----------|
   | login.title | ログイン | Login | iOS: Localizable.strings<br>Android: strings.xml |
   | login.email_placeholder | メールアドレス | Email | 同上 |
   | login.password_placeholder | パスワード | Password | 同上 |
   | login.button | ログイン | Login | 同上 |
   | login.forgot_password | パスワードを忘れた方 | Forgot Password? | 同上 |
   | login.no_account | アカウントをお持ちでない方 | Don't have an account? | 同上 |
   
   ### エラーメッセージ
   | キー | 日本語 | 英語 | 使用場面 |
   |-----|--------|------|----------|
   | error.network | ネットワークエラーが発生しました | Network error occurred | API通信失敗時 |
   | error.invalid_credentials | メールアドレスまたはパスワードが正しくありません | Invalid email or password | ログイン失敗時 |
   | error.email_taken | このメールアドレスは既に使用されています | This email is already taken | 登録時の重複エラー |
   | error.weak_password | パスワードは8文字以上で入力してください | Password must be at least 8 characters | バリデーションエラー |
   
   ## プッシュ通知
   
   ### 通知タイトル・本文
   | 通知タイプ | タイトル | 本文 | 送信タイミング |
   |-----------|---------|------|---------------|
   | welcome | ようこそ！ | アプリへの登録ありがとうございます | 登録完了時 |
   | daily_reminder | 今日の予定 | 本日の予定を確認しましょう | 毎朝9:00 |
   | new_message | 新着メッセージ | {sender_name}さんからメッセージが届きました | メッセージ受信時 |
   ```

5. **docs/PLATFORM_SPECIFIC_GUIDE.md**
   プラットフォーム固有の実装ガイド：
   ```
   # プラットフォーム固有実装ガイド
   
   ## iOS固有の実装
   
   ### 必要な権限（Info.plist）
   - カメラ: NSCameraUsageDescription
   - 写真: NSPhotoLibraryUsageDescription  
   - 通知: UNUserNotificationCenter
   - 位置情報: NSLocationWhenInUseUsageDescription
   
   ### 主要ライブラリ（Swift Package Manager）
   - Alamofire: 5.8.0 (ネットワーク通信)
   - SwiftyJSON: 5.0.1 (JSON解析)
   - KeychainAccess: 4.2.2 (セキュアストレージ)
   - Firebase: 10.18.0 (プッシュ通知・アナリティクス)
   
   ### ディープリンク
   - Scheme: myapp://
   - Universal Links: https://myapp.com/app/*
   
   ## Android固有の実装
   
   ### 必要な権限（AndroidManifest.xml）
   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   <uses-permission android:name="android.permission.CAMERA" />
   <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
   ```
   
   ### 主要ライブラリ（build.gradle）
   - Retrofit: 2.9.0 (ネットワーク通信)
   - OkHttp: 4.12.0 (HTTPクライアント)
   - Gson: 2.10.1 (JSON解析)
   - Hilt: 2.48 (DI)
   - Firebase: 32.7.0 (プッシュ通知・アナリティクス)
   
   ### ディープリンク
   - Scheme: myapp://
   - App Links: https://myapp.com/app/*
   
   ## 管理画面（Web）
   
   ### 技術スタック
   - Framework: Next.js 14.0
   - UI: Tailwind CSS + shadcn/ui
   - 状態管理: Zustand
   - API通信: TanStack Query
   
   ### 主要機能
   - ユーザー管理（一覧・詳細・編集）
   - プッシュ通知送信
   - アプリ設定管理
   - 利用統計ダッシュボード
   ```

6. **docs/MOBILE_ARCHITECTURE.md**
   モバイルアプリのアーキテクチャ：
   ```
   # モバイルアプリアーキテクチャ
   
   ## 全体構成図
   ```
   [iOS App]     [Android App]     [管理画面(Web)]
       |               |                  |
       +---------------+------------------+
                       |
                  [API Gateway]
                       |
                 [Backend API]
                  /    |    \
            [DB]  [Redis]  [Storage]
                       |
              [Push Notification Service]
                  /         \
            [APNs]         [FCM]
   ```
   
   ## iOS アーキテクチャ（MVVM + Clean Architecture）
   
   ### レイヤー構成
   - **Presentation Layer**
     - Views (SwiftUI)
     - ViewModels
   - **Domain Layer**
     - UseCases
     - Models
   - **Data Layer**
     - Repositories
     - DataSources (API, LocalDB)
   
   ## Android アーキテクチャ（MVVM + Repository Pattern）
   
   ### レイヤー構成
   - **UI Layer**
     - Activities/Fragments
     - ViewModels
     - Compose UI
   - **Domain Layer**
     - UseCases
     - Models
   - **Data Layer**
     - Repositories
     - Remote/Local DataSources
   
   ## 共通設計方針
   
   ### API通信
   - 認証: Bearer Token (JWT)
   - リフレッシュトークン: 自動更新
   - タイムアウト: 30秒
   - リトライ: 3回（指数バックオフ）
   
   ### オフライン対応
   - 重要データはローカルDBにキャッシュ
   - オフライン時は Queue に保存
   - オンライン復帰時に自動同期
   
   ### エラーハンドリング
   - ネットワークエラー: 自動リトライ + ユーザー通知
   - 認証エラー: ログイン画面へ遷移
   - サーバーエラー: エラー画面表示
   ```

【分析方法】
1. **バックエンドコードのスキャン**
   - API ルーティング（Spring Boot, Express, FastAPI等）
   - リクエスト/レスポンスDTO
   - バリデーションルール
   - エラーコード定義

2. **iOSアプリのスキャン**
   - Swift/SwiftUIファイル
   - Storyboard/XIBファイル
   - Localizable.strings
   - Info.plist設定
   - API通信層の実装

3. **Androidアプリのスキャン**
   - Kotlin/Javaファイル
   - レイアウトXML
   - strings.xml（全言語）
   - AndroidManifest.xml
   - Retrofit/OkHttp実装

4. **管理画面のスキャン**
   - React/Vue/Angularコンポーネント
   - API呼び出し箇所
   - 管理機能の実装

5. **プラットフォーム共通/固有の処理**
   - 共通ロジックの特定
   - OS固有の実装箇所
   - プッシュ通知の実装差異

【出力形式】
- 各ドキュメントは独立したMarkdownファイルとして出力
- プラットフォーム別の情報は明確に区別
- 実装ファイルパスは正確に記載
- 多言語対応箇所は全言語分記載

【注意事項】
- iOS/Android で異なる実装は両方記載
- 文言は実際のリソースファイルから抽出
- APIバージョニング（v1, v2等）に注意
- 非推奨APIは[Deprecated]とマーク
- セキュリティトークン等は除外

このモバイルアプリプロジェクトのソースコードを徹底的に分析し、上記6つのドキュメントを完全な形で生成してください。
```

---

## ドキュメント更新プロンプト

### 📝 定期更新プロンプト（モバイルアプリ版）

```
あなたは優秀なテクニカルライターです。モバイルアプリプロジェクトの既存ドキュメントを最新のコードに合わせて更新してください。

【タスク】
バックエンド、iOS、Android、管理画面の最近の変更を検出し、影響を受けるドキュメントを更新してください。

【更新対象】
docs/ディレクトリ内の以下のファイル：
- API_REFERENCE.md
- DATABASE_SCHEMA.md
- MOBILE_FEATURE_MATRIX.md
- UI_TEXT_CATALOG.md
- PLATFORM_SPECIFIC_GUIDE.md
- MOBILE_ARCHITECTURE.md

【更新手順】

1. **変更検出（プラットフォーム別）**
   
   **Backend**
   ```bash
   # APIコントローラーの変更
   git diff HEAD~10 -- "*Controller*" "*Routes*" "*Api*"
   ```
   
   **iOS**
   ```bash
   # Swift ファイルとリソースの変更
   git diff HEAD~10 -- "*.swift" "*.strings" "Info.plist"
   ```
   
   **Android**  
   ```bash
   # Kotlin/Java ファイルとリソースの変更
   git diff HEAD~10 -- "*.kt" "*.java" "strings.xml" "AndroidManifest.xml"
   ```

2. **影響範囲の特定**
   - 新規API追加・変更・削除
   - DB スキーマ変更
   - 新画面・機能追加
   - UI文言の追加・変更
   - 権限追加
   - ライブラリ更新

3. **ドキュメント更新内容**
   
   **API_REFERENCE.md**
   - 新規エンドポイントを追加（iOS/Android両方の実装箇所も）
   - レスポンス形式の変更を反映
   - プラットフォーム固有のヘッダー要件を更新
   
   **UI_TEXT_CATALOG.md**
   - 新規追加された文言（Localizable.strings, strings.xml）
   - 変更された文言を更新
   - 削除された文言に[廃止]マーク
   - 多言語対応状況を確認
   
   **MOBILE_FEATURE_MATRIX.md**
   - 新機能の実装状況（iOS/Android/Backend）
   - 未実装プラットフォームを[未実装]と明記
   - プラットフォーム間の実装差異を記載

4. **プラットフォーム別の注意点**
   
   **iOS更新時**
   - SwiftUIとUIKitの混在に注意
   - iOS バージョン要件の変更
   - 新規追加されたCapability
   
   **Android更新時**
   - minSdkVersion/targetSdkVersion の変更
   - Jetpack ComposeとXMLレイアウトの混在
   - ProGuard/R8ルールの追加

5. **変更履歴の記録**
   各ドキュメントの末尾に追加：
   ```markdown
   ## 更新履歴
   - 2024-01-15: [iOS/Android] プッシュ通知許可画面を追加
   - 2024-01-15: [API] POST /api/v1/notifications/register エンドポイント追加
   - 2024-01-15: [Android] minSdkVersion を 24 に更新
   - 2024-01-15: [iOS] iOS 15.0 以上を必須に変更
   ```

【確認事項】
- [ ] iOS/Android 両方の実装を確認したか
- [ ] 文言の多言語対応は完了しているか
- [ ] API のプラットフォーム別パラメータは正しいか
- [ ] 新規ライブラリのライセンスは問題ないか
- [ ] 権限追加の説明文は適切か

プラットフォーム間の整合性を保ちながら、効率的にドキュメントを更新してください。
```

---

### 🌐 UI文言の一括更新プロンプト

```
アプリ内のUI文言を網羅的に更新してください。

【タスク】
iOS の Localizable.strings と Android の strings.xml から、すべての文言を抽出し、UI_TEXT_CATALOG.md を完全に更新してください。

【抽出対象】
1. **iOS**
   - Base.lproj/Localizable.strings
   - ja.lproj/Localizable.strings
   - en.lproj/Localizable.strings
   - SwiftUI Text() 内のハードコード文言

2. **Android**
   - values/strings.xml
   - values-ja/strings.xml
   - values-en/strings.xml
   - Kotlinコード内のハードコード文言

3. **バックエンド**
   - エラーメッセージ定義
   - プッシュ通知テンプレート
   - メールテンプレート

【出力形式】
画面/機能ごとにグループ化し、以下の情報を含める：
- キー（リソースID）
- 各言語の文言
- 使用箇所（画面名、ファイル名）
- 文字数制限がある場合は明記
- プラットフォーム差異

【特別な考慮事項】
- 改行コード（\n）の扱い
- 書式指定子（%@, %d, %s等）
- HTML タグを含む文言
- 絵文字を含む文言
- プラットフォーム固有の文言

すべての文言を漏れなく文書化してください。
```

---

### 📱 新機能追加時の包括的更新プロンプト

```
新機能が追加されました。関連するすべてのドキュメントを包括的に更新してください。

【新機能】
[機能名: 例）ソーシャルログイン機能]

【実装内容】
- Backend: OAuth2.0 認証フロー実装
- iOS: Sign in with Apple 実装
- Android: Google Sign-In 実装

【更新すべきドキュメント】
1. API_REFERENCE.md
   - OAuth認証エンドポイント追加
   - 既存の認証フローとの統合

2. DATABASE_SCHEMA.md
   - social_accounts テーブル追加
   - users テーブルの変更

3. MOBILE_FEATURE_MATRIX.md
   - ソーシャルログイン機能の実装詳細
   - プラットフォーム別の実装差異

4. UI_TEXT_CATALOG.md
   - ソーシャルログイン関連の文言
   - エラーメッセージ

5. PLATFORM_SPECIFIC_GUIDE.md
   - iOS: Capability追加、URL Scheme設定
   - Android: SHA-1 fingerprint設定

6. MOBILE_ARCHITECTURE.md
   - 認証フローの更新
   - 外部サービス連携

プラットフォームごとの実装詳細と、既存機能への影響を明確に記載してください。
```

---

## 使い方

1. **初回作成時**: 「初期ドキュメント生成プロンプト（モバイルアプリ版）」をコピーしてAIに貼り付け
2. **定期更新時**: 「定期更新プロンプト（モバイルアプリ版）」を週次/月次で実行
3. **文言更新時**: 「UI文言の一括更新プロンプト」でローカライズリソースを同期
4. **新機能追加時**: 「新機能追加時の包括的更新プロンプト」で全ドキュメント更新

これらのプロンプトは、モバイルアプリ開発特有の要件（マルチプラットフォーム対応、UI文言管理、プッシュ通知等）を考慮して設計されています。