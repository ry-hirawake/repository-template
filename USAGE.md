# How To Use

---

## 1. 

### 1.1. フレームワークのインストール
- 新規プロジェクト直下で下記のコマンドを実行
curl -L https://api.github.com/repos/ry-hirawake/repository-template/tarball/main | tar -xz --strip-components=1 --exclude='*/README.md'

---

## 2. SetUp

### 2.1. Claude Code インストール
- Claude Codeのインストールを実施
https://code.claude.com/docs/ja/setup

### 2.2. Codex CLI インストール
- Codex CLIのインストールを実施
https://developers.openai.com/codex/cli/

### 2.3. Gemini CLI インストール
- Gemini CLIのインストールを実施
https://cloud.google.com/blog/ja/topics/developers-practitioners/introducing-gemini-cli

---

## 3. WorkFlow

### 3.1. プロジェクトへの導入方法
1. Githubでリポジトリを作成
2. リポジトリ内にフレームワークをインストール
3. Claude Code、Codex CLI、Gemini CLIをインストール

### 3.2. PRJ開始時
1. /cmd-spec-init           # PRJ要件定義作成
2. /cmd-tech-design-init    # PRJ設計定義作成
3. /cmd-prj-init            # PRJリポジトリ初期化

### 3.3. 開発フェーズ（機能単位で繰り返し）
1. /cmd-prj-steering        # PRJコンテキスト作成
2. /cmd-requirement         # 要件精査
3. /cmd-story               # Story作成
4. /cmd-ui-design           # UIデザイン作成
5. /cmd-tech-design         # 調査&設計
6. /cmd-task                # タスク作成
7. /cmd-impl                # 実装（TDD：テスト、実装、動作確認）
8. /cmd-review              # Code Review、UI Review
9. /cmd-commit              # コミット
