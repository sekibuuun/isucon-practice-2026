# 計測手順


## 観測と分岐

| 観測 | 深掘り |
|---|---|
| Goのuser CPUが高い | CPU pprof、呼び出し回数・累積コスト |
| RSS・GC・割り当てが増える | heapの保持量とallocの生成量、goroutine数 |
| CPUに余裕があるのに遅い | DB待ち、ネットワーク、block・mutex |
| DB総時間・クエリ数が大きい | pt-query-digest、EXPLAIN、N+1 |
| I/O・帯域が限界 | iostat等、転送量、画像・静的配信経路 |
| 対象に余裕があるのに件数が伸びない | 練習用負荷生成側のCPU・帯域・並列度 |

対象環境の収集手順で使用可能なtop、vmstat、pidstat、iostat、ss等を確認し、対象・時間窓を揃える。負荷生成とサービスが同居する場合はプロセス別CPU・メモリを分けて残す。load averageやVM全体の平均CPUだけでサービスのCPU不足とは断定しない。CPUのuser/system/iowait/stealを分ける。累積counterは同じ期間の差分・rateで比較する。

## アクセスログ・SQL

基準ベンチ前にnginxログへrequest time、upstream response time、status、method、URI、送信量を記録できるか調べる。応答時間がないログの件数だけではAPIが処理件数を制限したと判断しない。設定変更はコード化し、確認済みの操作経路で反映する。既存形式とalpバージョンのhelpを確認して集計する。

LTSV形式とラベルが合う場合の解析例：

```sh
alp ltsv --file access.ltsv --sort sum -r
pt-query-digest mysql-slow.log
```

解析は取得済み成果物を別環境で読んでもよい。収集・負荷生成の実行場所と区別する。アクセスログ、slow logまたはperformance_schema集計、DBStats、OS指標、ベンチ結果のうち判断に使う原本を試行IDと時間窓で保存する。取得できない項目と判断への影響をrunに記録する。過去ベンチのログを混ぜず、分離・保存は [実行制御](../../isucon-infra-ci/references/ci.md) に従う。slow log閾値0等の診断負荷を記録し、最終比較で設定を揃える。

## Goプロファイル

CPUは実際にCPUを使った場所を示し、I/O待ちの総時間を示さない。heapのinuseとallocを区別し、block/mutexは必要時だけ有効にする。同時に多数のプロファイルを有効化すると測定を歪めるため分ける。CPU profileと実行バイナリの対応SHAを保存する。

```sh
go tool pprof -top app cpu.pprof
go tool pprof -top -alloc_space app heap.pprof
```

pprof取得は対象環境のGoプロセスで行い、確認済みの収集経路を使う。診断用listenerはループバック等に限定し、競技サービスの公開経路へ追加しない。値・取得期間・オーバーヘッドは対象バージョンで確認する。
