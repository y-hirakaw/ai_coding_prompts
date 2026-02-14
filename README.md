# ai_coding_prompts
個人的なAI開発の際に利用するプロンプト集です。

### spec-to-technical-design-prompt.md

* ビジネス要求（Googleドキュメント等）を技術設計書に変換するプロンプト
* DDD、非機能要件、セキュリティ、運用設計を網羅した13セクション構成
* Gemini / NotebookLM / Claude いずれでも使用可能
* 出力にはMermaid図（ER図、シーケンス図、状態遷移図）を含む

### existing-system-design-prompt.md

* 既存システムの改修・機能追加向けの技術設計書変換プロンプト
* 認証・DB基盤等の既存資産を活用する前提で、差分設計と影響分析に特化
* 変更分類マーカー（`[既存流用]`/`[既存拡張]`/`[新規追加]`/`[推論補完]`）で変更範囲を可視化
* トランザクション整合性、セキュリティ影響分析、リリース後監視計画、品質ゲートを含む14セクション構成
* 5エージェント（Backend/QA/PM/Security/DevOps）によるレビュー済み

### technical-design-to-pbi-prompt.md

* 技術設計書からPBI/GitHub Issueに分割するプロンプト
* MoSCoW優先度、MVPスコープ識別、カテゴリ別DoD、ユーザーストーリー形式を含む
* Mermaid依存関係図、スプリント配置、リリース計画を出力
* GitHub CLI一括登録用CSV/コマンド出力にも対応

### claude-code-index-prompts.md

* Claude Codeでcursorライクなindexを作成させるプロンプト
* Spring Boot/iOS/Android/その他の情報でそれぞれindex.jsonファイルを作成させます。
* 指示の仕方の例
  * claude-code-index-prompts.md の内容を確認し、その指示に従ってください。
  * （作成後）作成したindexファイルをproject_indexフォルダに追加したあと、CLAUDE.mdに計画する際に読み込むべきファイルとしてルールを記載してください
