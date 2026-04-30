# DEPLOY: 初回 GitHub Public Repo 作成

このファイルは初回デプロイ用。デプロイ完了後、`README.md` に役割が引き継がれるため**削除してOK**。

---

## 前提

- GitHub アカウント: `ironborn365`
- リポジトリ名: `ai-governance`(変更可、ただし全ファイル内のURL書き換え必要)
- 公開設定: **Public**(Private だと AI が raw URL で取得できない)
- 機微情報なし(本リポジトリにはガバナンス文書のみ)

## 手順

### Step 1: GitHub上でリポジトリ作成

ブラウザで https://github.com/new を開き:

- Owner: `ironborn365`
- Repository name: `ai-governance`
- Description: `AI agent governance documents (charter, lessons, operation guide, patterns) — public mirror of Notion canonical source for cross-AI accessibility`
- Public: ✅
- README, .gitignore, License: **チェックなし**(本パッケージに含まれているため)

「Create repository」をクリック。

### Step 2: ローカルで初期化&push

zipを `~/dev/` 配下などに展開した後、PowerShell で:

```powershell
cd path\to\ai-governance

# 初期化
git init
git branch -M main
git add .
git commit -m "Initial commit: AI governance docs (charter, lessons, operation guide, patterns)"

# リモート登録&push
git remote add origin https://github.com/ironborn365/ai-governance.git
git push -u origin main
```

### Step 3: 確認

以下のURLが**ブラウザで直接開ける**か確認:

- https://github.com/ironborn365/ai-governance
- https://raw.githubusercontent.com/ironborn365/ai-governance/main/llms.txt
- https://raw.githubusercontent.com/ironborn365/ai-governance/main/llms-full.txt

raw URLが開ければデプロイ成功。

### Step 4: AIへの周知

#### Claude(このチャット): 既にメモリ追加済み(完了時に更新提案あり)

#### Claude Code:
プロジェクト・Vaultの `CLAUDE.md` から本リポジトリへのリンクを追記(任意、必要時に)

#### Cowork:
グローバル指示にURLを登録(設定画面から)

#### Gemini:
新会話開始時、以下を貼り付け:

```
ガバナンス指針として以下のURLの内容に従ってください:
https://raw.githubusercontent.com/ironborn365/ai-governance/main/llms-full.txt
```

または `llms-full.txt` の中身を直接コピペ。

### Step 5: DEPLOY.md 削除

デプロイ完了後、本ファイルは不要なので削除してコミット:

```powershell
git rm DEPLOY.md
git commit -m "Remove DEPLOY.md after initial deployment"
git push
```

## 同期ポリシー(以降の運用)

1. Notion で編集
2. 本リポジトリにも反映(手動コピペ or 同期スクリプト)
3. `git commit -m "Sync from Notion: <変更概要>"` で push

将来的には Notion → GitHub の自動同期スクリプトも検討可能。

## トラブルシューティング

### `git push` でエラー
- GitHub Personal Access Token が必要(2021年以降パスワード認証廃止)
- 設定: https://github.com/settings/tokens で `repo` スコープのトークンを発行
- push時にユーザー名 + トークンを入力

### URLが404
- リポジトリが Private になっていないか確認
- ブランチ名が `main` か `master` か確認(URLは main 前提)
- ファイル名のtypoを確認(`llms.txt` であって `llm.txt` でない)

### Notion との内容ずれ
- 本リポジトリは**Notion正典のミラー**であり、編集元はNotion
- ずれを発見したら Notion を正として、こちらを上書きコミット
