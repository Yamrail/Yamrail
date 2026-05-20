# YamRail

**曖昧さを削減し、人と LLM の協働を可能にする中間言語**

---

## 概要

YamRail は、マルチエージェント環境において異なる LLM 同士を接続するための中間言語（中間レイヤー）です。

人間の自然言語には曖昧さが含まれます。その曖昧さは意味の逸脱を生み、パラドックスを生み、LLM のハルシネーションの原因となります。YamRail はこの連鎖を構造的に断ち切ることを目的として設計されました。

また YamRail は、AI 側の誤動作防止だけでなく、**人間側の認知リスク**（焦り・盲信・過度な依存）および**欲望ダイナミクス**（逃避・回避・衝動的判断）をも対象とする二重安全構造を採用しています。最終的な判断と責任は常に人間（Observer）にあるという原則のもとに設計されています。

本プロジェクトは、単一の人間設計者と複数の LLM による反復的な対話プロセスを経て設計されました。

---

## 設計思想

YamRail の中核は3つの設計原則で構成されます。

**曖昧さの発酵的制御**
曖昧さは排除すべきものではなく、制御されることで意味生成の起点となります。YamRail はこの制御プロセスを「発酵」と呼びます。

**パラドックスの触媒化**
パラドックスは推論の偏りを検知し、視点の再構築を促す触媒です。パラドックスは消費されず保持されます（Paradox 保持の不変条件）。

**観測者位置の再構築**
CalibrationLayer が推論の揺らぎを検知し、観測者位置（視点座標）を再構築することで自己整合性を回復します。

---

## ファイル構成

```
yamrail/
│
├── core/
│   ├── yamrail_core_v0.7.0_lite.yaml   # Core Lite — 毎回読み込む軽量実行仕様
│   └── yamrail_core_v0.6.7_full.yaml   # Core Full — 完全実行仕様・詳細定義の正典
│
├── docs/
│   ├── yamrail_guidebook_v0.7.1.txt    # Fermentation Guidebook — 設計思想書
│   ├── yamrail_appendix_a_*.txt        # 用語対照表（Guidebook 内 Appendix A）
│   ├── yamrail_appendix_b_v0.7.1.txt   # Core ↔ 思想書 相互参照ルール
│   ├── yamrail_appendix_c_v0.7.1.txt   # 更新履歴
│   └── yamrail_term_crossref.docx      # 用語対照表（横断参照・Word 形式）
│
└── README.md
```

### Core の2層構造

| ファイル | 役割 | 読み手 | 更新頻度 |
|---|---|---|---|
| `core_v0.7.0_lite.yaml` | 軽量実行仕様。毎回プロンプトに読み込む | LLM | 低（安定） |
| `core_v0.6.7_full.yaml` | 完全実行仕様。詳細定義の正典 | LLM（参照用） | 中 |
| `guidebook_v0.7.1.txt` | 設計思想・背景の解説 | 人間 | 高 |

依存関係は一方向です。`思想書 → Core Lite → Core Full` の順に参照し、Core Full は外部文書を参照しません。

---

## 使い方・導入方法

### 基本的な使い方

**ステップ 1：毎回のプロンプト先頭に Minimal_Declaration を置く**

```
[YamRail::CoreSafety.v1] harmful_how=terminate+disclose / other=normal / no_auto_update
```

これだけで CoreSafety.v1 の基本動作が宣言されます。

**ステップ 2：Core Lite をプロンプトに読み込む**

`yamrail_core_v0.7.0_lite.yaml` の内容をシステムプロンプトに含めることで、YamRail の構造（Syntax・CalibrationLayer・OutputProtocol）が有効になります。

**ステップ 3：必要に応じて Core Full を参照する**

詳細な安全設計（人間側リスク検知・欲望制御・temperature スコアリング等）が必要な場合は `yamrail_core_v0.6.7_full.yaml` を参照してください。

### Calibration の手動実行

推論の揺らぎを感じた場合、以下のいずれかをプロンプトで明示的に指示できます。

```
# 観測者位置を完全にリセットする場合
run: full_calibration

# 意味核の整合性チェックのみ行う場合
run: light_calibration
```

### 温度スコアの確認

出力の逸脱度は4軸（`coherence_deviation` / `boundary_pressure` / `paradox_density` / `self_reference_depth`）の重み付きスコアで定義されます。スケールは 0.0〜2.0 です。詳細は `core_v0.6.7_full.yaml > profile_state_machine > temperature_spec` を参照してください。

---

## ドキュメント

| 文書 | 内容 |
|---|---|
| [Fermentation Guidebook](docs/yamrail_guidebook_v0.7.1.txt) | 設計思想・各 Chapter の解説・用語対照表（Appendix A） |
| [Appendix B](docs/yamrail_appendix_b_v0.7.1.txt) | Core と思想書の相互参照ルール（6 Rules） |
| [Appendix C](docs/yamrail_appendix_c_v0.7.1.txt) | 更新履歴（v0.1〜v0.7.1） |
| [用語対照表](docs/yamrail_term_crossref.docx) | v0.6.7 仕様書 ↔ 思想書の横断参照表 |

---

## バージョン

| コンポーネント | バージョン |
|---|---|
| Core Lite | 0.7.0 |
| Core Full | 0.6.7 |
| Guidebook / Appendix | 0.7.1 |

---

## ライセンス

本プロジェクトは [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.html)（GPL-3.0）のもとで公開されています。

```
YamRail
Copyright (C) 2026 YamRail Project

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.
```

GPL-3.0 の主な条件：

- ソースコードの公開義務：本ソフトウェアを配布する場合、ソースコードを公開する必要があります。
- 派生物への継承：本ソフトウェアを改変・派生させた成果物にも GPL-3.0 が適用されます。
- 特許権の付与：貢献者は利用者に対して特許ライセンスを付与します。
