# GitHub の使い方入門

> GitHubの基本から実践的なTipsまでを網羅したリファレンスガイドです。

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com)
[![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)](https://git-scm.com)

---

## 目次

1. [GitHubとは](#1-githubとは)
2. [基本用語集](#2-基本用語集)
3. [リポジトリの操作](#3-リポジトリの操作)
4. [ブランチとマージ](#4-ブランチとマージ)
5. [プルリクエスト (PR)](#5-プルリクエスト-pr)
6. [Issues（課題管理）](#6-issues課題管理)
7. [GitHub Actions（CI/CD）](#7-github-actionscicd)
8. [Projects（タスク管理）](#8-projectsタスク管理)
9. [GitHub Pages](#9-github-pages)
10. [セキュリティ機能](#10-セキュリティ機能)
11. [コラボレーション機能](#11-コラボレーション機能)
12. [便利なTips & ショートカット](#12-便利なtips--ショートカット)
13. [よく使うGitコマンド早見表](#13-よく使うgitコマンド早見表)

---

## 1. GitHubとは

GitHubは **Gitリポジトリをホスティングするクラウドサービス** です。コードの管理だけでなく、チームでの共同開発・レビュー・CI/CD・プロジェクト管理まで一括して行えます。

- 個人の学習・ポートフォリオ公開から、世界最大のOSSプロジェクト（Linux Kernel・VSCode等）まで利用されている
- Gitは **バージョン管理システム（VCS）** そのもの、GitHubはGitをホストする **プラットフォーム**（混同注意）
- 競合サービス：GitLab / Bitbucket / Gitea（セルフホスト）

---

## 2. 基本用語集

| 用語 | 読み方 | 意味 |
|------|--------|------|
| **Repository (repo)** | リポジトリ | プロジェクトのファイルと履歴をまとめた場所 |
| **Commit** | コミット | 変更を記録するスナップショット。メッセージを付ける |
| **Branch** | ブランチ | コミット履歴の分岐。並行作業に使う |
| **Merge** | マージ | ブランチの変更を別のブランチに統合すること |
| **Clone** | クローン | リモートリポジトリをローカルにコピーする操作 |
| **Fork** | フォーク | 他人のリポジトリを自分のアカウントにコピーすること |
| **Pull Request (PR)** | プルリクエスト | ブランチの変更をレビューしてもらい、マージを依頼する仕組み |
| **Issue** | イシュー | バグ報告・機能要望・タスクを管理する課題票 |
| **Tag** | タグ | 特定のコミットに名前を付ける（バージョン管理に使う） |
| **Release** | リリース | タグに説明・バイナリ等を付けて公開するもの |
| **README** | リードミー | プロジェクトの説明を書くファイル（Markdown形式） |
| **HEAD** | ヘッド | 現在チェックアウトしているコミットを示すポインタ |
| **Origin** | オリジン | デフォルトのリモートリポジトリの名前 |
| **Upstream** | アップストリーム | フォーク元の本家リポジトリを指すことが多い |
| **Stash** | スタッシュ | 作業中の変更を一時退避する機能 |
| **Rebase** | リベース | コミット履歴を整理・移植する操作 |
| **Cherry-pick** | チェリーピック | 特定のコミットだけを別ブランチに適用する |
| **.gitignore** | ギットイグノア | Gitが無視するファイル・フォルダのルールを書くファイル |
| **Workflow** | ワークフロー | GitHub Actionsで実行する自動化の定義ファイル |
| **Artifact** | アーティファクト | ActionsのジョブがCI中に生成したファイル（ビルド結果等） |

---

## 3. リポジトリの操作

### 3.1 リポジトリの作成

```bash
# GitHub CLI を使う（最速）
gh repo create my-project --public --clone

# または GitHub Web UI で作成後にクローン
git clone https://github.com/username/my-project.git
cd my-project
```

### 3.2 基本的な作業の流れ

```
[ローカル編集] → git add → git commit → git push → [GitHub上にアップ]
```

```bash
git add .                        # 全変更をステージング
git add src/main.py              # 特定ファイルのみ
git commit -m "feat: ログイン機能を追加"
git push origin main             # main ブランチへ push
```

### 3.3 コミットメッセージの慣習（Conventional Commits）

```
<type>: <概要>

feat:     新機能
fix:      バグ修正
docs:     ドキュメントのみの変更
style:    コードの意味に影響しない変更（フォーマット等）
refactor: リファクタリング
test:     テストの追加・修正
chore:    ビルド・補助ツールの変更
```

### 3.4 リモートとの同期

```bash
git fetch origin          # リモートの変更情報を取得（ローカルには反映しない）
git pull origin main      # fetch + merge（リモートの変更をローカルに取り込む）
git pull --rebase         # pull 時に rebase を使う（履歴が線形になる）
```

---

## 4. ブランチとマージ

### 4.1 ブランチの基本

```bash
git branch feature/login          # ブランチを作成
git checkout feature/login        # ブランチを切り替え
git checkout -b feature/login     # 作成 + 切り替えを一度に

# モダンな書き方（Git 2.23+）
git switch -c feature/login       # 作成 + 切り替え
git switch main                   # mainへ戻る
```

### 4.2 マージ

```bash
git checkout main
git merge feature/login           # 通常マージ（マージコミットが生まれる）
git merge --squash feature/login  # 全コミットを1つにまとめてマージ
git merge --no-ff feature/login   # 必ずマージコミットを作る
```

### 4.3 マージ戦略の比較

| 戦略 | コマンド | 特徴 |
|------|---------|------|
| **Merge commit** | `git merge` | 履歴が完全に残る。PRがどこから来たか分かる |
| **Squash merge** | `--squash` | ブランチのコミットを1つに圧縮。履歴がきれいになる |
| **Rebase merge** | `git rebase` | 線形履歴になる。コンフリクト解消が複雑になることも |

### 4.4 コンフリクト解消

```bash
git merge feature/login
# コンフリクト発生 → ファイルを手動で修正する
# <<<<<<< HEAD
# ここが現在のブランチの内容
# =======
# ここがマージ先の内容
# >>>>>>> feature/login

git add conflicted-file.txt
git commit                        # マージコミットを作成
```

### 4.5 Rebase

```bash
git checkout feature/login
git rebase main                   # main の最新コミットを起点に付け直す
git rebase -i HEAD~3              # 直近3コミットをインタラクティブに整理
```

> ⚠️ **注意**: すでに push したブランチへの rebase は、他の開発者の履歴を壊す可能性がある。`git push --force-with-lease` を使うこと。

---

## 5. プルリクエスト (PR)

### 5.1 PR の作成

```bash
# GitHub CLI
gh pr create --title "feat: ログイン機能" --body "## 概要\nOAuth2を使ったログインを実装"

# Web UI でも作成可能
```

### 5.2 PR の種類と状態

| 種別 | 説明 |
|------|------|
| **Draft PR** | まだレビュー不要な WIP (Work In Progress) PR。マージ不可 |
| **Ready for review** | レビュー可能な通常 PR |
| **Approved** | レビュアーが承認した状態 |
| **Changes requested** | 修正が必要とレビュアーが判断した状態 |

### 5.3 レビュー機能

- **コメント (Comment)**: 質問・提案のみ
- **Approve**: 変更に同意してマージを承認
- **Request changes**: マージ前に修正が必要
- **Suggested changes**: コード上で直接修正案を提示できる（1クリックで適用可能）

### 5.4 PRのマージ方法

| 方法 | 説明 | 使いどころ |
|------|------|-----------|
| **Create a merge commit** | マージコミットを作成 | 履歴を完全に残したいとき |
| **Squash and merge** | 全コミットを1つに圧縮 | 細かいコミットを整理したいとき |
| **Rebase and merge** | 線形履歴にする | 常にきれいな履歴を保ちたいとき |

### 5.5 便利な機能

```markdown
<!-- PRの説明文でIssueを自動クローズ -->
Closes #42
Fixes #10
Resolves #8

<!-- 特定ユーザーにレビュー依頼 -->
@username お願いします

<!-- チェックボックス -->
- [x] テスト追加
- [ ] ドキュメント更新
```

---

## 6. Issues（課題管理）

### 6.1 Issueの構成要素

| 要素 | 説明 |
|------|------|
| **Title** | 課題の件名 |
| **Body** | 詳細説明（Markdown対応） |
| **Label** | カテゴリタグ（bug / enhancement / documentation 等） |
| **Milestone** | 目標バージョンや期限でグループ化 |
| **Assignee** | 担当者 |
| **Project** | 関連プロジェクトボード |

### 6.2 Issue テンプレート

`.github/ISSUE_TEMPLATE/bug_report.md` を作成すると、Issue作成時に自動でフォームが表示される。

```markdown
---
name: バグ報告
about: バグを報告する
labels: bug
---

## バグの内容

## 再現手順
1. 
2. 

## 期待する動作

## 実際の動作

## 環境
- OS:
- バージョン:
```

### 6.3 Issue の検索フィルタ

```
is:open is:issue label:bug         # 未解決のバグ
assignee:username                  # 特定ユーザーのIssue
milestone:"v1.0"                   # マイルストーン指定
no:assignee                        # 担当者なし
```

---

## 7. GitHub Actions（CI/CD）

### 7.1 概要

GitHubリポジトリに `.github/workflows/*.yml` を置くだけで、**CI/CDパイプライン**を自動実行できる。

### 7.2 基本的な Workflow の例

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4          # コードをチェックアウト
      - uses: actions/setup-node@v4        # Node.js をセットアップ
        with:
          node-version: '20'
      - run: npm ci                         # 依存関係をインストール
      - run: npm test                       # テスト実行
```

### 7.3 主要なトリガー (on:)

| トリガー | 説明 |
|----------|------|
| `push` | push 時に実行 |
| `pull_request` | PR 作成・更新時 |
| `schedule` | cron 式で定期実行 |
| `workflow_dispatch` | 手動実行（ボタンで起動） |
| `release` | Release 作成時 |

### 7.4 Secrets（機密情報）

```yaml
# リポジトリの Settings > Secrets に登録したキーを使う
- run: deploy.sh
  env:
    API_KEY: ${{ secrets.API_KEY }}
    DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

### 7.5 よく使う公式 Actions

| Action | 用途 |
|--------|------|
| `actions/checkout` | リポジトリのチェックアウト |
| `actions/setup-node` | Node.js 環境セットアップ |
| `actions/setup-python` | Python 環境セットアップ |
| `actions/upload-artifact` | ビルド成果物の保存 |
| `actions/cache` | 依存関係のキャッシュ |
| `github/codeql-action` | コードの脆弱性スキャン |

---

## 8. Projects（タスク管理）

GitHub Projects は **カンバンボード・テーブル・ロードマップ** ビューでタスクを管理できる機能。

### 8.1 ビューの種類

| ビュー | 用途 |
|--------|------|
| **Board（カンバン）** | Todo / In Progress / Done の列で管理 |
| **Table** | スプレッドシート形式で一覧管理 |
| **Roadmap** | タイムライン（ガントチャート）形式 |

### 8.2 活用ポイント

- Issue や PR をプロジェクトに追加すると自動で進捗が連携
- カスタムフィールド（優先度・ストーリーポイント等）を追加できる
- 自動化（Automation）でステータスを自動更新できる

---

## 9. GitHub Pages

**静的サイトを無料でホスティング**できる機能。ポートフォリオ・ドキュメントサイト・ブログに最適。

### 9.1 有効化の手順

1. `Settings` > `Pages` を開く
2. Source を `Deploy from a branch` に設定
3. ブランチ（`main` or `gh-pages`）とフォルダ（`/` or `/docs`）を選択
4. `https://username.github.io/repo-name/` で公開される

### 9.2 Jekyll との連携

GitHub Pages は Jekyll（静的サイトジェネレーター）に対応。`_config.yml` を置くだけで Markdown をサイトに変換できる。

```yaml
# _config.yml
title: My Docs
theme: minima
```

---

## 10. セキュリティ機能

### 10.1 Dependabot

依存ライブラリの脆弱性を自動検出し、**バージョンアップ PR を自動作成**する。

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

### 10.2 Code Scanning

**CodeQL** を使ってコードの脆弱性を静的解析する。Actions Workflow として設定する。

```yaml
- uses: github/codeql-action/analyze@v3
  with:
    languages: ['javascript', 'python']
```

### 10.3 Secret Scanning

コード内にAPIキーや認証情報が含まれていないか自動でスキャンする。検出するとアラートを通知。

### 10.4 Branch Protection Rules

`Settings` > `Branches` で設定できる。

- **Require PR before merging**: 直接 push を禁止、PR 必須
- **Require status checks**: CI 通過を必須にする
- **Require review from Code Owners**: `CODEOWNERS` ファイルで指定した人のレビューを必須にする
- **Require signed commits**: GPG署名を必須にする

---

## 11. コラボレーション機能

### 11.1 Collaborators と Team

- **Collaborators**: 個人リポジトリに特定ユーザーを招待
- **Organization**: 会社・チーム単位の管理
  - **Team**: Org内でグループを作り、権限を一括管理
  - **Role**: Read / Triage / Write / Maintain / Admin

### 11.2 CODEOWNERS

```
# .github/CODEOWNERS
*.js    @frontend-team
*.py    @backend-team
docs/   @documentation-team
```

指定したファイルが変更されたPRに、自動でレビュアーが追加される。

### 11.3 Discussions

Issue とは別に、QA・アイデア共有・アナウンスなどができる**フォーラム機能**。

### 11.4 Wiki

リポジトリに付属するドキュメントページ。git で管理はされないが、Markdown で記述できる。

### 11.5 GitHub Gist

**スニペット（コード断片）を手軽に共有**できるサービス。

- Public Gist: 全体公開
- Secret Gist: URLを知っている人だけアクセス可

---

## 12. 便利なTips & ショートカット

### 12.1 キーボードショートカット（Web UI）

| キー | 動作 |
|------|------|
| `T` | ファイル検索（fuzzy search） |
| `B` | Git Blame を表示 |
| `L` | 行番号へジャンプ |
| `W` | ブランチ・タグの切り替え |
| `.`（ドット） | **github.dev**（ブラウザ上のVSCode）を開く |
| `>` | github.dev をライトモードで開く |
| `?` | ショートカット一覧を表示 |
| `S` or `/` | 検索ボックスにフォーカス |

### 12.2 GitHub CLI（`gh`コマンド）

```bash
# インストール
brew install gh
gh auth login

# リポジトリ操作
gh repo create my-app --public     # リポジトリ作成
gh repo clone owner/repo           # クローン
gh repo view --web                 # ブラウザで開く

# PR 操作
gh pr create                       # PR 作成（対話式）
gh pr list                         # PR 一覧
gh pr checkout 42                  # PR #42 をチェックアウト
gh pr merge --squash               # squash でマージ

# Issue 操作
gh issue create                    # Issue 作成
gh issue list --label bug          # バグラベルの Issue 一覧
gh issue close 10                  # Issue #10 をクローズ
```

### 12.3 Codespaces

ブラウザ or VS Code からクラウド上の開発環境を起動できる。ローカル環境のセットアップ不要。

```bash
gh cs create --repo owner/repo     # Codespace を起動
gh cs list                         # 一覧
```

### 12.4 GitHub Markdown の便利記法

````markdown
<!-- タスクリスト -->
- [x] 完了タスク
- [ ] 未完了タスク

<!-- 折りたたみ -->
<details>
<summary>詳細を見る</summary>
ここに内容を書く
</details>

<!-- 注意書き（GFM Alerts） -->
> [!NOTE]
> 補足情報です

> [!WARNING]
> 注意が必要です

> [!IMPORTANT]
> 重要な情報です

<!-- メンション -->
@username       <!-- ユーザー -->
@org/team-name  <!-- チーム -->

<!-- Issue/PR のリンク -->
#42             <!-- 同リポジトリの Issue/PR -->
owner/repo#42   <!-- 別リポジトリ -->
````

### 12.5 READMEバッジの例

```markdown
[![CI](https://github.com/owner/repo/actions/workflows/ci.yml/badge.svg)](https://github.com/owner/repo/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![npm version](https://badge.fury.io/js/package-name.svg)](https://badge.fury.io/js/package-name)
```

### 12.6 便利な URL パターン

```
# 差分を見る
https://github.com/owner/repo/compare/v1.0...v2.0

# 特定コミットの差分
https://github.com/owner/repo/commit/<sha>

# ファイルの特定行にリンク
https://github.com/owner/repo/blob/main/src/app.js#L42-L60

# 最新リリースのダウンロード
https://github.com/owner/repo/releases/latest
```

### 12.7 .gitignore のテンプレート

```bash
# 言語・フレームワーク別のテンプレートを取得
curl -L https://www.gitignore.io/api/node,macos,visualstudiocode > .gitignore
```

GitHub のリポジトリ作成時に言語を選択すると自動生成もされる。

---

## 13. よく使うGitコマンド早見表

```bash
# 初期設定
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"   # VSCode をエディタに

# リポジトリ
git init                          # ローカルで初期化
git clone <url>                   # クローン
git remote -v                     # リモートを確認
git remote add upstream <url>     # upstream を追加（フォーク時）

# 状態確認
git status                        # 変更状態を確認
git log --oneline --graph         # 履歴をグラフ表示
git diff                          # 差分を確認（unstaged）
git diff --staged                 # ステージ済みの差分

# 変更の保存
git add <file>                    # ステージング
git add -p                        # 変更を部分的にステージング
git commit -m "message"           # コミット
git commit --amend                # 直前のコミットを修正（未pushのみ）

# ブランチ
git branch -a                     # 全ブランチ一覧
git branch -d feature/xxx         # ブランチ削除（マージ済みのみ）
git branch -D feature/xxx         # ブランチ強制削除

# 取り消し・戻す
git restore <file>                # ファイルの変更を破棄（unstaged）
git restore --staged <file>       # ステージングを取り消す
git reset HEAD~1                  # 直前のコミットを取り消す（変更は残す）
git reset --hard HEAD~1           # 直前のコミットと変更を全て取り消す
git revert <sha>                  # コミットを打ち消すコミットを作る（履歴に残る）

# Stash
git stash                         # 現在の変更を退避
git stash pop                     # 退避した変更を復元
git stash list                    # stash の一覧

# タグ
git tag v1.0.0                    # タグを付ける
git tag -a v1.0.0 -m "Release"   # アノテーション付きタグ
git push origin --tags            # タグを push

# その他
git cherry-pick <sha>             # 特定コミットを適用
git bisect start                  # バグ混入コミットを二分探索で特定
git blame <file>                  # 各行のコミット・作者を表示
git reflog                        # HEAD の移動履歴（ほぼ何でも復元可）
```

---

## 覚えておくべき重要ポイント

1. **`git reflog` はほぼ何でも戻せる** — 誤って reset や rebase しても慌てない
2. **force push は `--force-with-lease` を使う** — 他の人の push を上書きしないよう安全に
3. **PRはこまめに作る** — 大きなPRよりも小さなPRの方がレビューされやすい
4. **Issueを先に作ってからコードを書く** — 意図・背景が残りチームと認識合わせができる
5. **デフォルトブランチを保護する** — Branch Protection Rules で main への直push を禁止
6. **Secretsをコードに書かない** — `.env` は必ず `.gitignore` に追加し、GitHub Secrets を使う

---

*このガイドは随時更新されます。不明な点は [GitHub Docs](https://docs.github.com) も参照してください。*
