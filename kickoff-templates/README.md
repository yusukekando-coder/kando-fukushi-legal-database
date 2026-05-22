# Claude艦隊作戦・キックオフ指示書テンプレート

最終更新: 2026-05-22

---

## このディレクトリの目的

サブClaude のチャットを起動する際、最初に渡すキックオフ指示書を集約。

---

## 使い方

1. 新しい Claude チャットを開く
2. 担当に応じたテンプレートをコピー
3. チャットの最初のメッセージとして貼り付ける
4. Claude が「準備完了」と応答するまで待つ
5. 具体的な作業指示を出す

---

## テンプレート一覧

| ファイル | 担当 | 対象資料 |
|---|---|---|
| claude-a-sandei-kijun.md | Claude A: 算定基準告示専門 | 札幌市11.pdf（全449ページ） |
| claude-b-ryuui-jikou.md | Claude B: 留意事項通知専門 | 留意事項通知改正版（全450ページ） |
| claude-c-qa.md | Claude C: Q&A集専門 | Q&A Vol.1〜8 |
| claude-d-related-shourei.md | Claude D: 関連法令・省令専門 | 厚労省令第171号・関連告示 |

---

## メインClaude は別管理

メインClaude（丹波さんが常時対話している場）は、
このテンプレートを使わず、CLAUDE.md と SPEC.md と HANDOFF.md を
直接読み込ませて起動する。

---

## Claude艦隊作戦の全体像

```
丹波さん
  ↕
メインClaude（chat 0）— 戦略立案・統合・指示書作成
  ↙         ↓         ↘         ↘
Claude A   Claude B   Claude C   Claude D
算定基準   留意事項   Q&A集      関連法令
告示専門   通知専門   専門       省令専門
  ↓          ↓         ↓          ↓
raw/01-    raw/02-    raw/05-qa/  raw/06-
sandei-    ryuui-               related-
kijun/     jikou/               shourei/
  ↘          ↓         ↙          ↙
    docs/legal/interpretation/ （メインClaude が統合）
```

---

## 参考: キックオフ→作業→完了のフロー

```
1. 丹波さんがサブClaude のチャットを開く
2. キックオフ指示書を貼り付ける
3. Claude が必読ファイルを読み込む
4. 丹波さんが最初の具体的タスクを指示
5. Claude が作業（PDF取得→整形→マークダウン作成）
6. Claude が「Claude Code 指示書」を出力
7. 丹波さんが Claude Code に指示書を渡して Push
8. メインClaude に完了報告
9. docs/legal/HANDOFF-LEGAL.md に申し送り記録
```
