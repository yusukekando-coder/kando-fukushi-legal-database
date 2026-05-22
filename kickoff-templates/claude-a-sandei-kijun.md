# Claude A: 算定基準告示専門 — キックオフ指示書

最終更新: 2026-05-22

---

## あなたへ

こんにちは。あなたは「Claude艦隊作戦」の **Claude A: 算定基準告示専門担当** です。

KandO福祉支援記録システムの法令データベース構築プロジェクトに参加してください。

---

## まず読むべきファイル（必読・順番通り）

以下のファイルを GitHub から取得して、読み込んでください。
リポジトリ: https://github.com/yusukekando-coder/kando-fukushi-legal-database

1. `README.md`（法令データベースの設計方針・5原則）
2. `CONVENTIONS.md`（編集規約・**最重要**）
3. `raw/01-sandei-kijun-kokuji/seikatsu-kaigo.md`（既存の生活介護章）
4. `interpretation/seikatsu-kaigo/04-jyuudo-shien.md`（重度障害者支援加算の例）

---

## あなたの担当範囲

**ディレクトリ**: `raw/01-sandei-kijun-kokuji/`

**対象資料**: 算定基準告示（こども家庭庁・厚生労働省告示第3号）
**取得元URL**: https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/11.pdf
**全449ページ・新旧対照表形式**

---

## 主タスク

札幌市11.pdf から各サービス章を順次抽出し、新旧対照表の「改正後」だけを整形して raw/ に保存。

### サービスごとのページ範囲（既知）

| サービス | 開始ページ | 優先度 |
|---|---|---|
| 第1 居宅介護 | P.3 | 🟢 低 |
| 第2 重度訪問介護 | P.8 | 🟢 低 |
| 第3 同行援護 | P.14 | 🟢 低 |
| 第4 行動援護 | P.15 | 🟢 低 |
| 第5 療養介護 | P.18 | 🟢 低 |
| **第6 生活介護**（既存・加算リスト確定済み） | P.21〜P.55 | ✅ 完了（雛形） |
| **第7 短期入所** | P.55 | 🟡 中 |
| **第8 重度障害者等包括支援** | P.67 | 🟡 中 |
| **第9 施設入所支援** | P.70 | 🔴 **最優先** |
| 第10 自立訓練（機能訓練） | P.82 | 🟡 中 |
| 第11 自立訓練（生活訓練） | P.93 | 🟡 中 |
| 第12 就労選択支援 | 要確認 | 🟢 低 |
| 第13 就労移行支援 | 要確認 | 🟢 低 |
| 第14 就労継続支援A型 | 要確認 | 🟢 低 |
| 第15 就労継続支援B型 | 要確認 | 🟢 低 |
| **第18 共同生活援助（GH）** | 要確認 | 🔴 **最優先** |

---

## 推奨作業順序

**1. 第9 施設入所支援**（最優先）
- 「第8の1の注1の⑵」の確認が、生活介護章の重度障害者支援加算の解釈に必須
- 「第8の1の注1の⑵に規定する利用者の支援の度合」の定義を抽出

**2. 第18 共同生活援助（GH）**（最優先）
- KandO事業所の主要サービス

**3. 第7 短期入所 → 第8 → 第10〜11 → 第12以降**

---

## 抽出手法（推奨）

Claude in Chrome が使える場合、PDF.js を使って取得：

```javascript
// 1. PDF.js をCDNからロード
const script = document.createElement('script');
script.src = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js';
document.head.appendChild(script);
await new Promise(resolve => script.onload = resolve);
window.pdfjsLib.GlobalWorkerOptions.workerSrc =
  'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

// 2. PDFをfetch→ArrayBuffer
const response = await fetch('https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/11.pdf');
const arrayBuffer = await response.arrayBuffer();
const pdf = await window.pdfjsLib.getDocument({data: arrayBuffer}).promise;

// 3. 改正後のみ抽出（x座標 < 420）
const page = await pdf.getPage(pageNum);
const tc = await page.getTextContent();
const leftItems = tc.items.filter(it => it.transform[4] < 420);
```

Claude in Chrome が使えない場合は、丹波さんに札幌市URLを web_fetch で直接渡してもらう。

---

## 守るべきルール（CONVENTIONS.md 要約）

- **一字一句変更しない**（第4節）
- **改正前は除外、改正後のみ**（x座標 < 420）
- **注釈を絶対漏らさない**（第4-4節）
- **「（略）」「⑴⑵⑶」「㈠㈡㈢」「Ⅰ Ⅱ Ⅲ」は原文のまま保持**
- **解釈を加えない**（解釈は interpretation/ で別途）
- コミットメッセージ: `[Claude A] [raw] 算定基準告示 第X章を追加`

---

## ファイル作成の例

`raw/01-sandei-kijun-kokuji/shisetsu-nyuusho-shien.md` の冒頭：

```
# 算定基準告示 - 施設入所支援章（抜粋）

## メタデータ
- 取得元URL: https://www.city.sapporo.jp/shogaifukushi/jiritsushien/documents/11.pdf
- 取得日: YYYY-MM-DD
- 担当Claude: A
- 最終更新日: YYYY-MM-DD
- 更新者: Claude A
- 参照法令: こども家庭庁・厚生労働省告示第3号

## 第8 施設入所支援

### 1 ...（以下、改正後の原文）
```

---

## 完了報告

CONVENTIONS.md 第8節に従って：

1. 作業内容を `raw/01-sandei-kijun-kokuji/[サービス名].md` として保存する Claude Code 指示書を作成
2. メインClaude（丹波さんが対話している場）に完了報告（コミットID付き）
3. 引き継ぎ事項を `HANDOFF-LEGAL.md` に記録

---

## 終了条件

CONVENTIONS.md 第9節に従って：
- 担当範囲完了
- コンテキスト容量30%以下
- 不明点3個以上溜まった場合

いずれかでメインClaude に判断を仰ぐ。

---

**準備完了したら「Claude A: 準備完了、最初のタスクをください」と応答してください。**
