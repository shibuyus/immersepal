# 機能要件定義 v1.1 (2025-04-22)

## 1. 概要

AI を活用した多言語対応の単語帳（フラッシュカード）Web アプリケーション。ユーザーはデッキを作成し、単語や例文を登録・学習できる。将来的にはモバイルアプリ（React Native）展開も視野に入れる。
**★ アーキテクチャ補足:** バックエンド API は UI の表示言語から独立させ、`/api/...` 形式の固定パスで提供する。

## 2. MVP (Minimum Viable Product) スコープ

### 2.1. ユーザー認証・管理 (基本)

#### 2.1.1. ユーザー登録/ログイン

- メールアドレスとパスワードによるサインアップ・ログイン機能。
- **登録時入力:** ユーザー名(必須※), Email(必須), Password(必須)。
- **技術:** Supabase Auth。`auth.users` と `public.User` の同期トリガー設定済。

#### 2.1.2. パスワード再設定

- パスワード忘れユーザー向け再設定フロー。(メール送信、新パスワード設定)
- **技術:** Supabase Auth。**実装予定**。

#### 2.1.3. パスワード変更

- ログイン中ユーザー向けパスワード変更機能 (設定画面)。
- **技術:** Supabase Auth。**実装予定**。

#### 2.1.4. アカウント削除

- ログイン中ユーザーのアカウント削除機能 (設定画面)。
- 関連データカスケード削除。
- **技術:** Service/API は**実装予定**。

### 2.2. デッキ管理 (基本 CRUD)

#### 2.2.1. デッキ作成

- 新規デッキ作成。
- **入力:** デッキ名 (必須, ユーザー毎ユニーク), 説明 (任意)。
- **実装状況:** UI, Hook, API (`POST /api/decks`), Service **実装済・動作確認済**。★ API パスを `/api/decks` に修正済 (要コード反映) ★

#### 2.2.2. デッキ一覧表示

- ログインユーザーのデッキ一覧表示。
- **表示項目:** デッキ名、説明(任意)など。
- **実装状況:** UI, Hook, API (`GET /api/decks`), Service **実装済・動作確認済**。★ API パスを `/api/decks` に修正済 (要コード反映) ★

#### 2.2.3. デッキ削除

- 自身のデッキを削除。確認ダイアログ表示。
- **実装状況:** UI (確認ダイアログ連携含む), Hook, API (`DELETE /api/decks/[deckId]`), Service **実装済・動作確認済**。★ API パスを `/api/decks/[deckId]` に修正済 (要コード反映) ★

### 2.3. 単語・カード管理 (基本 CRUD)

#### 2.3.1. カード追加

- 特定デッキ内にカード追加 (デッキ詳細ページ)。
- **入力:** 表面テキスト (必須), 裏面テキスト (必須)。
- **実装状況:** UI, Hook, API (`POST /api/decks/[deckId]/cards`), Service **実装済・動作確認済**。★ API パスを `/api/decks/[deckId]/cards` に修正済 (要コード反映) ★

#### 2.3.2. カード一覧表示

- 特定デッキ内のカード一覧表示 (デッキ詳細ページ)。
- **表示項目:** 表面・裏面テキストなど。
- **実装状況:** UI (`CardList`), Hook (`useCards`), API (`GET /api/decks/[deckId]/cards`), Service **実装済・動作確認済**。★ API パスを `/api/decks/[deckId]/cards` に修正済 (要コード反映) ★

#### 2.3.3. カード編集

- カードの表面・裏面テキスト編集。
- **実装状況:** バックエンド (API `PUT /api/decks/[deckId]/cards/[cardId]`, Service - Resultパターン) は**実装済**。フロントエンド (Hook `useUpdateCard`, 編集UI) は**未実装**。★ API パスを `/api/decks/[deckId]/cards/[cardId]` に修正済 (要コード反映) ★

#### 2.3.4. カード削除

- デッキからカードを削除。確認ダイアログ表示。
- **実装状況:** バックエンド (API `DELETE /api/decks/[deckId]/cards/[cardId]`, Service)、フック (`useDeleteCard`)、UI **実装済・動作確認済**。★ API パスを `/api/decks/[deckId]/cards/[cardId]` に修正済 (要コード反映) ★

### 2.4. AI 連携 (基本)

- カード追加/更新時にテキストから自動生成・保存機能を**将来実装予定**。
  - 自動音声生成 (GCP TTS)
  - 自動解説生成 (GCP Vertex AI Gemini)
  - 自動翻訳生成 (GCP Vertex AI Gemini)
- 保存先は `Card` モデルの関連フィールド。

### 2.5. 学習・復習機能 (基本)

- 以下の機能を**将来実装予定**。
  - フラッシュカード表示 (表裏反転、音声再生)
  - 評価ボタン
  - SRSアルゴリズム (`lib/srs.ts`) による更新
  - 復習セッション (対象カード抽出・表示)

### 2.6. ユーザーサポート (最低限)

- 問い合わせフォーム設置は **MVP 後の検討事項**。

## 3. 将来的な拡張機能 (優先度 低 / 参考)

- ユーザープロファイル機能の拡充
- データインポート/エクスポート機能
- 検索機能
- 学習統計・可視化機能
- カード入力のリッチテキスト対応
- 通知機能
- デッキ共有機能

---
