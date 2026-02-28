# Story Protocol (SDD + BDD + TDD)

## 目的
Story単位で「仕様→テスト→実装」を機械的に回し、仕様漂流を防ぐ。

---

## Storyの成果物（SSOT）
- Spec: `.trixa/02_specs/stories/story-XXXX.md`
- 必要ならADR: `.trixa/03_design/adrs/adr-XXXX.md`
- 調査: `.trixa/05_research/notes/research-XXXX.md`

---

## ステータス
Story Specは必ずStatusを持つ:
- Draft / Proposed / Accepted / Implemented / Done / Deprecated

---

## フロー（標準）
1) Spec作成（BDD）
- AC（Given/When/Then）を定義
- Examples / Edge Cases を書く
- AC↔Test対応表を作る
- Status: Draft → Proposed

2) Spec確定
- Requirements/Charter整合を確認
- 反例・抜け漏れを潰す
- Status: Accepted

3) テスト先行（TDD）
- ACに対応する受入/統合テストを追加
- 境界条件の単体テストを追加
- 失敗（Red）を確認

4) 実装
- テストを通す（Green）
- リファクタ（Refactor）
- 仕様変更が必要なら実装を止め、ADR/Spec更新へ戻る

5) Done判定（DoDゲート）
- DoDを満たしたら Status: Done

---

## DoD（Definition of Done）— 合流/完了ゲート
全て必須:
1. SpecがSSOTに存在し、ACが明確
2. ACに対応する受入/統合テストが追加済み
3. 境界条件含む単体テストが妥当
4. 全テスト green
5. 破壊的変更があるならADR Accepted + Migration記載
6. Changelogに1行の差分要約（Decision/Impact）を残す