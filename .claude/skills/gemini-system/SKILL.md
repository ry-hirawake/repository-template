---
name: gemini-system
description: 調査、大規模コードベースの理解、そしてマルチモーダルデータ処理のために、Gemini CLI を積極的に活用してください。Gemini は、大規模なコンテキストウィンドウ（100万トークン）、Google 検索のグラウンディング、動画/音声/PDF 分析、そしてリポジトリ全体の理解といった点で優れています。実装前の調査、ドキュメント分析、そしてマルチモーダルタスクにご利用ください。明示的なトリガー：「調査」、「調査」、「動画/音声/PDF の分析」、「コードベースの理解」。
metadata:
  short-description: Claude Code ↔ Gemini CLI collaboration (research & multimodal)
---

# Gemini システム — リサーチ＆マルチモーダル スペシャリスト

**Gemini CLI (gemini-3-pro-preview) は、100万トークンのコンテキストを備えたリサーチ スペシャリストです。**

> **詳細ルール**: `.claude/rules/gemini-delegation.md`

## コンテキスト管理（重要）

**サブエージェント経由を推奨。** Gemini出力は大きくなりがちなため。

| 状況 | 方法 |
|------|------|
| コードベース分析 | サブエージェント経由（推奨） |
| ライブラリ調査 | サブエージェント経由（推奨） |
| マルチモーダル | サブエージェント経由（推奨） |
| 短い質問 (1-2文回答) | 直接呼び出しOK |

## Gemini vs Codex

| Task | Gemini | Codex |
|------|--------|-------|
| **リポジトリ全体理解** | ✓ | |
| **ライブラリ調査** | ✓ | |
| **マルチモーダル (PDF/動画/音声)** | ✓ | |
| **最新ドキュメント検索** | ✓ | |
| **設計判断** | | ✓ |
| **デバッグ** | | ✓ |
| **コード実装** | | ✓ |

## いつ相談するか（必須）
  
| 状況 | トリガー例 |
|-----------|------------------|
| **調査** | 「調べて」「リサーチ」 / "Research" "Investigate" |
| **ライブラリドキュメント** | 「ライブラリ」「ドキュメント」 / "Library" "Docs" |
| **コードベース分析** | 「コードベース全体」 / "Entire codebase" |
| **マルチモーダル** | 「PDF」「動画」「音声」 / "PDF" "Video" "Audio" |

## 参照すべきでないケース

- 設計上の決定（Codex を使用）
- デバッグ（Codex を使用）
- コードの実装（Codex を使用）
- 単純なファイル操作（直接実行）

## 相談方法

### 推奨: サブエージェントパターン

**メインコンテキストを維持するには、タスクツールで `subagent_type='general-purpose'` を使用してください。**

```
タスクツールのパラメータ:
- subagent_type: "general-purpose"
- run_in_background: true (オプション、並列処理用)
- prompt: |
    Research: {topic}

    gemini -p "{research question}" 2>/dev/null

    出力全体を次の場所に保存: .claude/docs/research/{topic}.md
    簡潔な要約 (5～7 個の箇条書き) を返します。
```

### 直接呼び出し（短い質問のみ）

簡単な回答が期待できる簡単な質問の場合：

```bash
gemini -p "簡単な質問" 2>/dev/null
```

### CLI オプションリファレンス

```bash
# コードベース解析
gemini -p "{question}" --include-directories . 2>/dev/null

# マルチモーダル (PDF/ビデオ/オーディオ)
gemini -p "{prompt}" < /path/to/file.pdf 2>/dev/null

# JSON 出力
gemini -p "{question}" --output-format json 2>/dev/null
```

### ワークフロー (サブエージェント)

1. **サブエージェントを生成** (Gemini リサーチプロンプトを使用)
2. **作業を続行** → サブエージェントが並行して実行
3. **サマリーを受信** → サブエージェントが主要な調査結果を返す
4. **出力全体を保存** → `.claude/docs/research/{topic}.md`

## 言語プロトコル

1. Gemini に **英語** で質問する
2. **英語** で回答を受け取る
3. 結果をまとめ、適用する
4. ユーザーに **日本語** で報告する

## 出力先

Gemini の研究結果を次の場所に保存します:
```
.claude/docs/research/{topic}.md
```

これにより、Claude と Codex は後で調査結果を参照できるようになります。

## タスクテンプレート

### 実装前調査

```bash
gemini -p "Python 2025 の {feature} に関するベストプラクティスを調査します。
以下を含めます:
- 一般的なパターンとアンチパターン
- ライブラリの推奨事項（比較付き）
- パフォーマンスに関する考慮事項
- セキュリティ上の懸念事項
- コード例" 2>/dev/null
```

### リポジトリ分析

```bash
gemini -p "このリポジトリを分析します:
1. アーキテクチャの概要
2. 主要モジュールと役割
3. コンポーネント間のデータフロー
4. エントリポイントと拡張ポイント
5. 従うべき既存のパターン" --include-directories . 2>/dev/null
```

### ライブラリ調査

参照: `references/lib-research-task.md`

### マルチモーダル分析

```bash
# ビデオ
gemini -p "ビデオの分析: 主要概念、要点、タイムスタンプ" < tutorial.mp4 2>/dev/null

# PDF
gemini -p "抽出: API 仕様、例、制約" < api-docs.pdf 2>/dev/null

# オーディオ
gemini -p "文字起こしと要約: 決定事項、アクション項目" < meeting.mp3 2>/dev/null
```

## Codex との統合

| ワークフロー | 手順 |
|---------|-------|
| **新機能** | Gemini での調査 → Codex での設計レビュー |
| **ライブラリの選択** | Gemini での比較 → Codex での決定 |
| **バグ調査** | Gemini のコードベース検索 → Codex でのデバッグ |

## Gemini を選ぶ理由

- **100万トークンのコンテキスト**: リポジトリ全体を一度に参照
- **Google 検索**: 最新の情報とドキュメント
- **マルチモーダル**: ネイティブ PDF/動画/音声処理
- **高速探索**: ディープワーク前の簡単な概要確認
- **共有コンテキスト**: Claude/Codex 用に保存された結果
