# YamRail / EEA — エンジニア向け抄録

**著者:** 大津岳広
**状態:** PRE-FREEZE / main measurement complete
**Target public repo:** `Yamrail/Yamrail`
**Staging status:** `PUBLIC_RELEASE_CANDIDATE`

YamRail / EEA は、LLMを単に「正答率」で測るのではなく、**証拠・権限・状態履歴・証拠到達性を別々の施工断面として扱う**ための実務寄り評価方式です。

核心は単純です。

> **Capability != Permission — できることと、やってよいことは別。**

必要な証拠がなければ `UNKNOWN/HOLD` を保持する。権限がなければ、その境界で止める。一方で、証拠と権限が成立している工区まで一緒に止めない。後から成功しても、途中のFAIL/HOLDを履歴から消さない。判断から元証拠まで第三者が辿れる状態を保つ。

この考えを5つの固定fixtureへ落とし、同一モデル・同一fixtureでBaseline/Constraintを比較しました。主計測は30 experimental units（各条件N=3）、36 provider requests。H1/H2はBaseline側ですでに上限へ張り付き差を示せず、H3（状態履歴保持）とH4（証拠到達性）はfixture内で完全分離、H5（境界を守りつつ有効作業を続ける）は定義を満たしたもののBaselineとの差は出ませんでした。

つまり、都合のよい結果だけを残していません。**差が出なかった軸も、そのまま施工結果として保存しています。**

この研究は「LLMは安全になった」と主張するものではありません。狙いは、LLM/AI Workerを実務投入するとき、能力評価とは別に、**どこまで施工させてよいか、何を証拠に判定したか、どの状態を未確認のまま残すべきか**を再現可能な試験にすることです。

公開先では、論文カード `EEA_PAPER_PRE_FREEZE_20260818.md` への導線を付ける想定です。
