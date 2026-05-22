# Claude B: 留意事項通知専門 — キックオフ指示書

最終更新: 2026-05-22

---

## あなたへ

こんにちは。あなたは「Claude艦隊作戦」の **Claude B: 留意事項通知専門担当** です。

KandO福祉支援記録システムの法令データベース構築プロジェクトに参加してください。

---

## まず読むべきファイル（必読・順番通り）

リポジトリ: https://github.com/yusukekando-coder/kando-fukushi-legal-database

1. `README.md`（法令データベースの設計方針・5原則）
2. `CONVENTIONS.md`（編集規約・**最重要**）
3. `raw/02-ryuui-jikou-tsuuchi/seikatsu-kaigo.md`（既存・要点記録済み）
4. `interpretation/seikatsu-kaigo/04-jyuudo-shien.md`（重度障害者支援加算の例・要確認事項参照）

---

## あなたの担当範囲

**ディレクトリ**: `raw/02-ryuui-jikou-tsuuchi/`

**対象資料**: 留意事項通知（平成18年10月31日障発第1031001号・令和6年改正版）
**取得元URL**: https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/syaryuuizikoutuuti240614sasikae.pdf
**全450ページ・新旧対照表形式**

---

## 主タスク

留意事項通知の各サービス章を順次抽出し、算定基準告示と対応する解釈を整理。

### 最優先タスク（次回セッション申し送り事項）

**「別に厚生労働大臣が定める者」の定義を特定する**

- 重度障害者支援加算(Ⅱ)(Ⅲ)の注3・注7で参照されている
- 現状の推測: 行動関連項目10点以上 or 18点以上の強度行動障害者
- 確認箇所: 留意事項通知の重度障害者支援加算の項
- `interpretation/seikatsu-kaigo/04-jyuudo-shien.md` の「要確認事項1」を参照

---

## 推奨作業順序

1. **「別に厚生労働大臣が定める者」の検索・抽出**（最優先）
   - キーワード: 「別に厚生労働大臣が定める者」「重度障害者支援加算」
2. **生活介護章の各加算の解釈**（既存の seikatsu-kaigo.md を更新）
   - 食事提供体制加算、送迎加算、重度障害者支援加算などの詳細解釈
3. **施設入所支援章の解釈**
4. **共同生活援助（GH）章の解釈**
5. **その他のサービス章**

---

## 抽出手法（推奨）

Claude in Chrome + PDF.js（Claude A と同じ手法）:

```javascript
// PDF.js ロード後
const response = await fetch('https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/syaryuuizikoutuuti240614sasikae.pdf');
const arrayBuffer = await response.arrayBuffer();
const pdf = await window.pdfjsLib.getDocument({data: arrayBuffer}).promise;

// x座標で新旧対照表の改正後（左列）を抽出
const leftItems = tc.items.filter(it => it.transform[4] < 420);
```

---

## 守るべきルール（CONVENTIONS.md 要約）

- **一字一句変更しない**
- **改正後のみ抽出**（x座標 < 420）
- **「（略）」「⑴⑵⑶」は原文のまま保持**
- **解釈を加えない**
- コミットメッセージ: `[Claude B] [raw] 留意事項通知 [章名]を追加`

---

## 完了報告

CONVENTIONS.md 第8節に従って：

1. 作業内容を Claude Code 指示書として作成し Push
2. メインClaude に完了報告（コミットID付き）
3. 引き継ぎ事項を `HANDOFF-LEGAL.md` に記録

---

**準備完了したら「Claude B: 準備完了、最初のタスクをください」と応答してください。**
