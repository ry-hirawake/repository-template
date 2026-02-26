---
name: cmd-prj-init
description: AI駆動でproduct.mdとtech_stack.mdからプロジェクト構成を動的生成。Geminiで最新情報調査、Codexで設計相談し、技術スタックに応じた最適な初期構造を自動構築。
disable-model-invocation: true
---

# cmd-prj-init

## 概要

**AI駆動の動的プロジェクト生成**。`tech_stack.md`で定義済みの技術スタックをもとに、Geminiで2026年最新のベストプラクティスを調査し、Codexで最適なアーキテクチャ設計を確定後、カレントディレクトリにプロジェクト構成（`src/`等のディレクトリ・設定ファイル）を生成する。

**前提**: `.dgic/steering/product.md`（システム定義）と `.dgic/steering/tech_stack.md`（技術スタック選定）が存在すること

---

## 実行フロー

### Phase 1: ドキュメント読み込み

必須ファイル（`product.md`, `tech_stack.md`）確認後、自然言語理解で以下を抽出:
- プロジェクト名（例: 「タスク管理SaaS」→ `task-manager`）
- **選定済み技術スタック**（例: React, FastAPI, PostgreSQL）
- 要件概要・制約

---

### Phase 2: Gemini リサーチ

サブエージェント経由で**選定済み技術スタック**の最新情報をGemini調査（`references/research-guide.md` 参照）:
- 最新バージョン・依存関係
- 2026年推奨プロジェクト構造
- 必須ツール（Linter/Formatter/Test）
- Docker/CI/CD推奨
- セキュリティ・パフォーマンスBP

出力: 構造化JSON（実行可能なコマンド含む）

---

### Phase 3: Codex 設計相談

サブエージェント経由でCodex設計依頼（`references/design-guide.md` 参照）:
- アーキテクチャパターン（理由含む）
- ディレクトリ構造
- テスト戦略
- 認知的負債の最小化

出力: 設計判断の根拠と具体的構成

---

### Phase 4: ユーザー確認

```
🤖 AI駆動プロジェクト生成
📦 {project_name} | 🏗️ {tech_stack}
📊 Gemini（最新BP） + Codex（設計）完了
✅ はい / ❌ いいえ
```

---

### Phase 5: プロジェクト生成

Gemini + Codex結果に基づき**カレントディレクトリ**に作成:
- ディレクトリ作成（`src/`, `tests/`等）
- 依存関係・設定ファイル（package.json, .eslintrc, pytest.ini等）
- Docker/CI/CD設定（推奨時）
- README生成

---

### Phase 6: 完了報告

```
✅ 生成完了
📁 {ディレクトリツリー}
🎯 Codex設計: {アーキテクチャ} - {理由}
🚀 次: cd {project_name} && {セットアップコマンド}
🔍 ログ: .dgic/logs/gemini_research.json, codex_design.md
```

---

## 特徴

- 🌐 **柔軟な技術対応**: 静的テンプレート不要、AI調査で選定済み技術の最新構成を動的生成
- 🤖 **AI駆動の最適化**: Codexが要件・制約考慮し最適アーキテクチャ設計
- 📊 **2026年BP反映**: Geminiが最新推奨（Docker/CI/CD/セキュリティ）反映
- 🔄 **保守不要**: referencesファイル追加不要、技術更新時もAI自動対応
- 📝 **トレーサビリティ**: 調査・設計ログを`.dgic/logs/`保存

---

## エラーハンドリング

| エラー | 対応 |
|--------|------|
| **Gemini失敗** | 基本構成で継続、公式docs推奨 |
| **Codex失敗** | 標準パターン適用、手動調整推奨 |
| **両方失敗** | 公式ドキュメント参照、手動セットアップ |
| **必須ファイル不在** | `/cmd-spec-init`, `/cmd-tech-design-init` 実行促す |

---

## References

- `references/research-guide.md`: Gemini調査プロンプトテンプレート
- `references/design-guide.md`: Codex設計プロンプトテンプレート
