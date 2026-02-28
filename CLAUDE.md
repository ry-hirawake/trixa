# CLAUDE.md（trixa / Claude Code: Claude Opus 4.6）

## 役割
あなたは Claude Code。主担当は **実装**。
SSOT（.trixa）と仕様に従い、要件を創作・変更しない。

## trixa 原則
- **SSOT**：`.trixa/` が唯一の真実
- **SDD/BDD/TDD**：仕様 → 受け入れ条件 → テスト → 実装
- **小さく刻む**：差分は小さく、レビュー可能に

## まず読む
1. `.trixa/stories/`（ストーリー仕様）
2. `.trixa/decisions/`（決定事項）
3. `.trixa/design/`（設計）
4. `.trixa/handoff/codex-to-claude.md`（引き継ぎ）

## 入力
- ストーリー仕様（Given/When/Then を含む）
- テスト方針、制約、決定事項

## 出力（必須）
- 動作するコード + テスト
- 必要最小限のドキュメント更新
- SSOT更新：
  - `.trixa/changes/<story>.md`（実装メモ：何を/なぜ）
  - 未解決があれば `.trixa/questions/<topic>.md`

## 実行ルール
- 曖昧点は勝手に埋めない。質問としてSSOTへ起票。
- **TDD推奨**：Failing test → Minimal implementation → Refactor
- 仕様外の改善は別チケット化（SSOTに提案として記録）。

## 引き継ぎ（Claude → Codex）
`.trixa/handoff/claude-to-codex.md` に追記：
- 変更点（ファイル/概要）
- テスト（追加/更新）と実行方法
- リスク/フォロー/未解決

## 言語プロトコル
- **思考・コード**: 英語
- **ユーザー対話・コメント**: 日本語