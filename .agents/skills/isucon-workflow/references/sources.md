# 参考サイト

対象ソフトウェアの版・競技制約を確認して使う。競技中の検索には非公開の問題内容を含めない。

| 資料 | 用途・注意 |
|---|---|
| [ISUCON2026公式レギュレーション](https://isucon.net/archives/59966826.html) | 必須条件の確認。当日マニュアルが優先 |
| [チューニングサンプル](https://github.com/tatsujin-web-performance/tatsujin-web-performance) | 実装例。版とライセンスを確認 |
| [private-isu](https://github.com/catatsuy/private-isu) | Goを含む練習題材。本番のルールや点数の代用ではない |
| [alp](https://github.com/tkuchiki/alp) | URI群の総時間・回数・分位点。ログ形式・ラベル・版を合わせる |
| [pt-query-digest](https://docs.percona.com/percona-toolkit/pt-query-digest.html) | slow logのクエリ別負担。収集区間と観測負荷を揃える |
| [Go Diagnostics](https://go.dev/doc/diagnostics) | CPU・heap・mutex・block・traceの使い分け。取得負荷に注意 |
| [Go net/http](https://pkg.go.dev/net/http#Transport) | 接続再利用・idle/全接続制限。対象Go版に合わせる |
| [Go database/sql](https://pkg.go.dev/database/sql#DBStats) | 接続プールと待ち時間の確認 |
| [MySQL EXPLAIN](https://dev.mysql.com/doc/refman/8.4/en/using-explain.html) | 実行計画分析。8.4以外の版では対応機能を再確認 |
| [nginx proxy module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html) | buffer・keepalive・cache設定。配信仕様を維持 |
| [USE Method](https://www.brendangregg.com/usemethod.html) | utilization/saturation/errorsで資源を切り分ける |
| [GitHub手動実行](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manually-run-a-workflow) | dispatchの前提とref・入力 |
| [GitHub concurrency](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency) | 環境単位の排他。pending置換・キャンセルにも注意 |
| [gh run list](https://cli.github.com/manual/gh_run_list) / [view](https://cli.github.com/manual/gh_run_view) | run ID・SHA・状態の照合 |
| [Terraform plan](https://developer.hashicorp.com/terraform/cli/commands/plan) / [backend](https://developer.hashicorp.com/terraform/language/backend) | Terraformが既存方式の場合の差分レビューとstate。秘密はGitに登録しない |
