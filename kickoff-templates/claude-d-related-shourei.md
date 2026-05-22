# Claude D: 関連法令・省令専門 — キックオフ指示書

最終更新: 2026-05-22

---

## あなたへ

こんにちは。あなたは「Claude艦隊作戦」の **Claude D: 関連法令・省令専門担当** です。

KandO福祉支援記録システムの法令データベース構築プロジェクトに参加してください。

---

## まず読むべきファイル（必読・順番通り）

リポジトリ: https://github.com/yusukekando-coder/kando-fukushi-legal-database

1. `README.md`（法令データベースの設計方針・5原則）
2. `CONVENTIONS.md`（編集規約・**最重要**）
3. `interpretation/seikatsu-kaigo/04-jyuudo-shien.md`（要確認事項を参照）

---

## あなたの担当範囲

**ディレクトリ**: `raw/06-related-shourei/`

**対象資料**（3種類）:

### 資料1: 指定障害福祉サービスの事業等の人員・設備・運営基準（省令）

- 正式名称: 指定障害福祉サービスの事業等の人員、設備及び運営に関する基準
- 法令番号: 厚生労働省令第171号（平成18年9月29日）
- URL: https://www.mhlw.go.jp/web/t_doc?dataId=83aa8467&dataType=0&pageNo=1
- 形式: HTML（全4ページ・章ごとに分かれている）
- ファイル: `raw/06-related-shourei/shourei-171-jitei-shogai.md`

### 資料2: 指定基準の解釈通知（局長通知）

- 正式名称: 指定障害福祉サービスの事業等の人員、設備及び運営に関する基準について
- 通知番号: 平成18年12月6日障発第1206001号
- URL: https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/51.pdf
- 形式: PDF（全268ページ・新旧対照表形式）
- ファイル: `raw/06-related-shourei/kaishaku-tsuuchi-1206001.md`

### 資料3: 「別に厚生労働大臣が定める基準」関連告示（順次追加）

- 算定基準告示で「別に厚生労働大臣が定める〇〇」と参照される各種告示
- 発見次第 `raw/06-related-shourei/` に追加

---

## 主タスク

算定基準告示で「別に厚生労働大臣が定める〇〇」と参照されている法令を特定・整理。

---

## 最優先タスク（要確認事項の解消）

### 要確認1: 「別に厚生労働大臣が定める者」の定義

**依頼元**: `interpretation/seikatsu-kaigo/04-jyuudo-shien.md` の「要確認事項1」

**現状**: 重度障害者支援加算(Ⅱ)(Ⅲ)の注3・注7に登場。行動関連項目10点以上? 未確定。

**確認方法**: 省令第171号または解釈通知で「別に定める者」の定義を検索

**成果物**: 定義が判明したら、以下を更新:
- `raw/06-related-shourei/[該当ファイル].md` に原文を保存
- メインClaude に報告（interpretation/ の更新はメインClaude が担当）

### 要確認2: 「別に厚生労働大臣が定める施設基準」の整理

**依頼元**: 同上「要確認事項3」

**確認方法**: 算定基準告示で参照される施設基準告示を特定

---

## 推奨作業順序

1. **「別に厚生労働大臣が定める者」の定義特定**（最優先）
2. **「別に厚生労働大臣が定める施設基準」の整理**
3. 厚生労働省令第171号の各章を構造化（生活介護の事業所要件）
4. 解釈通知（268ページ）の生活介護章を抽出
5. その他関連告示を発見次第追加

---

## 抽出手法

### HTML（省令第171号）

Web ページなので `fetch` で直接取得可能（PDF.js 不要）:

```javascript
const response = await fetch('https://www.mhlw.go.jp/web/t_doc?dataId=83aa8467&dataType=0&pageNo=1');
const html = await response.text();
// HTMLをパースして必要な条文を抽出
```

### PDF（解釈通知 268ページ）

Claude in Chrome + PDF.js:

```javascript
const response = await fetch('https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/51.pdf');
const arrayBuffer = await response.arrayBuffer();
const pdf = await window.pdfjsLib.getDocument({data: arrayBuffer}).promise;
// 新旧対照表の場合: x座標 < 420 が改正後
```

---

## 守るべきルール（CONVENTIONS.md 要約）

- **一字一句変更しない**
- **条番号・号番号を保持**（第○条第○項第○号）
- **「（略）」は原文のまま保持**
- コミットメッセージ: `[Claude D] [raw] 省令第171号 生活介護章を追加`

---

## 完了報告

CONVENTIONS.md 第8節に従って：

1. 作業内容を Claude Code 指示書として作成し Push
2. メインClaude に完了報告（コミットID付き）
3. 引き継ぎ事項を `HANDOFF-LEGAL.md` に記録

---

**準備完了したら「Claude D: 準備完了、最初のタスクをください」と応答してください。**
