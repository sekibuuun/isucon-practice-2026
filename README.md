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
$isucon-workflow を使い、練習環境の構成と操作方法を確認してGo実装を改善してください。
$isucon-measure を使い、対象環境の計測成果物からボトルネックを分析してください。
$isucon-db を使い、計測されたN+1を改善してください。
```

| スキル | 用途 |
|---|---|
| [isucon-workflow](.agents/skills/isucon-workflow/SKILL.md) | 一つのボトルネックの改善、記録、人間の採否判断 |
| [isucon-measure](.agents/skills/isucon-measure/SKILL.md) | 対象環境の計測・ログ・pprof分析 |
| [isucon-go](.agents/skills/isucon-go/SKILL.md) | Goのホットパス改善 |
| [isucon-db](.agents/skills/isucon-db/SKILL.md) | SQL・索引・接続管理 |
| [isucon-cache](.agents/skills/isucon-cache/SKILL.md) | キャッシュ整合性・生成負荷 |
| [isucon-infra-ci](.agents/skills/isucon-infra-ci/SKILL.md) | サーバー設定・構成管理・実行制御 |

一回のワークフローで、試行ごとのログ原本から制限要因を確認して一つのボトルネックを扱います。根拠が足りなければ追加計測します。候補は複数検討できますが、一度に修正するのは一候補だけです。「対象環境の確認→基準計測とログ保存→候補選定→実装・検証・セルフレビュー→計測用反映→ベンチとログ保存→修正前後の結果提示→人間の判断」と進みます。採用後は終了し、却下後は候補だけを取り消して同じボトルネックの候補選定へ戻ります。mustは有効スコアの成立を妨げる問題として修正・保持します。

アプリ・SQL・サーバー設定はコードで管理します。接続・反映・初期化・再起動・ベンチ・成果物回収には、リポジトリやユーザーが示した既存の操作経路を使います。ローカルコマンド、SSH、環境管理ツール、CIなど方式は限定せず、存在しないコマンドやAPIを推測しません。

性能評価では、サービス・DB・負荷生成の配置、割当資源、環境識別子、対象SHA、負荷条件を記録します。負荷生成がサービスと同居する練習環境も扱えますが、資源競合を含む同じ条件で比較し、公式スコアとは区別します。本番は競技サーバーと公式ベンチマーカーの正式手順を使います。

操作経路がない場合、スキルは勝手に実行環境や自動化を構築しません。TerraformやGitHub Actionsは既存構成にある場合だけ利用します。このブランチはスキルと資料の作成であり、実際の実行環境や自動化は含みません。

### 記録と再開

`docs/isucon/<ブランチ名>/` に `spec.md`（設計）、`run.md`（実装・反映・計測）、`review.md`（指摘・修正・実測判断）を残します。ブランチの `/` は階層として保持します。現在の [作成記録](docs/isucon/docs/codex-isucon-skills/spec.md) が例です。再開時は記録とGit・対象環境・進行中処理の実状態を照合します。

共通資料は同じ作業中に読み直さず、専門資料は必要な箇所だけ参照します。大量のログは集計と原本参照で扱い、計測・検証・ベンチなど必要なコマンドの実行は省きません。

### 競技中の運用と参考サイト

[ISUCON2026レギュレーション](https://isucon.net/archives/59966826.html) と当日マニュアルを必ず確認し、当日マニュアルを優先します。競技終了までリポジトリ、記録、ログ・計測成果物は非公開・チーム限定で運用してください。公開範囲の設定をこの変更で変更したわけではありません。

用途を付けた [参考サイト](.agents/skills/isucon-workflow/references/sources.md) を収録しています。スキル本文は実行手順と判断基準に絞っています。
