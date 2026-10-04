---
name: isucon-workflow
description: ISUCONのGoチューニングで計測・実装・セルフレビュー・CI検証を繰り返し、ブランチ別に記録する。
---

# ISUCON改善ループ

最初に [競技ルール](references/regulations.md) と [ループ・記録・レビュー](references/loop.md) を読む。既存の依頼と許可範囲を確認し、ブランチ別記録から現在状態を復元する。

1. 仕様、対象環境、保守・初期化手順、CI、スコア計算、当日ルールを調べる。未提供の情報を推測しない。
2. AWS上の対象環境で基準値を測る。計測できなければ不足を報告し、推測で性能変更を採用しない。
3. ボトルネックと改善仮説を `spec.md` に書き、下表から必要なスキルだけを読む。
4. コード変更と検証を行い `run.md` に記録する。実装後は必ずセルフレビューし `review.md` を更新する。
5. mustを修正し、根拠のある性能候補をCI経由で反映・計測する。採用・取り消し・再計測を選ぶ。
6. ボトルネックを再観測し、許可範囲・停止条件・残り時間内で続ける。

| 観測・作業 | 読むスキル |
|---|---|
| 根拠不足、アクセスログ・OS・プロファイル | [isucon-measure](../isucon-measure/SKILL.md) |
| Go CPU、割り当て、ロック、HTTP、テンプレート | [isucon-go](../isucon-go/SKILL.md) |
| SQL総時間、N+1、実行計画、接続待ち | [isucon-db](../isucon-db/SKILL.md) |
| キャッシュ対象・整合性・重複計算 | [isucon-cache](../isucon-cache/SKILL.md) |
| nginx、OS、Terraform、CI・再起動 | [isucon-infra-ci](../isucon-infra-ci/SKILL.md) |

同じCodexがスキルを読み分ける。独自ハーネス・別エージェントは不要。大量ログは集計と原本参照で扱う。固定指示と実行状態を分離する。

競技終了前はCI経由で再起動・データ保持・起動順依存・ベンチ再現を確認する。最良の検証済み状態、未解消must、未検証事項を報告する。

ツールの詳細を確認する場合は [参考サイト](references/sources.md) を参照する。
