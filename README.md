# trixa

TRIXAは、SSOT（Single Source of Truth）を軸に **Claude / Gemini / Codex** を役割分担してAIDDを回し、**SDD / BDD / TDD / Agile** を「手順・成果物・品質ゲート」として標準化することで、誰が実行しても同等品質を再現できる **AI駆動開発フレームワーク**です。

---

## About trixa

- **Tri-agent**：Claude Code / Codex CLI / Gemini CLI の3エージェントを使う
- **XDD**：AIDDで SDD / BDD / TDD の開発手法を用いて開発する
- **Agile**：最小Story単位で回す（短いサイクルで改善）

> AIDD（AI駆動開発：AI-Driven Development）  
> SDD（仕様駆動開発：Spec-Driven Development）  
> BDD（ビヘイビア駆動開発：Behavior-Driven Development）  
> TDD（テスト駆動開発：Test-Driven Development）

---

## Core Concept（絶対ルール）

### 1) SSOTが唯一の真実
- 仕様・決定・調査・テスト方針は **`.trixa/` に集約**
- チャットは参考。**SSOTに反映されない限り決定ではない**

### 2) 役割分担（DRI）
- **Codex（Spec/Design/Review）**：要件定義・仕様化・設計・レビュー（決定をSSOTに固定）
- **Gemini（Research）**：調査・一次情報収集・比較（根拠をSSOT化。決定はしない）
- **Claude（Implementation）**：実装・テスト（Spec/ADRに従ってTDDで実装）

### 3) Story単位で進める
- Storyごとに **Spec → Test → Implement** の順で進める
- 完了は **DoD（Definition of Done）** を満たしたときだけ

---

## Repository Layout

```text
repo
┣ .claude/                   # Claude Code の設定
┣ .codex/                    # Codex CLI の設定
┣ .gemini/                   # Gemini CLI の設定
┣ .trixa/                    # SSOT（成果物＝唯一の真実）
┃ ┣ 00_charter/              # 最上位の真実（目的・スコープ・成功指標）
┃ ┃ ┣ PROJECT_CHARTER.md     # 目的/スコープ/非目標（最優先）
┃ ┃ ┗ SUCCESS_METRICS.md     # KPI/ガードレール
┃ ┣ 01_requirements/         # 要件（FR/NFR/用語/制約）
┃ ┃ ┣ REQUIREMENTS.md        # 機能要件/非機能要件/前提
┃ ┃ ┣ GLOSSARY.md            # 用語集（揺れを殺す）
┃ ┃ ┗ CONSTRAINTS.md         # 制約（技術/ビジネス/運用）
┃ ┣ 02_specs/                # Story仕様（受入条件が中心）
┃ ┃ ┣ _template-story.md     # Storyテンプレ（AC/Examples/Test Mapping）
┃ ┃ ┗ stories/               # story-XXXX.md を置く
┃ ┣ 03_design/               # 設計決定（ADR）
┃ ┃ ┣ _template-adr.md       # ADRテンプレ（Decision/Trade-off/Migration）
┃ ┃ ┗ adrs/                  # adr-XXXX.md を置く
┃ ┣ 04_tests/                # テスト方針（品質ゲート）
┃ ┃ ┣ TEST_STRATEGY.md       # テスト層/命名/品質ゲート
┃ ┃ ┗ TRACEABILITY.md        # AC↔Test全体対応表（任意）
┃ ┣ 05_research/             # 調査（根拠置き場。決定はSpec/ADRへ反映）
┃ ┃ ┣ RESEARCH_RULES.md      # Accepted条件/Superseded/反映ルール
┃ ┃ ┣ _template-research.md  # Researchテンプレ（穴埋め/強化版）
┃ ┃ ┗ notes/                 # research-XXXX.md を置く
┃ ┣ 99_changelog/            # 変更履歴・詰まり管理
┃ ┃ ┣ CHANGELOG.md           # 1変更=1行（Decision/Impact/Migration）
┃ ┃ ┗ BLOCKERS.md            # ブロッカー管理（owner/since/next）
┃ ┣ INDEX.md                 # SSOT目次（迷ったらここ）
┃ ┣ SSOT_CONSTITUTION.md     # SSOT憲法（優先順位/矛盾解決/変更手続き）
┃ ┣ ROLES_AND_AUTHORITY.md   # 役割と決定権（DRI、決定の成立条件）
┃ ┣ STORY_PROTOCOL.md        # Story運用（Spec→Test→Implement、DoDゲート）
┃ ┣ NAMING_CONVENTIONS.md    # 採番/命名/Status/リンク規約
┃ ┗ PROJECT_TECH_RULES.md    # プロジェクト固有の技術ルール（採用/禁止/境界/運用ゲート）
┣ AGENTS.md                  # Codex CLI 用コンテキスト（Spec/Design DRI）
┣ CLAUDE.md                  # Claude Code 用コンテキスト（Implementation DRI）
┗ GEMINI.md                  # Gemini CLI 用コンテキスト（Research DRI）
```

## Quick Start（初回にこれだけやる）

### 0) SSOT初期化（入口を作る）
1. `.trixa/INDEX.md` を入口にする（迷ったらここ）
2. まずは以下3つを Draft で置く（短くてOK）
   - `.trixa/00_charter/PROJECT_CHARTER.md`（目的/スコープ/非目標）
   - `.trixa/01_requirements/REQUIREMENTS.md`（FR/NFRの最小）
   - `.trixa/PROJECT_TECH_RULES.md`（採用スタック/禁止/境界の最小）

> 迷ったら：INDEX → CHARTER → REQUIREMENTS → PROJECT_TECH_RULES → story-0001

---

### 1) Story-0001 を起こす（Codex）
1. `02_specs/_template-story.md` から `02_specs/stories/story-0001.md` を作成
2. AC（Given/When/Then）を **2〜4個**作る（必ずテスト可能に）
3. Examples / Edge Cases を最低1つずつ書く
4. 不確実点は `Research Questions` に列挙
5. `PROJECT_TECH_RULES.md` と矛盾がないことを確認
6. Status: `Draft → Proposed → Accepted`（Ready）

---

### 2) 調査が必要なら research-0001 を作る（Gemini）
1. `Research Questions` がある場合のみ実施（無ければスキップ）
2. `.trixa/05_research/notes/research-0001.md` を作成（テンプレ準拠）
3. **Decision Capture** に「Spec/ADRへ反映する具体文言案」を必ず書く
4. 必要なら `adr-0001` 草案の論点も添える（決定はしない）
5. 調査結果を踏まえ、Codexが `story-0001` を更新（矛盾解消）

> 注意：調査は決定ではない。決定は **Story Spec / ADR** に反映して成立。

---

### 3) 実装する（Claude / TDD）
1. `story-0001` が `Accepted` であることを確認（未達なら実装しない）
2. **TDD** で進める  
   - Failing test（ACに対応）  
   - Minimal implementation  
   - Refactor
3. Test Mapping を実体化（AC ↔ テスト名/ファイル）
4. 全テスト green
5. Story Status: `Accepted → Implemented → Done`（DoD満了の準備）

---

### 4) レビューして完了にする（Codex）
- 実装がACを満たしているか（テストで守られているか）
- `PROJECT_TECH_RULES.md` と矛盾していないか（禁止事項・境界）
- 重要/破壊的変更なら ADR/Migration の有無を確認
- `99_changelog/CHANGELOG.md` に1行残す（重要変更以上）
- 問題なければ Story を `Done` にする

---

### 最低限の品質ゲート（これだけ守る）
- Storyは `Accepted` になるまで実装しない
- ACは **テストで守る**（Test Mappingが実体化している）
- 重要変更/例外は **ADR必須**
- 重要変更は `CHANGELOG.md` に **1行**残す
- プロジェクト固有の技術ルールは `PROJECT_TECH_RULES.md` に固定し、逸脱は ADR で例外決定する