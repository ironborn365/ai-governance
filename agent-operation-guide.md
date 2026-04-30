# 🧭 AIエージェント運用ガイド ── 迷わないための実行コンテキスト指針

**目的:** Claude / Claude Code / Cowork / Gemini など複数AIエージェントが、自分の現在地と読み込みコンテキストを把握し、迷わず行動するための運用ガイド。

**位置づけ:** AI連携憲章が「**何を守るか**」を定めるのに対し、本ガイドは「**現場でどう動くか**」を扱う実務マニュアル。

**最終更新:** 2026-04-30
**正典:** Notion `🧭 AIエージェント運用ガイド`
**本ミラー:** https://raw.githubusercontent.com/ironborn365/ai-governance/main/agent-operation-guide.md

---

## 1. セッション開始時の自己診断(全AI共通)

すべてのエージェントは作業に入る前に以下を自問する。

### Q1: 私は誰か(どのAIか)

自分が Claude(claude.ai) / Claude Code / Cowork / Gemini / Cursor統合AI のどれか自覚する。違いによって読み込めるリソースが異なる。

### Q2: 私は何を読み込んでいるか

**Claude (claude.ai):** システムプロンプト、ユーザーメモリ、接続済みMCP(Notion/Calendar/Gmail/Drive)

**Claude Code(最も誤解されやすい):**
- 起動した cwd の `CLAUDE.md`
- **cwdの親ディレクトリの `CLAUDE.md`**(自動ロード)
- **`~/.claude/CLAUDE.md`**(グローバル、常に自動ロード)
- `cwd/.claude/agents/*` および `~/.claude/agents/*`
- アクセス時にサブディレクトリの CLAUDE.md

**Cowork:** グローバル指示(設定済みの場合)、フォルダ固有指示、接続済みMCPコネクタ

**Gemini:** ユーザーが貼ったブートストラッププロンプト、添付ファイル、Notion非接続

### Q3: 何を期待されているか

ユーザー指示の本質を確認。過去のコンテキスト(Notion / 過去会話 / メモリ)を必要に応じて参照する。

### Q4: 私は本当にこれを実行する適任か

AI連携憲章の原則2に従い、より適したツールがあれば**実行前に**提案する。

---

## 2. リソース配置マップ(何がどこにあるか)

### 🌐 Notion(SSoT・編集源)

| ページ | 役割 |
|---|---|
| 🏠 Home | Claude起点ページ。全体の入口 |
| 🤖 AI連携・ツール引継ぎ憲章 | 全AI共通の横断行動原則 |
| ⚠️ Claude行動教訓ログ | 過去の失敗事例と禁則 |
| 📋 開発セッション終了時テンプレート | セッション終了時のチェックリスト |
| 🧭 AIエージェント運用ガイド(本ページ) | 現場での迷子防止 |
| 📐 Claude Code 自律実行パターン集 | 実証された自律実行の型 |
| 📦 各プロジェクトページ | 進行中プロジェクトのSSoT |

### 🌍 GitHub Public(本リポジトリ・AI共通配信)

`ironborn365/ai-governance` 配下に、Notion正典の読み取り用ミラー。Notion未接続のAI(Gemini等)からのアクセス手段。

### 💾 Obsidian Vault(`C:\Users\coton\my-vault`)

| パス | 役割 |
|---|---|
| `CLAUDE.md` | Vault作業時のスキーマ(80行制約) |
| `wiki/ai-handoff-charter.md` | 憲章ミラー(Vault内参照用) |
| `wiki/gemini-bootstrap-prompt.md` | Gemini向けコピペプロンプト |
| `raw/` | 元素材(AIは読むが書き換えない) |
| `wiki/` | AI生成知識 |

### 🛠 開発プロジェクト(`~/dev/my-apps/{project}/`)

| パス | 役割 |
|---|---|
| `CLAUDE.md` | プロジェクト固有のハーネス |
| `.claude/agents/` | プロジェクト固有subagent |
| `LESSONS.md` / `.cursorrules` | プロジェクト固有のルール |

### 🌍 グローバル(`~/.claude/`)

| パス | 役割 |
|---|---|
| `CLAUDE.md` | (現状未配置)横断的指針の配置候補 |
| `agents/` | (現状未配置)再利用可能なsubagent候補 |
| `projects/*/memory/` | Claude Codeのfeedback型・project型メモリ |

---

## 3. Claude Code の起動ディレクトリ判断

Claude Code を起動する cwd によって読み込まれるコンテキストが変わる。**最も誤解されやすい点。**

### 判断フロー

```
作業対象は何?
├── 特定の開発プロジェクト(例: evernote-migration)
│   → cd ~/dev/my-apps/{project}/  → claude
│   読まれるもの:
│     - {project}/CLAUDE.md
│     - {project}/.claude/agents/*
│     - 親 ~/dev/my-apps/CLAUDE.md(あれば)
│     - グローバル ~/.claude/CLAUDE.md
│
├── Vault全体の作業(raw/wiki操作、Ingest/Compile等)
│   → cd C:\Users\coton\my-vault\  → claude
│   読まれるもの:
│     - my-vault/CLAUDE.md(Vaultスキーマ)
│     - my-vault/.claude/agents/*(将来配置予定)
│     - グローバル ~/.claude/CLAUDE.md
│   ⚠️ 注意: 開発プロジェクトのsubagentは読まれない
│
├── プロジェクト横断・どこで動かすか不明
│   → cd ~/  または用途に応じた場所
│   読まれるもの:
│     - グローバル ~/.claude/CLAUDE.md のみ
│
└── 他のAIに任せた方がよい
    → 引継ぎ提案(憲章原則2)
```

### CLAUDE.md ロード優先度

**深い階層が浅い階層に勝つ**(競合時)。
例: `~/dev/my-apps/CLAUDE.md` と `~/dev/my-apps/evernote-migration/CLAUDE.md` で矛盾があれば、**evernote-migration側が勝つ**。

### 起動時の必須確認コマンド

```bash
pwd                  # 自分はどこにいる?
ls CLAUDE.md         # 期待するハーネスがあるか?
ls .claude/agents/   # 期待するsubagentがあるか?
```

---

## 4. コンテキスト漏れ防止チェックリスト

### Claude Code 起動時

- [ ] cwd は意図したディレクトリか?(`pwd`)
- [ ] 期待する CLAUDE.md がそこにあるか?
- [ ] 期待する subagent が読まれているか?(`/agents`または起動バナー)
- [ ] グローバル `~/.claude/CLAUDE.md` と矛盾していないか?
- [ ] プロジェクトメモリ(`~/.claude/projects/*/memory/`)に古い情報が残っていないか?

### Cowork 起動時

- [ ] グローバル指示は最新か?(憲章URLが含まれているか)
- [ ] アクセス権限が必要なフォルダに権限が降りているか?
- [ ] HITLゲートの想定承認頻度は適切か?

### Gemini セッション開始時(人間側の作業)

- [ ] `gemini-bootstrap-prompt.md` の内容を最初に渡したか?
- [ ] 過去セッションの設定は引き継がれない前提で構築したか?

---

## 5. ありがちな迷子パターンと対処

### パターン1: 「subagentが見つからない」
- **症状:** Claude Code が `general-purpose` しか使わない、定義したsubagentが効かない
- **原因:** cwd が意図したプロジェクト外
- **対処:** `pwd` 確認 → 必要なら `cd`

### パターン2: 「CLAUDE.md の指針が効いていない」
- **症状:** 教訓に書いた禁則を犯す、手順を無視する
- **原因:** 別ディレクトリで起動 / CLAUDE.md が長すぎる(200行超)
- **対処:** cwd 確認 / ファイルサイズ確認 / `~/.claude/CLAUDE.md` にも配置

### パターン3: 「Notionに書いた指針が反映されない」
- **症状:** Claudeが憲章を踏まえずに行動
- **原因:** Notion MCP接続なし / memoryに登録なし / 単純な読み忘れ
- **対処:** MCP接続確認 / 開始時の自問Q2を実施 / memoryへポインタ追加 / **GitHub raw URL経由で取得**

### パターン4: 「複数AIで同じ作業を重複」
- **症状:** ClaudeとClaude Codeが同じファイルを別方法で更新
- **原因:** SSoT共有不足 / 引継ぎ曖昧
- **対処:** 憲章原則2に従い明確に引継ぐ / SSoTをNotionに固定

### パターン5: 「Geminiが憲章を意識しない」
- **症状:** Geminiが引継ぎ提案をしない / Notion参照を試みる(できない)
- **原因:** ブートストラップ・プロンプトを渡していない
- **対処:** 毎セッションでコピペが必要(または GitHub raw URL を渡す)

### パターン6: 「同名異物で混乱」
- **症状:** 「Dispatch」「ベース」「skill」など同名異物の取り違え
- **原因:** 固有名詞の文脈確認不足(教訓#004)
- **対処:** 固有名詞は文脈・機能・製品まで揃えて確認

### パターン7: 「MCPが切れた」
- **症状:** Notion MCP等が突然使えなくなる
- **原因:** OAuth失効(claude.ai経由の配信が組織変更等で切れる)
- **対処:** 「Claude Code 自律実行パターン集」§1, §9参照、永続トークン併用が構造的解決

---

## 6. 新規プロジェクト・タスク開始時のテンプレート

1. **コンテキスト確認**(上記Q1-Q4)
2. **関連リソース把握**:
   - Notion Home → 該当プロジェクトページ
   - 教訓ログを最新で参照
   - 憲章で自分の役割確認
3. **計画提示**(HITLゲート1: ユーザー承認)
4. **段階実行**(中間成果物ごとにレビュー、教訓#007)
5. **記録**:
   - 進捗 → Notion プロジェクトページ
   - 失敗 → 教訓ログ
   - 提案 → 憲章ページ「提案記録」
6. **引継ぎ判断**(常時、憲章原則2)
7. **セッション終了時**: 開発セッション終了時テンプレートに従う

---

## 7. 設計判断のフレームワーク

ファイル・指針を「どこに置くべきか」迷ったら以下を参照。

| 性質 | 置き場所 | 例 |
|---|---|---|
| 1つのプロジェクトでしか使わない | プロジェクトCLAUDE.md / `.claude/agents/` | evernote-migration の5本subagent |
| 複数プロジェクトで再利用 | `~/.claude/agents/` または `~/.claude/CLAUDE.md` | code-reviewer, doc-writer等 |
| **全AI(Claude含む)で共通** | **Notion(SSoT)+ GitHub Public(配信)** | AI連携憲章、教訓ログ、本ガイド |
| Vault固有(知識ベース運用) | Vault `CLAUDE.md` / `wiki/` | Ingest/Compileルール |
| 個人開発の固有設定(秘匿) | `CLAUDE.local.md`(gitignore対象) | API キー設定など |
| Geminiにも届けたい | Vault `wiki/` + GitHub raw URL + ブートストラッププロンプト | 憲章ミラー |

---

## 8. メンテナンスポリシー

本ガイドの更新トリガー:

- Claude Code の仕様変更(cwd挙動、subagent優先度、新機能)
- 新ツール導入(Open Cowork、Devin類似などが入る可能性)
- 迷子パターンの新規発見 → セクション5に追記
- 配置場所の変更 → セクション2に反映
- ユーザーから「迷子になった」報告があったとき(教訓ログとも連携)

更新時は **Notion正典 → 本リポジトリ → Vaultミラー → 各プロジェクトCLAUDE.md** の順で同期。

---

## 9. 各エージェントへの推奨初動

### Claude (claude.ai) が新会話で
1. ユーザーメモリ確認
2. 必要に応じて Notion Home フェッチ、または GitHub raw URL でllms.txt取得
3. 憲章ページのURLを記憶している前提で行動
4. 適切なら他ツール引継ぎ提案

### Claude Code が新セッションで
1. `pwd` でcwd確認
2. CLAUDE.md読み込み内容確認(階層的)
3. 利用可能subagent確認(`/agents`)
4. 不明点があれば実行前にユーザーに尋ねる(教訓#006)

### Cowork が新セッションで
1. グローバル指示の確認
2. 操作対象フォルダの権限確認
3. 計画提示 → 承認 → 実行のサイクル

### Gemini が新セッションで(人間が初動を支援)
1. ユーザーがブートストラッププロンプトを貼る、または `https://raw.githubusercontent.com/ironborn365/ai-governance/main/llms-full.txt` のURLを渡す
2. 憲章準拠で行動
3. 末尾に【引継ぎ提案: ツール名 or なし】を必ず付ける

---

**正典(編集元):** Notion `🧭 AIエージェント運用ガイド`
編集はNotionで行い、本ミラーは反映後にコミットすること。
