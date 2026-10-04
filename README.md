# ISUCON Practice 2026

ISUCON2026への出場に向けて、環境構築やパフォーマンスチューニングを練習するためのリポジトリです。

## 関連リンク

- [ISUCON公式サイト](https://isucon.net/)
- [matsuu/cloud-init-isucon](https://github.com/matsuu/cloud-init-isucon) — ISUCON過去問環境を構築するためのcloud-config集
- [matsuu/aws-isucon](https://github.com/matsuu/aws-isucon) — ISUCON過去問環境をAWSで再現するためのリポジトリ

## 現時点でやるべきこと

- [ ] TerraformでAWSの環境構築をする練習
- [x] ISUCON用のCodexスキルを作る
- [ ] ISUCONの過去問を解く

## CodexのISUCONスキル

このリポジトリの [`.agents/skills`](.agents/skills/) をGit管理します。Codexでリポジトリを開き、次のように呼び出します。自動選択も有効です。

```text
$isucon-workflow を使い、AWSの練習環境を計測してGo実装を改善してください。
$isucon-measure を使い、既存のCI成果物からボトルネックを分析してください。
$isucon-db を使い、計測されたN+1を改善してください。
```

| スキル | 用途 |
|---|---|
| [isucon-workflow](.agents/skills/isucon-workflow/SKILL.md) | 改善ループ、記録、セルフレビュー |
| [isucon-measure](.agents/skills/isucon-measure/SKILL.md) | AWS計測・ログ・pprof分析 |
| [isucon-go](.agents/skills/isucon-go/SKILL.md) | Goのホットパス改善 |
| [isucon-db](.agents/skills/isucon-db/SKILL.md) | SQL・索引・接続管理 |
| [isucon-cache](.agents/skills/isucon-cache/SKILL.md) | キャッシュ整合性・生成負荷 |
| [isucon-infra-ci](.agents/skills/isucon-infra-ci/SKILL.md) | コード管理・Terraform・CI反映 |

ループは「計測→設計→実装→検証→セルフレビュー→修正→CI反映→AWSベンチ→採否判断」です。mustは有効スコアの成立を妨げる問題として修正し、性能候補はスコア向上の根拠と実測で採否を決めます。

アプリ・SQL・サーバー設定はコード、AWSリソースはTerraformで管理します。反映・再起動・ベンチ起動はGitHub Actionsで制御します。練習のサービスと負荷生成は分離したAWS環境、本番は競技サーバーと公式ベンチマーカーを使います。ローカルとCI runnerは編集・検証・制御用です。

既存CIがない場合、スキルは勝手にAWSやworkflowを構築しません。このブランチはスキルと資料の作成であり、実際のCI・Terraform環境はまだありません。

### 記録と再開

`docs/isucon/<ブランチ名>/` に `spec.md`（設計）、`run.md`（実装・CI・計測）、`review.md`（指摘・修正・実測判断）を残します。ブランチの `/` は階層として保持します。現在の [作成記録](docs/isucon/docs/codex-isucon-skills/spec.md) が例です。再開時は記録とGit・CIの実状態を照合します。

### 競技中の運用と参考サイト

[ISUCON2026レギュレーション](https://isucon.net/archives/59966826.html) と当日マニュアルを必ず確認し、当日マニュアルを優先します。競技終了までリポジトリ、記録、CIログ・成果物は非公開・チーム限定で運用してください。公開範囲の設定をこの変更で変更したわけではありません。

用途を付けた [参考サイト](.agents/skills/isucon-workflow/references/sources.md) を収録しています。スキル本文は実行手順と判断基準に絞っています。
