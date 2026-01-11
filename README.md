# Claude Code テンプレート

Claude Codeプロジェクト用のマルチ技術スタック対応テンプレート。

**核心: 「品質を自動で強制しながら、開発者の負担を減らす」**

## テンプレートの概要

このテンプレートは、Claude Code用の包括的な開発環境で、以下を提供します：

- **Kent Beck's TDD** と **Tidy First** 方法論に基づく開発
- **AI-Agentic Scrum** フレームワーク（1 Sprint = 1 PBI）
- **自動トリガースキル** で品質を強制
- **手動コマンド** で明示的なワークフロー制御

## セットアップ

```bash
# 1. テンプレートをプロジェクトにコピー
cp -r .claude /path/to/your/project/
cp CLAUDE.md /path/to/your/project/

# 2. プロジェクトでClaude Codeを起動
cd /path/to/your/project
claude
```

### 必要条件

- [Claude Code CLI](https://docs.anthropic.com/claude-code)
- [GitHub CLI](https://cli.github.com/) (`gh`) - fix-ci機能に必要
- Git

---

## 3つの開発ワークフロー

### 1. Plan Mode + /dig（推奨・最もシンプル）

新機能開発や複雑な変更に最適。

```
1. Plan Modeで開始
   claude --plan または /plan または shift + tab

2. Claude Codeが調査・計画を立てる

3. /dig で要件を言語化
   構造化された質問で曖昧な点を明確化
   → 人間が決断を提供

4. Plan Modeを抜けて実装
   完成度の高いPlanを元に実装
   （TDDスキルが自動適用）
```

**ユースケース**: 新機能開発、複雑な変更、要件が曖昧なタスク

### 2. Scrum開発（チーム/大規模PJ向け）

複数PBIを管理する大規模開発に最適。

```
1. /scrum:init でplan.md作成
   プロダクトゴール、バックログ、DoD等を設定

2. /go で次のタスクを実行
   - plan.mdの次の未完了テストを実装
   - TDDサイクルを自動的に回す

3. 繰り返し
   /go → テスト → 実装 → リファクタ → コミット

4. Sprint Review/Retrospectiveを実施
```

**ユースケース**: 複数PBIを管理する開発、チーム開発のシミュレーション

### 3. TDD開発（シンプル・単機能向け）

単一機能追加やバグ修正に最適。

```
/tdd:red      # 失敗するテストを1つ書く（コミットしない）
/tdd:green    # 最小限のコードでテストを通す
/git:commit   # コミット（feat: または test:）
/tdd:refactor # 構造改善（各ステップでコミット）
```

**ユースケース**: 単一機能追加、バグ修正、明確な要件のタスク

`implement FizzBuzz using TDD` のような指示で `tdd` スキルが自動トリガーされます。

---

## 典型的な使用シナリオ

### シナリオA: 新機能を追加したい

```
1. Plan Modeで「ユーザー認証機能を追加したい」と伝える
2. /dig を実行して要件を明確化（OAuth? JWT? etc）
3. 計画を承認
4. TDDスキルが自動適用されながら実装
5. コミット時にgit-commitスキルでWHY-focusedメッセージ
```

### シナリオB: バグを修正したい

```
1. 「ログイン時にエラーが出る」と伝える
2. AIが調査し、失敗テストを書く（RED）
3. 修正実装（GREEN）
4. 必要ならリファクタ
5. コミット
```

### シナリオC: Scrumで開発を管理したい

```
1. /scrum:init を実行
2. Product Goal、初期PBIを設定
3. plan.mdが生成される
4. /go でPBIを1つずつ実行
5. Sprint Review/Retrospectiveを実施
```

### シナリオD: 既存コードをリファクタしたい

```
1. 「このコードを整理して」と伝える
2. tidyingスキルが自動トリガー
3. 振る舞いを変えずに構造改善
4. 各改善ごとにコミット（Tidy First原則）
```

---

## 機能一覧

### Skills（自動トリガー）

条件を満たすと自動で動作。意識不要。

| スキル | 説明 | トリガー条件 |
|--------|------|-------------|
| **tdd** | Kent Beck's TDD（Red-Green-Refactor） | 「TDDで」「テストを書いて」等 |
| **tidying** | Kent Beck's "Tidy First?" | 「コードを整理して」「リファクタして」等 |
| **git-commit** | WHYフォーカスのコミット | コミット操作時 |
| **deslop** | AI生成コードパターンを除去 | コミット準備時 |
| **fix-ci** | CI失敗を自動診断・修正 | CIチェック失敗時 |
| **purpose-driven-impl** | 形骸化実装・スタブ防止 | 実装時 |
| **quality-guardrails** | テスト/lint設定を弱めず修正 | テスト/lint失敗時 |

### Commands（手動実行）

| コマンド | 説明 | いつ使う？ |
|----------|------|-----------|
| `/dig` | 構造化質問で曖昧さを解消 | Plan Mode中 |
| `/go` | plan.mdの次ステップを実行 | Scrum/計画駆動開発時 |
| `/tdd:red` | 失敗テスト1つを書く | TDDサイクル開始 |
| `/tdd:green` | テストを通す最小実装 | RED後 |
| `/tdd:refactor` | 構造改善（振る舞い変更なし） | GREEN後 |
| `/git:commit` | WHY-focused Conventional Commit | 変更をコミットする時 |
| `/scrum:init` | AI-Agentic Scrum初期化 | 新プロジェクト開始時 |

### Rules（常時有効）

#### quality-guardrails
- テストを通すためにテストを弱めない
- lint設定を緩めない
- CI設定を変更しない
- 実装を直すことで解決する

#### workflow
- 小さなインクリメントで開発
- TDD Red-Green-Refactor サイクル
- コミット前にテスト/lint実行

#### claude/config-maintenance
- Claude Code設定ファイルのメンテナンスガイド
- path-specificで `.claude/` 編集時のみ有効

### Agents（サブエージェント）

#### 設計支援エージェント

| エージェント | 説明 |
|--------------|------|
| **agent-architect** | 新しいエージェントを設計 |
| **agent-improver** | 既存エージェントを改善 |
| **agent-reviewer** | エージェント設定をレビュー |
| **claude-skills-architect** | スキルを設計・最適化 |
| **command-architect** | コマンドを設計 |

#### Scrumエージェント

| エージェント | 説明 |
|--------------|------|
| **scrum/team/product-owner** | Product Backlog管理、PBI順序付け |
| **scrum/team/scrum-master** | Scrumイベントファシリテーション |
| **scrum/team/developer** | TDD実装、タスク実行 |
| **scrum/events/sprint-planning** | Sprint Goal定義、計画 |
| **scrum/events/sprint-review** | Increment検査、フィードバック収集 |
| **scrum/events/sprint-retrospective** | チーム改善 |
| **scrum/events/backlog-refinement** | PBI分解・明確化 |

---

## ディレクトリ構造

```
.claude/
├── agents/                    # サブエージェント定義
│   ├── scrum/                # Scrumエージェント
│   │   ├── team/            # チームロール（PO, SM, Dev）
│   │   └── events/          # イベントファシリテーター
│   └── *.md                  # 設計支援エージェント
├── commands/                  # スラッシュコマンド（手動実行）
│   ├── dig/                  # 要件明確化
│   ├── git/                  # Gitコマンド
│   ├── tdd/                  # TDDコマンド（red/green/refactor）
│   ├── scrum/                # Scrumコマンド
│   └── go.md                 # 次ステップ実行
├── rules/                     # ルール（常時有効）
│   ├── claude/               # Claude Code設定ルール
│   ├── quality-guardrails.md # テスト/lint品質保証
│   └── workflow.md           # 開発ワークフロー
└── skills/                    # スキル（自動トリガー）
    ├── tdd/                  # TDD methodology
    ├── tidying/              # Tidy First methodology
    ├── git-commit/           # WHY-focused commits
    ├── deslop/               # AI slop除去
    ├── fix-ci/               # CI失敗自動修正
    ├── purpose-driven-impl/  # 形骸化実装防止
    └── quality-guardrails/   # 実装修正強制
```

---

## カスタマイズ

### プロジェクト固有ルールの追加

`.claude/rules/your-rule.md` を作成:

```markdown
# ルール名

ルールの内容をここに記述。
```

### カスタムコマンドの追加

`.claude/commands/your-command.md` を作成:

```markdown
---
description: "コマンドの説明"
---

コマンドの指示をここに記述。
```

### Skillsの拡張

`.claude/skills/your-skill/SKILL.md` を作成:

```markdown
---
name: your-skill
description: "スキルがトリガーされる条件"
---

スキルの指示をここに記述。
```

---

## 対応技術スタック

- TypeScript / JavaScript (Node.js, React, Vue, Next.js等)
- Python (Django, FastAPI, Flask等)
- Rust
- Go
- Java / Kotlin

---

## グローバル設定との連携

このテンプレートはグローバルなClaude Code設定と併用できます:

- `~/.claude/CLAUDE.md` - 個人の原則
- `~/.claude/rules/` - グローバルルール
- `~/.claude/skills/` - グローバルスキル

プロジェクトレベルの設定はグローバル設定を補完しますが、上書きはしません。

---

## まとめ

1. **シンプルに始める**: Plan Mode + TDDスキルの自動適用で十分
2. **必要に応じて**: `/dig` で曖昧性解消、`/tdd:*` で明示的TDD制御
3. **大規模なら**: `/scrum:init` でScrum管理
4. **品質は自動**: quality-guardrails, deslop, fix-ciが自動で動く

このテンプレートの核心は **「品質を強制しながら、開発者の負担を減らす」** ことです。

---

## クレジット

設定ファイルの一部は [atusy/dotfiles](https://github.com/atusy/dotfiles) を参考にしています。

## ライセンス

MIT
