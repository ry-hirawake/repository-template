# Multi　Agent　Orchestration

ClaudeCodeがCodexCLIとGeminiCLIを統合し、AIDD × SDD × TDD × Agileの開発手法を駆使して、並列開発を行う。

---

## Why This Exists

| Agent | Strength | Use For |
|-------|----------|---------|
| **Claude Code** | 1Mコンテキスト、オーケストレーション、Agent Teams | 全体統括、コードベース分析、並列チーム管理 |
| **Codex CLI** | 深い推論、設計判断、デバッグ | 設計相談、エラー分析、トレードオフ評価 |
| **Gemini CLI** | Google Search、マルチモーダル | 外部情報取得、ライブラリ調査、PDF/動画/音声処理 |

**IMPORTANT**: 単体では難しいタスクも、3エージェントの協調で解決できる。

---

## Context Management (CRITICAL)

Claude Code (Opus 4.6) のコンテキストは **1M トークン**（実質 **350-500k**、ツール定義等で縮小）。

**YOU MUST** サブエージェント経由で Codex/Gemini を呼び出す（出力が10行以上の場合）。

| 出力サイズ | 方法 | 理由 |
|-----------|------|------|
| 1-2文 | 直接呼び出しOK | オーバーヘッド不要 |
| 10行以上 | **サブエージェント経由** | メインコンテキスト保護 |
| 分析レポート | サブエージェント → ファイル保存 | 詳細は `.claude/docs/` に永続化 |

```
# MUST: サブエージェント経由（大きな出力）
Task(subagent_type="general-purpose", prompt="Codexに設計を相談し、要約を返して")

# OK: 直接呼び出し（小さな出力のみ）
Bash("codex exec ... '1文で答えて'")
```

---

## Quick Reference

### Codex を使う時

- 設計判断（「どう実装？」「どのパターン？」）
- デバッグ（「なぜ動かない？」「エラーの原因は？」）
- 比較検討（「AとBどちらがいい？」）

→ 詳細: `.claude/rules/codex-delegation.md`

### Gemini を使う時

- 外部リサーチ（「最新のドキュメントは？」「ライブラリを調べて」）
- マルチモーダル（「このPDF/動画/音声を見て」）

> **注意**: コードベース分析は Claude が直接行う。Gemini への委託は不要。

→ 詳細: `.claude/rules/gemini-delegation.md`

---

## WorkFlow

```
# プロジェクト開始準備
/cmd-spec-init           Phase 0.1: PRJの目的、システム概要策定
/cmd-tech-design-init    Phase 0.2: 技術スタック選定
/cmd-prj-init            Phase 0.3: PRJリポジトリ初期化

#　要件定義
/cmd-prj-steering        Phase 1: PRJ理解
/cmd-requirement         Phase 2: 要件精査
/cmd-story               Phase 3: Story作成
/cmd-ui-design           Phase 4: UIデザイン作成
/cmd-tech-design         Phase 5: 調査&設計

# 計画
/cmd-task                Phase 6: タスク作成

# 製造
/cmd-impl                Phase 7: # 実装（TDD：テスト、実装、動作確認）

# レビュー
/cmd-review              Phase 8: Code Review、UI Review

# コミット
/cmd-commit              Phase 9: コミット

```



## Documentation

| Location | Content |
|----------|---------|
| `.claude/rules/` |  |
| `.claude/docs/DESIGN.md` |  |
| `.claude/docs/research/` | 調査結果（Gemini / レビュー） |
| `.claude/docs/libraries/` | ライブラリ制約ドキュメント |
| `.claude/logs/cli-tools.jsonl` | Codex/Gemini入出力ログ |

---

## Language Protocol

- **思考・コード**: 英語
- **ユーザー対話**: 日本語

---

## Steering Configuration

---

## Development Guidelines


---

## Development Rules

