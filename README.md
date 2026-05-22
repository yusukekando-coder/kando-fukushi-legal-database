# kando-fukushi-legal-database

KandO福祉支援記録システムの**法令データベース**

## このリポジトリの目的

障害者総合支援法に基づく報酬告示・通知・Q&A集等の法令資料を、
KandOシステム実装の根拠資料として整然と整理する。

複数の Claude チャットが並行参照することで、効率的に法令データベースを
構築・維持する（「Claude艦隊作戦」）。

## ディレクトリ構造

```
kando-fukushi-legal-database/
├── README.md                          # このファイル
├── CONVENTIONS.md                     # 編集規約（必読）
├── kickoff-templates/                 # 各サブClaude のキックオフ指示書
│   ├── claude-a-sandei-kijun.md       # Claude A: 算定基準告示専門
│   ├── claude-b-ryuui-jikou.md        # Claude B: 留意事項通知専門
│   ├── claude-c-qa.md                 # Claude C: Q&A集専門
│   └── claude-d-related-shourei.md    # Claude D: 関連法令・省令専門
├── raw/                               # 法令原文（一字一句）
│   ├── 01-sandei-kijun-kokuji/        # 算定基準告示
│   ├── 02-ryuui-jikou-tsuuchi/        # 留意事項通知
│   ├── 05-qa/                         # Q&A集
│   └── 06-related-shourei/            # 関連法令・省令
└── interpretation/                    # KandO実装用の解釈ガイド
    └── seikatsu-kaigo/                # 生活介護
```

---

## 法令解釈の5原則

KandOシステムの法令データベース構築・解釈は、以下の5原則に従う。

### 原則1: 「データ集約」より「データ存在」を重視

加算根拠書類は業務的に自然な場所に記録すればよい。
形式的にケース記録に書き溜める必要はなく、説明を求められた際に
提出できる形でデータが存在していればよい。

### 原則2: 評価・申請は「事業所外」の役割を尊重

強度行動障害スコア等の最終評価は相談支援事業所・GH-SW・かかりつけ医が行う。
KandOシステムは「事実集約」と「意見出しの根拠データ提供」が役割。

### 原則3: 法令準拠を「強制せず、忘れにくくする」UX

強制ブロックではなくアラート・チェック機能で実装する。
事業所職員の判断を尊重しつつ、見落としを防ぐ設計。

### 原則4: 判断支援 ≠ 判断代行

加算算定の最終判断は事業所が行う。
KandOシステムは法令準拠の可能性を示すが、自治体への確認を促す警告型UIを採用。
自治体ごと・改定ごとに解釈が変わる可能性があるため。

### 原則5: 注釈こそが解釈の核心

加算告示の本文（単位数・要件）よりも、注釈（注1, 注2, ...）の方が
監査・請求実務で問題になりやすい。
注釈を本文と同じレベルで永続記録し、解釈ポイントを明示する。

---

## 各サブClaude の役割

| Claude | 担当 | キックオフ指示書 |
|---|---|---|
| Claude A | 算定基準告示専門 | [kickoff-templates/claude-a-sandei-kijun.md](kickoff-templates/claude-a-sandei-kijun.md) |
| Claude B | 留意事項通知専門 | [kickoff-templates/claude-b-ryuui-jikou.md](kickoff-templates/claude-b-ryuui-jikou.md) |
| Claude C | Q&A集専門 | [kickoff-templates/claude-c-qa.md](kickoff-templates/claude-c-qa.md) |
| Claude D | 関連法令・省令専門 | [kickoff-templates/claude-d-related-shourei.md](kickoff-templates/claude-d-related-shourei.md) |
| メインClaude | 統合・戦略判断（丹波さんと対話） | — |

詳細は [CONVENTIONS.md](CONVENTIONS.md) を参照。

---

## 関連リポジトリ

KandO福祉支援記録システム本体（Private）:  
https://github.com/yusukekando-coder/fukushi-system
