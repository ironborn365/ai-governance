# AI Governance Documents

> **2026-10-10 更新**：現行のルールは [`common-rules.md`](./common-rules.md)。以下の表の4文書と「Notion が正典」の記述は 2026-04-30 時点のもので失効。正本は各PCの `~/.claude/CLAUDE.md`。

このリポジトリはOniku個人のAIエージェント運用に関するガバナンス文書群を、**全AIツールから等しくアクセスできる中立な場所**として公開するためのもの。

## 想定読者

- Claude(claude.ai chat / Claude Code / Cowork)
- Gemini(Pro / Workspace)
- ChatGPT / GPTs
- Cursor / Codex / その他AIコーディングツール
- 将来追加されるAIアシスタント
- 上記AIに指示を出す人間

## 何が入っているか

| ファイル | 内容 | 役割 |
|---|---|---|
| [`llms.txt`](./llms.txt) | 軽量目録(各文書の1行サマリ+URL) | AIツールの入口 |
| [`llms-full.txt`](./llms-full.txt) | 全文連結 | コピペ移行・全文インジェスト用 |
| [`ai-handoff-charter.md`](./ai-handoff-charter.md) | AI連携・ツール引継ぎ憲章(行動原則) | WHAT |
| [`lessons-log.md`](./lessons-log.md) | 過去の失敗事例と禁則 | WHY-NOT |
| [`agent-operation-guide.md`](./agent-operation-guide.md) | エージェント運用ガイド(現場ナビ) | HOW-WHERE |
| [`claude-code-autonomy-patterns.md`](./claude-code-autonomy-patterns.md) | Claude Code 自律実行パターン集 | WHAT-PATTERN |

## ソース・オブ・トゥルース

正典は**Notion**(authoring)。本リポジトリは**読み取り用ミラー**。

不整合があった場合の優先順位:
1. Notion正典(編集はここで行う)
2. 本リポジトリ(配信用、Notionから手動 or スクリプト同期)

各ファイル末尾に対応Notionページへのリンクあり。

## なぜこの形か

- **AGENTS.md / llms.txt** の業界動向に整合(2026年4月時点の調査ベース)
- **Notion / Drive 障害から独立**(GitHub raw URL は最も安定なAI到達手段)
- **コンテキストロット対策**:`llms.txt` で目録のみ提供、必要時に `llms-full.txt` または個別ファイルを取得
- **公開のリスク管理**:このリポジトリは governance 文書のみ。コード・機微情報・プロジェクト固有データは含まない

## 使い方

### AIへの渡し方

新規AIセッション開始時、以下のいずれか:

```
このリポジトリの llms.txt を読んでください:
https://raw.githubusercontent.com/ironborn365/ai-governance/main/llms.txt
```

または、Geminiのように直接URLを取りにいけないAIの場合は、`llms-full.txt` の内容をコピペ。

### 個別文書を直接渡す

```
https://raw.githubusercontent.com/ironborn365/ai-governance/main/ai-handoff-charter.md
https://raw.githubusercontent.com/ironborn365/ai-governance/main/lessons-log.md
https://raw.githubusercontent.com/ironborn365/ai-governance/main/agent-operation-guide.md
https://raw.githubusercontent.com/ironborn365/ai-governance/main/claude-code-autonomy-patterns.md
```

## 更新ポリシー

- 編集はNotionで行う
- 反映タイミング: Notion更新後、本リポジトリにも反映(手動コミット or 同期スクリプト)
- 本リポジトリのコミット履歴がガバナンス文書の変更履歴になる

## ライセンス

個人運用ドキュメントのため明示的なOSSライセンスは付けない。
内容を参考にすることは自由。コピー・再配布する場合は出典明記推奨。

---

**最終更新:** 2026-04-30(初版起票)
